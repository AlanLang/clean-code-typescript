# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)
![jaywcjlove/sb](https://jaywcjlove.github.io/sb/lang/chinese.svg)

代码整洁之道在typescript中的实现
灵感来源于 [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)。

## 目录

  1. [简介](#简介)
  2. [变量](#变量)
  3. [Functions](#functions)
  4. [Objects and Data Structures](#objects-and-data-structures)
  5. [Classes](#classes)
  6. [SOLID](#solid)
  7. [Testing](#testing)
  8. [Concurrency](#concurrency)
  9. [Error Handling](#error-handling)
  10. [Formatting](#formatting)
  11. [Comments](#comments)
  12. [Translations](#translations)

## 简介

![软件质量取决于阅读代码时骂人的次数](https://www.osnews.com/images/comics/wtfm.jpg)

本文是 Robert C 所书写的关于软件工程原理的书籍《代码整洁之道》的 TypeScript版，这不是一篇风格指南，而是为了使用 TypeScript 生成可读、可复用和可重构的软件指南。
本文中并非所有的原则都必须严格遵守，其中仅有一部分原则会被普遍认同，这些仅仅是一些原则，只是致力于整洁代码的作者们多年的经验。
我们的软件工程技术只有50多年的历史，我们仍旧在学很多东西，当软件架构和架构本身一样古老时，我们更难去遵守规则，现在，就让这些指南最为评估您和您团队的 TypeScript 代码质量的炼金石吧。
此外，了解这些知识并不会让您马上成为一名优秀的软件开发人员，与代码合作多年并不意味着您不会犯错误，每一段代码都是从初稿开始，向黏土一样慢慢被塑造成最终的形状。然后，在我们与同行一起审查时，我们会慢慢去修复不完美的地方，所以不要因为改进初稿而感到挫败，让我们去打败代码吧。

**[⬆ 回到顶部](#目录)**

## 变量
### 使用有意义的变量名称
给变量取一个有意义的名称以便于帮助读者去区分变量之前的区别

**不推荐:**

```ts
function between<T>(a1: T, a2: T, a3: T): boolean {
  return a2 <= a1 && a1 <= a3;
}

```

**推荐:**

```ts
function between<T>(value: T, left: T, right: T): boolean {
  return left <= value && value <= right;
}
```

**[⬆ 回到顶部](#目录)**

### 使用容易拼读的变量名称
如果你无法拼读它，你会像一个白痴一样无法解释它

**不推荐:**

```ts
type DtaRcrd102 = {
  genymdhms: Date;
  modymdhms: Date;
  pszqint: number;
}
```

**推荐:**

```ts
type Customer = {
  generationTimestamp: Date;
  modificationTimestamp: Date;
  recordId: number;
}
```
**[⬆ 回到顶部](#目录)**

### 为同样类型的变量使用同样的词汇

**不推荐:**

```ts
function getUserInfo(): User;
function getUserDetails(): User;
function getUserData(): User;
```

**推荐:**

```ts
function getUser(): User;
```

**[⬆ 回到顶部](#目录)**

### 使用有意义的名称
相比于写代码而言，我们更多的是去阅读代码，所以你写的代码是否可读和可检索至关重要，未命名的变量会影响对我们代码里理解，并伤害读者，所以让你的变量名可检索。可以使用[TSLint](https://palantir.github.io/tslint/rules/no-magic-numbers/) 来帮助识别未命名的常量。

**不推荐:**

```ts
// 86400000 是什么鬼?
setTimeout(restart, 86400000);
```

**推荐:**

```ts
// 将它们声明为大写的命名常量.
const MILLISECONDS_IN_A_DAY = 24 * 60 * 60 * 1000;

setTimeout(restart, MILLISECONDS_IN_A_DAY);
```

**[⬆ 回到顶部](#目录)**

### 使用可解释的变量

**不推荐:**

```ts
declare const users: Map<string, User>;

for (const keyValue of users) {
}
```

**推荐:**

```ts
declare const users: Map<string, User>;

for (const [id, user] of users) {
}
```

**[⬆ 回到顶部](#目录)**

### 使用有意义的变量来避免心里映射

显示由于隐式

**不推荐:**

```ts
const u = getUser();
const s = getSubscription();
const t = charge(u, s);
```

**推荐:**

```ts
const user = getUser();
const subscription = getSubscription();
const transaction = charge(user, subscription);
```

**[⬆ 回到顶部](#目录)**

### 不要添加不必要的上下文

如果你的 class/type/object 的名称已经告诉了你，就不要要在属性里在重复写一遍。

**不推荐:**

```ts
type Car = {
  carMake: string;
  carModel: string;
  carColor: string;
}

function print(car: Car): void {
  console.log(`${car.carMake} ${car.carModel} (${car.carColor})`);
}
```

**推荐:**

```ts
type Car = {
  make: string;
  model: string;
  color: string;
}

function print(car: Car): void {
  console.log(`${car.make} ${car.model} (${car.color})`);
}
```

**[⬆ 回到顶部](#目录)**