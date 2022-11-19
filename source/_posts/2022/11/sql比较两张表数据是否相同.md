---
layout: pages
title: sql比较两张表数据是否相同
date: 2022-11-19 22:52:11
tags:
  - SQL
categories:
  - SQL
---

业务提出一个需求，提交某张表 A 的时候判断它与另一张表 B 的数据是否有改变，有的话提交表 A 前先提示更新数据，用程序判断简直是噩梦，只能搜索一下看能不能直接在数据库里面比较两批数据，幸好是有的。

<!-- more -->

先是找到说用 `inner join` 进行关联查询的，说是 `表 A 数据条数= 表 B 数据条数 = 表A inner join 表B 的条数` 即可认为一样，**但是要注意表 A 和表 B 里面要先分组去重**。

不过后面又看到 `minus` 操作应该可以实现操作，感觉也比较简单一点，可以对查询结果集的进行减法操作。

A minus B 就意味着将结果集 A 去除结果集 B 中所包含的所有记录后的结果，即在 A 中存在，而在 B 中不存在的记录。

对 Oracle 来说，minus 操作有两点需要注意：

- Oracle 的 minus 是按列进行比较的，所以 A 能够 minus B 的前提条件是结果集 A 和结果集 B 需要有相同的列数，且相同列索引的列具有相同的数据类型。
- Oracle 会对 minus 后的结果集进行去重，即如果 A 中原本多条相同的记录数在进行 A minus B 后将会只剩一条对应的记录。

sql 如下：

```sql
select 字段名称 from 表1 minus select 字段名称 from 表2;
```

只要 A minus B 和 B minus A 都返回空集就可以了。

# 参考文章

- [如何用 sql 比较两张表数据是否一致？](https://zhuanlan.zhihu.com/p/113617244)
- [Oracle 函数——MINUS](https://www.cnblogs.com/zuiyue_jing/p/12019766.html)
