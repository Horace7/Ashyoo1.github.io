---
title:  "JavaScript中数组的操作"
categories: [Array]
tags: [Array]
---
平常的项目中用到数组的地方挺多的,基本上从后台拿到的json数据不是对象就是数组

好记性不如我这24k氪金键盘啊,所以这里整理一下敲一敲常用的数组的操作<br>

不积跬步，无以至千里 加油

---

### concat
`concat 会合并两个数组`

```javascript
const arr = [5, 17, 6, 8]
const arr1 = [5, 17, 6, 8, 6, 8]

arr.concat(arr1)
// [5, 17, 6, 8, 5, 17, 6, 8, 6, 8]
```

### replace
`replace 用于在字符串中用一些字符替换另一些字符，或替换一个与正则表达式匹配的子串。`

```javascript
// 表单验证需要判断20个字符,一个汉字等于2个字符
// 正则匹配所有的汉字,然后替换成字符,可以随意定义
value.replace(/[^/x00-\xff]/g, '**').length > 20
```

### some
`匹配数组中每一个元素,如果有一个元素通过由提供的函数实现的测试,则立刻返回true,否则返回false`

```javascript
  // 可以用到的地方就很多了,比如需要判断返回的数据中存不存在某个元素,就可以用这个方法来判断
const arr = [5, 17, 6, 8]

arr.some(e => { return  e > 9 }) // true
```

### every
`匹配数组中每一个元素,如果所有元素都通过由提供的函数实现的测试,则返回true,有一个没有通过则返回false`
```javascript
const arr = [5, 17, 6, 8]

arr.every(e => { return  e > 9 }) // false
```

### filter
`筛选出数组中符合条件的元素,组成一个新的数组`
```javascript
const arr = [5, 17, 6, 8]

// 可以过滤出想要的元素再进行操作
arr.concat(arr1).filter(e => {return e > 7}) // [17, 8]
```

### map
`数组中的每个元素都调用一个提供的函数后返回一个新数组`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.map(item => item.key).filter(e => e > 1) //  [2, 3, 4]
```

### forEach
`让数组中的每一项做一件事`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.forEach(item => console.log(item))
// {key: 1, value: 10}
// {key: 1, value: 10}
// {key: 1, value: 10}
// {key: 2, value: 20}
// {key: 3, value: 30}
// {key: 4, value: 30}
```

### push
`将一个或多个元素添加到数组的末尾，并返回新数组的长度`
```javascript
const arr = [5, 17, 6, 8]

arr.push(4) // 5
```

###  includes
`方法用来判断一个数组是否包含一个指定的值,返回 true或 false`
```javascript
const arr = [5, 17, 6, 8]

arr.includes(5) true
```

### join
`将数组（或一个类数组对象）的所有元素连接到一个字符串中`
```javascript
const kvArr = [
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 1, value: 10},
    {key: 2, value: 20},
    {key: 3, value: 30},
    {key: 4, value: 30},
]

kvArr.map(item => {return item.key}).join(',')
//1,1,1,2,3,4
```
---
### 数组去重

* 第一种方法

```javascript
// new Set 中的元素只可以出现一次,返回一个新的Set对象
// Array.from()再从一个类似数组或可迭代对象中创建一个新的数组实例
Array.from(new Set(kvArr.map(item => item.key)))
```
* 第二种方法

```javascript
// 定义一个空数组 let ret = []
// include判断是否包含元素
// push 如果不包含,则push到ret
let ret = []
kvArr.map(item => item.key).map(e => {
    if (!ret.includes(e)) {
      ret.push(e)
      return ret
    }
})
ret.map(a => {
    switch(a) {
        case 1 :
          return console.log(111)
        case 2 :
          return console.log(222)
    }
})
```


