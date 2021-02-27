# 如何实现instanceof
参考文档 [MDN](https://developer.mozilla.org/zh-cn/docs/web/javascript/reference/operators/instanceof)

instanceof 运算符用于检测构造函数的 prototype 属性是否出现在某个实例对象的原型链上。

基础语法是 **object instanceof constructor**，其中 **object** 和 **constructor** 分别是“实例对象”和“构造函数”。例如：
```
const person = { name: '小小', age: 10 };
person instanceof Object // true
person instanceof String // false
```
根据 instanceof 的定义和语法可知要实现 instanceof 需要满足一下几个条件：

1. constructor（右侧参数）是一个构造函数，拥有 prototype 属性；不满足条件的都返回 false。

2. object（左侧参数）是一个实例对象。像Number、String、Boolean 这些基础数据应该直接返回 false。

3. 遍历 object 的原型链上查找 constructor 的 prototype 是否在其原型链上。

以下是伪代码实现，具体实现可参考[ECMAScript文档](https://www.ecma-international.org/wp-content/uploads/ECMA-262.pdf) （12.10.4、7.3.21、7.2.3等章节）
```
const myInstanceof = (leftArg, rightArg) => {
  if (rightArg不是Object || !rightArg.prototype === false)
    return false;

  if (leftArg不是Object，即非引用类型)
    return false;
        
  let leftArgProto = leftArg.__proto__;
  while(leftArgProto) {
    if (rightArg.prototype === leftArgProto) {
      return true;
    }
    leftArgProto = leftArgProto.__proto__
  }
    
  return false;
};
```
顺便提一下看到网上有很多人解释 instanceof 的原理都只提到了上面提到的第3点，以至于实现的函数会存在 myInstanceof(1, Number) 返回 true 的情况。这是由于 JS “自动装箱”导致的，在浏览器中输入 (1).__proto__ === Number.prototype 会返回 true ，这里的 (1) 已经被 JS 自动装箱成Number对象。但在 instanceof 操作符中 JS 是对左侧参数进行过类型判断并未自动装箱。

PS：这篇文章是在学习 instanceof 实现原理看到很多文章实现的函数都存在 myInstanceof(1, Number) === true 的情况下，个人通过学习官方文档总结的。如有错误，感谢指正。
