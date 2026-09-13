---
title: "JS第三课：函数与递归"
published: 2026-09-12
updated: 2026-09-12
description: "文章"
tags: [前端]
category: 分类
draft: false
author: jia
---
# JS 第三课：函数与递归

> 本课对应练习文件：`index.html`（函数基础、参数、作用域、return）、`test.html`（课堂任务：饮料价格、计算器、阶乘、斐波那契）。

# 一、函数的概念

在数学里，函数是这样的关系：**任意一个 x 的值，都有唯一确定的 y 值与之对应**。

- `x` 是**自变量**，`y` 是**函数值**（因变量）；
- 记作 `y = f(x)`，函数值具有**确定性**；
- 计算机里的函数沿用了这个思想 → 也就是**函数式编程**。

# 二、为什么要用函数：高内聚、低耦合

| 原则       | 含义                                                         |
| ---------- | ------------------------------------------------------------ |
| **高内聚** | 代码相关性强、独立性强，一个功能封装在一起                     |
| **低耦合** | 把重复代码提取出来，用单独的函数完成，模块之间依赖少           |
| **单一职责** | 一个功能的代码只负责一个功能，是独立模块，不依赖其他模块     |

> 核心思想：**解耦合 → 函数**。把一段固定功能封装成一个函数，需要用时调用即可。

```js
// 重复代码用函数封装，避免写三遍
if (2 > 0) { test(); }
if (3 > 0) { test(); }
if (1 > 0) { test(); }

function test() {
    for (var i = 0; i < 10; i++) {
        console.log(i);
    }
}
```

> 一句话理解函数：**一个固定功能（程序段）被封装的过程**，它需要「一个入口和一个出口」——**入口就是参数，出口就是返回值（return）**。

# 三、函数声明与调用

## 1. 最基本的写法 —— 函数声明

```js
function test(参数) {
    // 函数的执行语句
}
```

**函数只有被调用，才会执行。**

```js
function test() {
    console.log('执行了');
}
test(); // 调用函数
```

## 2. 函数名的命名规则

1. 不能以数字开头；
2. 只能由字母、`_`、`$` 组成，可包含数字；
3. 建议使用**小驼峰命名法**（camelCase），复合单词如 `myWonderfulTest`。

## 3. 一个易错点：`var a = b = 1`

```js
function test() {
    var a = b = 1; // 只有 a 用 var 声明了，b 没有
    console.log(a, b); // 1 1
}
test();
// console.log(a); // 报错：a 是局部变量，函数外访问不到
console.log(b);     // 1 （b 没写 var，成了全局变量）
```

> 注意：`var a = b = 1` 等价于 `b = 1; var a = 1;`，其中 `b` 未用 `var` 声明，会**泄漏成全局变量**。

# 四、函数表达式与匿名函数

## 1. 命名函数表达式（函数字面量）

```js
var test = function test1() {
    var a = 1, b = 2;
    console.log(a, b);
    // test1(); // 函数内部可用 test1 调用自己（递归）
};
console.log(test.name); // "test1"
test();                 // 1 2
```

## 2. 匿名函数表达式

```js
var test = function () {
    var a = 1, b = 2;
    console.log(a, b);
};
```

# 五、参数：形参与实参

```js
// 形式参数（形参）：在函数定义时「形式上占位」
function test(a, b) {
    console.log(a + b);
}

// 实际参数（实参）：调用时传入的「实际值」，与形参一一对应
test(3, 5);         // 8
test('false', NaN); // 参数没有数据类型区分
```

**要点：**

- 参数**没有数据类型区分**，任何值都能作为实参传入；
- **形参和实参数量可以不相等**（多余实参可被 `arguments` 接收，缺少的形参为 `undefined`）。

# 六、arguments 对象

`arguments` 是函数内部自动提供的一个「类数组」对象，保存了调用时的**所有实参**。

```js
function test(a, b) {
    console.log(arguments);        // 实参列表 [1, 2, 4]
    console.log(arguments[1]);     // 2（第 2 个实参）
    console.log(arguments.length); // 3（实参长度）
    console.log(test.length);      // 2（形参长度）

    for (var i = 0; i < arguments.length; i++) {
        console.log(arguments[i]); // 依次输出 1、2、4
    }
}
test(1, 2, 4);
```

> 对比记忆：`arguments.length` 是**实参**个数，`函数名.length` 是**形参**个数。

## 应用：累加任意多个实参

```js
function sum() {
    var a = 0;
    for (var i = 0; i < arguments.length; i++) {
        a += arguments[i];
    }
    console.log(a);
}
sum(1, 2, 3, 4, 5, 6); // 21
```

## 函数内部可修改实参的值

```js
function test(a, b) {
    a = 3;
    console.log(arguments[0]); // 3（形参 a 改变，映射的实参同步变化）
    console.log(arguments[2]); // 7
}
test(1, 2, 7);
```

# 七、return 返回值

`return` 用于**返回结果并结束函数**，`return` 之后的代码不会执行。

```js
function test() {
    console.log("我正在执行");
    return 0;
    console.log("我执行完了就结束这个函数"); // 不会执行
}
test();
```

## 示例：结合逻辑运算符返回默认值

```js
function test(name) {
    return name || '您没有填写姓名！';
    // name 为空时是 undefined → false
    // '您没有填写姓名！' 是 true
    // || 有真则真，返回真值
}
console.log(test('程小野')); // 程小野
console.log(test());         // 您没有填写姓名！
```

# 八、作用域：全局变量与局部变量

```js
a = 1;            // 全局变量（没写 var 声明，或写在函数外）
function test1() {
    var b = 2;    // 局部变量（只在本函数内有效）
    console.log(a, b); // 1 2

    function test2() {
        var c = 3; // 局部变量
        console.log(a, b, c); // 1 2 3
    }
    test2();
    // console.log(c); // 报错：c is not defined
}
test1();
// console.log(b); // 报错：b is not defined
```

- **全局变量**：在函数外（或未用 `var` 声明）定义的变量，到处都能访问；
- **局部变量**：在函数内用 `var` 声明的变量，只在函数内部有效；
- 内层函数可以访问外层变量（作用域链），但外层**不能**访问内层变量。

# 九、递归：规律 + 出口

**递归**：函数自己调用自己。写递归的两个关键：

1. **找规律**（递推关系）；
2. **找出口**（终止条件，避免死循环）。

> 递归的本质是「**在出口处再向上逐层计算**」。

## 1. 阶乘 n!（递归）

```
n! = n × (n-1) × (n-2) × ... × 1
5! = 5 × 4!
4! = 4 × 3!
3! = 3 × 2!
2! = 2 × 1
```

```js
function fact(n) {
    if (n === 1) {
        return 1;              // 出口
    }
    return n * fact(n - 1);    // 规律
}
console.log(fact(5)); // 120
```

## 2. 斐波那契数列（递归）

斐波那契数列：`1, 1, 2, 3, 5, 8, ...`，从第 3 项起，每一项 = 前两项之和。

- 规律：`fb(n) = fb(n-1) + fb(n-2)`；
- 出口：`n <= 0` 返回 `0`，`n <= 2` 返回 `1`。

```js
function fb(n) {
    if (n <= 0) {
        return 0;
    }
    if (n <= 2) {
        return 1;
    }
    return fb(n - 1) + fb(n - 2);
}

console.log(fb(3)); // 2
```

**计算过程展开（以 n = 5 为例）：**

```
fb(5) = fb(4) + fb(3)   // 3 + 2 = 5
fb(4) = fb(3) + fb(2)   // 2 + 1 = 3
fb(3) = fb(2) + fb(1)   // 1 + 1 = 2
fb(2) = 1
fb(1) = 1
```

## 3. 斐波那契数列（for 循环迭代，非递归）

```js
var n = parseInt(window.prompt('请输入第几位：'));
if (isNaN(n)) {
    console.log('输入异常');
} else if (n <= 0) {
    console.log('输入错误');
} else {
    var n1 = 1, n2 = 1, n3;
    if (n <= 2) {
        console.log(1);
    } else {
        for (var i = 2; i < n; i++) {
            n3 = n1 + n2; // 后一项 = 前两项之和
            n1 = n2;      // 指针右移
            n2 = n3;
        }
        console.log(n3);
    }
}
```

**滚动指针示意：**

```
 1   1   2   3   5   8
n1  n2  n3
    n1  n2  n3
        n1  n2  n3
```

### 更严格的输入校验

```js
var input = window.prompt("请输入第几位：");
var n = parseInt(input);

// 严格要求输入内容整体就是数字，不能混杂字母
if (isNaN(n) || String(n) !== input) {
    console.log("输入异常");
} else if (n <= 0) {
    console.log("输入错误");
} else {
    // 斐波那契逻辑同上……
}
```

# 十、课堂任务

## 任务 1：函数返回饮料价格

> 定义函数，从 `window.prompt` 接受一个饮料名称，返回对应价格。

```js
var drink = window.prompt('请输入饮料名称');
function price(drink) {
    switch (drink) {
        case '可乐':
            document.write("可乐：¥4");
            break;
        case '雪碧':
            document.write("雪碧：¥5");
            break;
        case '矿泉水':
            document.write('矿泉水：¥2');
            break;
        default:
            document.write('没找到');
    }
}
price(drink);
```

## 任务 2：函数实现简易计算器

> 定义函数，接受一个运算符号（`+ - * / %`）和两个数，做运算并返回结果。

```js
var char = window.prompt('请输入运算符号');
function number(char) {
    switch (char) {
        case '+':
            num1 = Number(window.prompt('请输入num1:'));
            num2 = Number(window.prompt('请输入num2:'));
            document.write(num1 + num2);
            break;
        case '-':
            num1 = Number(window.prompt('请输入num1:'));
            num2 = Number(window.prompt('请输入num2:'));
            document.write(num1 - num2);
            break;
        case '%':
            num1 = Number(window.prompt('请输入num1:'));
            num2 = Number(window.prompt('请输入num2:'));
            document.write(num1 % num2);
            break;
        case '*':
            num1 = Number(window.prompt('请输入num1:'));
            num2 = Number(window.prompt('请输入num2:'));
            document.write(num1 * num2);
            break;
        case '/':
            num1 = Number(window.prompt('请输入num1:'));
            num2 = Number(window.prompt('请输入num2:'));
            document.write(num1 / num2);
            break;
        default:
            document.write("只能是+ - * % /");
    }
}
number(char);
```

> 注：以上代码里的 `num1`、`num2` 未用 `var` 声明，属于全局变量（可再优化）。题目要求「返回运算结果」，也可改成用 `return` 返回。

## 任务 3：求 n!（不能用 for 循环）

```js
function fact(n) {
    if (n === 1) {
        return 1;
    }
    return n * fact(n - 1);
}
console.log(fact(5)); // 120
```

## 任务 4：斐波那契数列第 n 位（不能用 for 循环）

```js
function fb(n) {
    if (n <= 0) {
        return 0;
    }
    if (n <= 2) {
        return 1;
    }
    return fb(n - 1) + fb(n - 2);
}
console.log(fb(3)); // 2
```

# 本课小结

- 函数是「一个入口（参数）+ 一个出口（返回值）」的封装体，**只有被调用才执行**；
- 设计原则：**高内聚、低耦合、单一职责**，用函数实现解耦；
- 函数名用**小驼峰命名**；`var a = b = 1` 会让 `b` 泄漏成全局变量；
- 形参（占位）与实参（实际值）一一对应，数量可不相等，参数无类型限制；
- `arguments` 保存所有实参：`arguments.length` 是实参个数，`函数名.length` 是形参个数；
- `return` 返回结果并结束函数；可用 `name || '默认值'` 返回默认值；
- 全局变量到处可用，局部变量只在函数内有效，内层可访问外层、外层不可访问内层；
- **递归 = 规律 + 出口**，在出口处向上逐层计算（阶乘、斐波那契数列）；
- 斐波那契既可用**递归**（简洁、但重复计算多），也可用 **for 循环 + 滚动指针**（更高效）。
