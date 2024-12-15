---
title: TypeScript学习记录
date: 2023-10-20 09:00:00
tags:
  - 前端
  - Vue
categories:
  - 前端
  - Vue
---

2023 Vue 前端开发技术与实战培训

<!-- more -->

# 准备知识点

## true or false?

```javascript
console.log(3 && 4) // 4
console.log(3 || 4) // 3
```

条件为 true 时返回最后一次的真实值。

## ES6 的 OOP 要点

- 没有公有、私有属性。
- 有属于对象的和属于类的属性。常规的函数是属于类的，this 是属于对象的。
- ES6 是没有类型的，所以类函数也没有返回值。
- 继承：子类构造函数内，需要先调用父类的构造函数。

## ES6 部分特性

1. 字面量：
2. 字符串模版：反引号

```javascript
let n = 'aaa'
console.log(`your result is ${3 || 4}, your name is ${n}`)
```

3. 解构：对数组或者 JSON 拆解，让代码更简洁

```javascript
let info = ['zhang', 18]
// let name = info[0]
// let age = info[1]
let [name, age] = info

let info2 = { name: 'zhang', age: 18 }
// let name = info2.name
let { name, age } = info2
```

4. 默认值

```javascript
function hello(name = 'es6 default') {
  // let name = name || 'traditional default';
  console.log(`hello ${name}`)
}
```

5. 不定参数：形参

```javascript
function add(a, ...x) {
  return x.reduce((accumulator, currentVal) => {
    return accumulator + currentVal
  })
}
console.log(add(1, 2, 3, 4, 5, 6)) // 20
```

6. 拓展参数：实参

```javascript
function sayHello(name1, name2, name3) {
  console.log(`hello ${name1}, hello ${name2}, hello ${name3}`)
}
let names = ['zhangsan', 'lisi', 'wangwu']
sayHello(...names)
```

7. Proxies

8. Promise

> 状态：
>
> 成功：resolved；
> 失败：rejected；
> 进行中：pending；
>
> 状态的转化：
>
> pending -> resolved，使用 resolve()；
> pending -> rejected，使用 reject()；

```javascript
function cook() {
  console.log(`cook food started`)
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(`cook food finished`)
      resolve('cooked-food')
      // reject('boom here')
    }, 2000)
  })
}

function eat(food) {
  console.log(`start eating ${food}`)
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(`stop eating ${food}`)
      resolve('empty-dishes')
    }, 2000)
  })
}

function wash(dishes) {
  console.log(`start washing ${dishes}`)
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      console.log(`stop washing ${dishes}`)
      resolve('finished')
    }, 2000)
  })
}

cook()
  .then((data) => eat(data))
  .then((data) => wash(data))
  .then((data) => console.log(`result: ${data}`))
  .catch((error) => console.log(`error: ${error}`))
```

`async`：加在 function 前可以自动返回 promise 对象；
`await`：加在调用的 promise function 前可以阻塞异步请求，变为顺序执行；

```javascript
async function func1() {
  return 'func1 return'
}
async function func2() {
  // await func1().then((data) => console.log(data));
  return 'func2 return'
}

func2().then((data) => console.log(data))

Promise.all([func1(), func2()]).then((data) => {
  console.log(data)
})
```

# Vue

## 特殊变量

1. 观察变量

```javascript
watch: {
  a: (newVal) => {
    console.log(`watch:${newVal}`)
  }
}
```

2. 实时计算变量

- 不能在 data 里声明
- 必须在 computed 里面声明
- 需要在 view 里面使用

```javascript
computed: {
}
```

## router

- 定义好组件（用户页）
- 把用户页和路由地址（Path）一一对应，这个对应的管理工作就是 router
- 再把 router 配置到 vue 对象上，一起挂载到 DOM 对象
- view 需要有 router-view 及 router-link

## vuex

- 声明全局状态
- 读：
  - getter
  - mapper 映射+Computed
- 写：
  - dispatch(action)，组件上使用
  - action(commit) -> 允许异步
  - mutation -> 同步，修改状态

## vue 组件生命周期

- 初始化阶段：beforeCreate、created、beforeMount、mounted
- 运行中：beforeUpdate、updated
- 销毁阶段：beforeDestroy、destroyed

## vue3

1. 响应式数据声明

- ref，声明基础数据类型
- reactive，声明复杂数据类型

2. setup
