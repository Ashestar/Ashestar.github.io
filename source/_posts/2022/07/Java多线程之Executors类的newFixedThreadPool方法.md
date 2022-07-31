---
layout: pages
title: Java多线程之Executors类的newFixedThreadPool方法
date: 2022-07-27 22:50:33
tags:
  - Java
categories:
  - Java
---

# 前言

做的项目有涉及到读取 Excel 进行批量导入数据，本来是一行行单线程读取，虽然每行有一百多列，数据少的情况下倒没什么问题，但是给业务做测试时发现每次导几千条数据，这时候处理的时间就直接导致 http 请求超时，业务又要求导入数据不符合规范时将错误信息返回给前端，这时候就要想办法优化了，最简单直接的方法是直接上多线程处理。

<!--more-->

# newFixedThreadPool

经过寻求场外协助，知道 Java 有 `Executors` 类的`newFixedThreadPool`方法可以很方便地建立一个固定大小的线程池，加上 bing 的帮助，写了一个多线程处理 List 的方法。

```java
    /**
     * 使用 Executors 的 newFixedThreadPool 执行多线程处理 List 任务
     *
     * @param usage
     * @param list
     * @param biConsumer
     * @param <T>
     */
    public static <T> void fixedThreadPool(String usage, List<T> list, BiConsumer<Integer, T> biConsumer) {
        _log.info(usage + " =>  使用固定大小线程池处理List开始，线程池大小：" + threadNum);
        long startTime = System.currentTimeMillis();
        ExecutorService executorService = Executors.newFixedThreadPool(threadNum);
        int n = 0;
        for (T value: list) {
            int index = n++;
            // execute：无返回值
            // submit：返回Future对象
            executorService.execute(() -> {
                biConsumer.accept(index, value);
            });
        }
        // 停止提交
        executorService.shutdown();
        // 阻塞直到所有线程结束
        while (true) {
            if (executorService.isTerminated()) {
                long endTime = System.currentTimeMillis();
                _log.info(usage + " =>  使用固定大小线程池处理List结束，总执行时间：" + (endTime - startTime) + "ms");
                break;
            }
        }
    }
```

简单修改一下就能处理 Map 了。

```java
    /**
     * 使用 Executors 的 newFixedThreadPool 执行多线程处理 Map 任务
     *
     * @param usage
     * @param map
     * @param biConsumer
     * @param <T>
     */
    public static <U, T> void fixedThreadPool(String usage, Map<U, T> map, BiConsumer<U, T> biConsumer) {
        _log.info(usage + " =>  使用固定大小线程池处理Map开始，线程池大小：" + threadNum);
        long startTime = System.currentTimeMillis();
        ExecutorService executorService = Executors.newFixedThreadPool(threadNum);
        for (Map.Entry<U, T> entry: map.entrySet()) {
            // execute：无返回值
            // submit：返回Future对象
            executorService.execute(() -> {
                biConsumer.accept(entry.getKey(), entry.getValue());
            });
        }
        // 停止提交
        executorService.shutdown();
        // 阻塞直到所有线程结束
        while (true) {
            if (executorService.isTerminated()) {
                long endTime = System.currentTimeMillis();
                _log.info(usage + " =>  使用固定大小线程池处理Map结束，总执行时间：" + (endTime - startTime) + "ms");
                break;
            }
        }
    }
```

测试函数：

```java
    public static void main(String[] args) {
        List<Integer> list = Arrays.asList(0, 1, 2, 3);
        fixedThreadPool("test list", list, (index, value) -> {
            System.out.println("index : " + index + " -> value : " + (value + 1));
        });
    }
```

# 总结

为了更通用地使用多线程，所以我写成了这样，并且使用了 `BiConsumer`，这样可以使用两个参数的 consumer，分别是 List 的 index/Map 的 key 和对应的 value，方便处理。

一个很值得注意的问题是如果只是对`List`/`Map`做读取那用什么都可以，但如果涉及到多线程的修改，那么就要使用**线程安全**的类，比如`Vector`和`ConcurrentHashMap`。

简单使用了多线程处理之后，原本十几分钟的处理时间被压缩到一分钟之内，可以说效果显著了，当然也跟之前写的代码实在太 low 有关（现在也一样）。

还测试了自定义线程池的方法，有机会也在后面记录下来。
