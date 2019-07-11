# clean-code-typescript [![Tweet](https://img.shields.io/twitter/url/http/shields.io.svg?style=social)](https://twitter.com/intent/tweet?text=Clean%20Code%20Typescript&url=https://github.com/labs42io/clean-code-typescript)
![jaywcjlove/sb](https://jaywcjlove.github.io/sb/lang/chinese.svg)

代码整洁之道在typescript中的实现
灵感来源于 [clean-code-javascript](https://github.com/ryanmcdermott/clean-code-javascript)。

## 目录

  1. [简介](#简介)
  2. [变量](#变量)
  3. [方法](#方法)
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

### 尽量使用默认参数，而不是再进行判断

默认参数通常比再行判断更清晰。

**不推荐:**

```ts
function loadPages(count?: number) {
  const loadCount = count !== undefined ? count : 10;
  // ...
}
```

**推荐:**

```ts
function loadPages(count: number = 10) {
  // ...
}
```

**[⬆ 回到顶部](#目录)**

## 方法
### 参数 (最多两个)
限制方法参数的数量至关重要，因为它会使得您方法的测试变得更容易，参数数量超过三个就会造成过多的组合，你必须用每个单独的参数去测试大量的情况。
一个或两个参数是最理想的情况，如果可能的话应该尽量避免三个，应该去整合除此之外的任何东西。

通常如果你有两个以上的参数，说明你的方法做了太多的事情，如果不是，则用更高级的对象来作为参数。

如果你发现自己需要大量参数，则考虑使用对象。

可以使用[结构赋值](https://basarat.gitbooks.io/typescript/docs/destructuring.html)来明确方法所期望的参数属性。
这样有一些优点：
1. 当有人查看功能签名时，会立即清楚正在使用的属性。
2. 解构还克隆传递给函数的参数对象的指定原始值。 这有助于预防副作用。 注意：不会克隆从参数对象中解构的对象和数组。
3. TypeScript会警告您未使用的属性，如果没有解构，这将是不可能的。

**不推荐:**

```ts
function createMenu(title: string, body: string, buttonText: string, cancellable: boolean) {
  // ...
}

createMenu('Foo', 'Bar', 'Baz', true);
```

**推荐:**

```ts
function createMenu(options: { title: string, body: string, buttonText: string, cancellable: boolean }) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

你可以使用[类型别名](https://www.typescriptlang.org/docs/handbook/advanced-types.html#type-aliases)来进一步提高可读性:

```ts

type MenuOptions = { title: string, body: string, buttonText: string, cancellable: boolean };

function createMenu(options: MenuOptions) {
  // ...
}

createMenu({
  title: 'Foo',
  body: 'Bar',
  buttonText: 'Baz',
  cancellable: true
});
```

**[⬆ 回到顶部](#目录)**

### 每个方法只做一件事情
这是迄今为止软件工程中最重要的规则。 当一个方法执行多个操作时，它们会更加难以编写，测试和推理。 当您可以将一个函数隔离到一个动作时，它们可以轻松地重构，并且您的代码将更加清晰。 如果你除了这个之外没有别的东西，那么你将领先于许多开发者。

**不推荐:**

```ts
function emailClients(clients: Client[]) {
  clients.forEach((client) => {
    const clientRecord = database.lookup(client);
    if (clientRecord.isActive()) {
      email(client);
    }
  });
}
```

**推荐:**

```ts
function emailClients(clients: Client[]) {
  clients.filter(isActiveClient).forEach(email);
}

function isActiveClient(client: Client) {
  const clientRecord = database.lookup(client);
  return clientRecord.isActive();
}
```

**[⬆ 回到顶部](#目录)**

### 在方法名上体现要做的事情
**不推荐:**

```ts
function addToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();

// 很难从方法名中看出增加的是什么
addToDate(date, 1);
```

**推荐:**

```ts
function addMonthToDate(date: Date, month: number): Date {
  // ...
}

const date = new Date();
addMonthToDate(date, 1);
```

**[⬆ 回到顶部](#目录)**

### 方法应该只是一个级别的抽象
如果你的一个方法有多个级别的抽象，这说明这个方法做的事情太多了。必要的拆分可以使得方法更容易被重用和更容易测试。

**不推荐:**

```ts
function parseCode(code: string) {
  const REGEXES = [ /* ... */ ];
  const statements = code.split(' ');
  const tokens = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      // ...
    });
  });

  const ast = [];
  tokens.forEach((token) => {
    // lex...
  });

  ast.forEach((node) => {
    // parse...
  });
}
```

**推荐:**

```ts
const REGEXES = [ /* ... */ ];

function parseCode(code: string) {
  const tokens = tokenize(code);
  const syntaxTree = parse(tokens);

  syntaxTree.forEach((node) => {
    // parse...
  });
}

function tokenize(code: string): Token[] {
  const statements = code.split(' ');
  const tokens: Token[] = [];

  REGEXES.forEach((regex) => {
    statements.forEach((statement) => {
      tokens.push( /* ... */ );
    });
  });

  return tokens;
}

function parse(tokens: Token[]): SyntaxTree {
  const syntaxTree: SyntaxTree[] = [];
  tokens.forEach((token) => {
    syntaxTree.push( /* ... */ );
  });

  return syntaxTree;
}
```

**[⬆ 回到顶部](#目录)**

### 移除重复代码

尽量避免重复代码
重复代码是很糟糕的，因为这意味着如果你需要改某些逻辑，则必须去同时修改改多个地方。

想象一下，如果你经营者一家餐馆并需要追踪你的库存，比如你的西红柿、洋葱、大蒜、香料等，如果你有多个地方记录他们，每当他们变化了你就需要更新多个列表，如果你只有一个列表，那么你只用更新一个地方！

通常重复代码的出现是因为你有两个或者两个以上略有不同的东西，他们有很多共同之处，但是他们之间的差异迫使你需要有两个或者多个独立的方法来执行大部分相同的事情，删除重复代码意味着创建一个抽象，只需要一个方法/模块/类就可以处理这组不同的东西。

获得正确的抽象是至关重要的，这就是为什么你需要遵循[单一责任原则](#单一责任原则)，但是糟糕的抽象可能比重复代码更加糟糕，所以一定要小心，如果你能实现好的抽象，尽量去实现它，不要去做重复的事情。

**不推荐:**

```ts
function showDeveloperList(developers: Developer[]) {
  developers.forEach((developer) => {
    const expectedSalary = developer.calculateExpectedSalary();
    const experience = developer.getExperience();
    const githubLink = developer.getGithubLink();

    const data = {
      expectedSalary,
      experience,
      githubLink
    };

    render(data);
  });
}

function showManagerList(managers: Manager[]) {
  managers.forEach((manager) => {
    const expectedSalary = manager.calculateExpectedSalary();
    const experience = manager.getExperience();
    const portfolio = manager.getMBAProjects();

    const data = {
      expectedSalary,
      experience,
      portfolio
    };

    render(data);
  });
}
```

**推荐:**

```ts
class Developer {
  // ...
  getExtraDetails() {
    return {
      githubLink: this.githubLink,
    }
  }
}

class Manager {
  // ...
  getExtraDetails() {
    return {
      portfolio: this.portfolio,
    }
  }
}

function showEmployeeList(employee: Developer | Manager) {
  employee.forEach((employee) => {
    const expectedSalary = employee.calculateExpectedSalary();
    const experience = employee.getExperience();
    const extra = employee.getExtraDetails();

    const data = {
      expectedSalary,
      experience,
      extra,
    };

    render(data);
  });
}
```

**[⬆ 回到顶部](#目录)**

### 使用`Object.assign`或者解析构值来定义默认的对象

**不推荐:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  config.title = config.title || 'Foo';
  config.body = config.body || 'Bar';
  config.buttonText = config.buttonText || 'Baz';
  config.cancellable = config.cancellable !== undefined ? config.cancellable : true;

  // ...
}

createMenu({ body: 'Bar' });
```

**推荐:**

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu(config: MenuConfig) {
  const menuConfig = Object.assign({
    title: 'Foo',
    body: 'Bar',
    buttonText: 'Baz',
    cancellable: true
  }, config);

  // ...
}

createMenu({ body: 'Bar' });
```

或者，你也可以使用解析构值来实现。

```ts
type MenuConfig = { title?: string, body?: string, buttonText?: string, cancellable?: boolean };

function createMenu({ title = 'Foo', body = 'Bar', buttonText = 'Baz', cancellable = true }: MenuConfig) {
  // ...
}

createMenu({ body: 'Bar' });
```

通过显示传入`undefined`或者`null`值来避免任何副作用，也可以告诉`TypeScript`编译器不允许他们。请参阅[`--strictNullChecks`](https://www.typescriptlang.org/docs/handbook/release-notes/typescript-2-0.html#--strictnullchecks)

**[⬆ 回到顶部](#目录)**

### 不要将布尔值作为方法的参数

布尔值表示了你的方法不止做了一件事情，请根据布尔值拆分你的方法。

**不推荐:**

```ts
function createFile(name: string, temp: boolean) {
  if (temp) {
    fs.create(`./temp/${name}`);
  } else {
    fs.create(name);
  }
}
```

**推荐:**

```ts
function createTempFile(name: string) {
  createFile(`./temp/${name}`);
}

function createFile(name: string) {
  fs.create(name);
}
```

**[⬆ 回到顶部](#目录)**