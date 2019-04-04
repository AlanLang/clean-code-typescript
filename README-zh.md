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

**[⬆ 回到顶部](#table-of-contents)**

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

**[⬆ back to top](#table-of-contents)**