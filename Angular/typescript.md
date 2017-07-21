# TypeScript

### 目錄

* [入門](#入門)
* [變數宣告](#變數宣告)
* [分割代入](#分割代入)
* [型別](#型別)
* [Namespace (命名空間)](#命名空間)
* [Module (模組機制)](#模組機制)
* [Interface (介面)](#介面)
* [Function (函式)](#函式)
* [Class (類別)](#類別)
* [型別兼容性](#型別兼容性)
* [通用型別](#通用型別)
* [型別查找](#型別查找)
* [Generic (泛型)](#泛型)
* [型別斷言](#型別斷言)
* [冪運算子](#冪運算子)
* [Mixin (混入)](#混入)
* [合併宣告](#合併宣告)
* [Set (似陣列)](#似陣列)
* [Map (似物件)](#似物件)
* [Proxy (代理)](#代理)
* [Reflect (反映)](#反映)
* [Promise (承諾)](#承諾)
* [Iterator (迭代器)](#迭代器)
* [Generator (產生器)](#產生器)
* [Async (非同步)](#非同步)
* [Decorator (修飾器)](#修飾器)

***

## 入門

```bash
# 安裝 ts-node 與 typescript
$ npm i ts-node typescript -g

# 建立練習檔
$ touch script.ts
```

```ts
// script.ts
var ht = 'Hello TypeScript';
console.log(ht);
```

```bash
# 執行練習檔
$ ts-node script
```

```bash
# 輸出
Hello TypeScript
```

## 變數宣告

### 一般賦值

```ts
let foo = 123;
foo;  // 123
foo = 456;
foo;  // 456
```

```ts
let foo = 123;

if (true) {
  let foo = 456;
}

foo;  // 123
```

### 單一賦值

```ts
const bar = 123;
bar;  // 123
bar = 456;  // Error
```

```ts
const foo = { bar: 123 };
foo = { bar: 456 };  // Error
foo.bar = 456;  // OK
foo;  // { bar: 456 }
```

## 分割代入

### 陣列分割代入

```ts
let [foo, bar] = [123, 456];
foo;  // 123
bar;  // 456
```

```ts
let [foo = 1] = [];
foo;  // 1
```

```ts
let a = 1;
let b = 2;

[a, b] = [b, a];

a;  // 2
b;  // 1
```

### 物件分割代入

```ts
const foo = {
  x: 'bar',
  y: 'baz'
};

let { x, y } = foo;
x;  // "bar"
y;  // "baz"
```

```ts
let key = 'foo';
let { [key]: bar } = { foo: 'baz' };

bar;  // "baz"
```

```ts
const foo = () => [1, 2, 3];

let [a, , b] = foo();

a;  // 1
b;  // 3
```

```js
const { join } = require('path');
```

## 型別

### 布林值

```ts
let foo: boolean = true;
let bar: boolean = false;
```

### 數值

```ts
let foo: number = 18;  // 十進制
```

### 字串

```ts
let myName: string = 'Hale';

// 模板字串
let sentence: string = `My name is ${myName}.`;  // My name is Hale.
```

### 陣列

```ts
const foo: number[] = [1, 2, 3];
const bar: string[] = ['a', 'b', 'c'];

// 或者

const foo: Array<number> = [3, 2, 1];
const bar: Array<string> = ['x', 'y', 'z'];
```

### 物件

```ts
const foo: object = { prop: 0 };
```

```ts
const foo: object = { a: 1, b: 1, c: 1 };
const { c, ...bar } = foo;

bar;  // {a: number, b: number};
```

### 象徵

```ts
const s1 = Symbol();
const s2 = Symbol('foo');
```

```ts
const s1 = Symbol('foo');
const s2 = Symbol('foo');

s1 === s2;  // false
```

```ts
const KEY = Symbol();
const foo = {};

foo[KEY] = 123;

foo[KEY];  // 123

// 或者

const KEY = Symbol();
const foo = {
  [KEY]: 123
};

foo[KEY];  // 123
```

### 元組

```ts
let foo: [number, string];  // 宣告一個元組型別
foo = [123, 'abc'];  // 將 foo 初始化
```

### 列舉

```ts
enum Thing {
  Foo, Bar, Baz
};

let foo: Thing = Thing.Foo;  // 0
let bar: Thing = Thing.Bar;  // 1
let baz: Thing = Thing.Baz;  // 2

enum Thing2 {
  Up = 4,
  Down,
  Left,
  Right
};

let up: Thing2 = Thing2.Up;  // 4
let down: Thing2 = Thing2.Down;  // 5
let left: Thing2 = Thing2.Left;  // 6
let right: Thing2 = Thing2.Right;  // 7

// 使用 const 宣告
const enum Thing3 (
  // ...
  // 還能使用一些一元或二元運算子
  Foo = 60 >> 2  // 0011-1100 -> 0000-1111, 15
)

enum Thing4 {
  Foo
}
let foo = Thing4.Foo;
let thing4OfFoo = Thing4[Thing4.Foo];  // Foo

// 定義宣告
declare enum Thing5 {
  Foo,
  Bar,
  Baz
}
```

### 任意值

```ts
let notSure: any = 123;
notSure = 'abc';  // OK
notSure = 'true';  // OK
```

### 空值

```ts
let foo: void = undefined;
let bar: void = null;

function baz(thing: number): void {
  this.thing = thing;
}
```

```ts
let foo: string;
foo;  // Error

let bar: string | null;
bar;  // Error

let baz: string | undefined;
baz;  // OK
```

### 型別斷言

```ts
let foo: any = 'abc';
let bar = <string>foo;  // 語法: <>
let baz: number = bar.length;  // 3

// 或者
let bar2 = foo as string;  // 語法: as
let baz2: number = bar2.length;  // 3
```

### 型別併集

```ts
let foo: string | number;

foo = 'abc';  // OK
foo = 123;  // OK

// 使用介面
// 介面用法可以參考介面章節
interface Some {
  foo();
  baz();
}

interface Thing {
  bar();
  baz();
}

function getSomeThing(): Some | Thing {
  // ...
}
let st = getSomeThing();

st.foo();  // Error
st.bar();  // Error
st.baz();  // OK
```

### 型別別名

```ts
// 型別別名
type ThingType = string | number;
let foo: ThingType;

foo = 'abc';  // OK
foo = 123;  // OK
foo = true;  // Error
```

## 命名空間

```ts
namespace Thing {
  export class Foo { }
  export class Bar { }
}

let foo = new Thing.Foo();
let bar = new Thing.Bar();
```

```ts
namespace Thing {
  export namespace Foo {
    export class Bar { }
    export class Baz { }
  }
}

let baz = new Thing.Foo.Baz()

// 等同於
import foo = Thing.Foo;
let baz = new foo.Baz();
```

### 模組機制

```ts
// foo.ts
// 導出介面
export interface Foo {
  // ...
}

// 導出變數
export const numberRegexp = /^[0-9]+$/;
```

```ts
// bar.ts
import { Foo } from './foo';

export class Bar {
  // ...
}
```

```ts
// 預設導出
export default thing;
```

```ts
// foo.ts
export class Angular2 {
  // ...
}
```

```ts
// bar.ts
import { Angular2 as Ng2 } from './foo';  // 對導入的內容重新命名

let ng2 = new Ng2();
```

```ts
// thing.d.ts
declare module 'thing-module' {
  export function foo(): void;
}
```

```ts
// module.ts
/// <reference path="thing.d.ts" />
import * as thing from 'thing-module';
```

透過 Types 安裝模組定義

```bash
$ npm i @types/node -D
```

```ts
// script.ts
import { join } from 'path';  // OK
```

## 介面

```ts
interface Foo {
  bar: number;
  baz?: string;  // 可選參數
}

function thing(foo: Foo) {
  let bar = foo.bar;
  return bar;
}

thing({ bar: 123 });  // OK
thing({ baz: 'abc' });  // Error
thing({ bar: 123, baz: 'abc' });  // OK
```

```ts
interface Thing {
  [index: number]: string;
}

let thing: Thing = ['a', 'b', 'c'];

let foo: string = thing[1];  // 'b'
```

```ts
interface Square {
  color?: string;
  width?: number;
}

function createSquare(square: Square): { color: string; area: number } {
  // ...
}

let cs = createSquare({ width: 100, opacity: 0.5 } as Square);
```

```ts
interface Foo {
  (bar: number): void;
}

const baz = (foo: Foo) => foo(123);

baz((bar: number) => console.log(bar));  // 123
```

```ts
interface Foo {
  (x: number, y: number): boolean;
}

const foo: Foo = (x, y) => {
  let result = x + y;
  if (result >= 0) {
    return false;
  } else {
    return true;
  }
};

foo(2, -4);  // true
```

## 函式

```ts
// 設定函式型別
function addition(a: number, b: number): number {
  return a + b;
}

addition(1, 1);  // 2
addition('1', '1');  // Error
```

```ts
// 匿名函式
const addition = function(a: number, b: number): number {
  return a + b;
};

addition(1, 1);  // 2
addition('1', '1');  // Error
```

```ts
const foo: object = {
  bar() {

  }
};
```

### 可選參數

```ts
const ng = function(a: string, b: string, c?: string): string {
  return `${a} ${b} ${c}`;
};

ng('Angular', 'Material', 'Firebase');  // Angular Material Firebase
ng('Angular', 'Material');  // Angular Material undefined
ng('Angular');  // Error
```

```ts
const ng = function(a: string, b: string, c?: string): string {
  if (c !== undefined) {
    return `${a} ${b} ${c}`;
  }
  return `${a} ${b}`;
};

ng('Angular', 'Material', 'Firebase');  // Angular Material Firebase
ng('Angular', 'Material');  // Angular Material
ng('Angular');  // Error
```

### 預設參數

```ts
const ng = function(a: string, b: string = 'Angular 2'): string {
  return `${a} and ${b}`;
};

ng('Angular');  // Angular and Angular 2
ng('Angular', 'Material');  // Angular and Material
```

## 類別

### 定義類別

```ts
class Foo {
  bar: number;

  constructor(x: number, y: number) {
    this.bar = x + y;
  }

  baz(): number {
    return this.bar;
  }
}

const foo = new Foo(13, 14);

foo.baz();  // 27
```

### 靜態資料屬性

```ts
class Point {
  constructor(x: number, y: number) {
    this.x = x;
    this.y = y;
  }

  static zero(): object {
    return new Point(0, 0);
  }
}

Point.zero();  // { x: 0, y: 0 }
```

### 繼承

```ts
class Foo {
  constructor() {
    // ...
  }
}

class Bar extends Foo {
  constructor() {
    super();
    // ...
  }
}
```

```ts
class A {
  constructor() {
    console.log(new.target.name);
  }
}

class B extends A {
  constructor() {
    super();
  }
}

const a = new A();  // logs "A"
const b = new B();  // logs "B"
```

### 取值器和設值器

```ts
class Foo {
  get bar(): string {
    return 'Getter: baz';
  }

  set bar(value): void {
    console.log(`Setter: ${value}`);
  }
}

const foo = new Foo();

foo.bar;  // "Getter: baz"

foo.bar = 'baz';  // Setter: baz
```

### 修飾字元

`public` 表示可以在任何定放進行操作。

`private` 表示只能在自身內部進行操作。

`protected` 表示可以在自身或子內部進行操作。

```ts
class Thing {
  public foo: string;
  private bar: string;
  protected baz: string;
}
```

```ts
class Name {
  private name: string;

  constructor(private theName: string) {
    this.name = theName;
  }
}

new Name('Hale').name;  // Error: 'name' is private
```

```ts
class Adder {
  constructor(a: number) {
    console.log(a);
  }

  public add(b: number): number {
    return b;
  }
}

let foo: Adder = new Adder(1);

console.log(foo.add(2));
```

### 作為介面

將類別當作介面使用

```ts
class Point {
  x: number;
  y: number;
}

interface Point3d extends Point {
  z: number;
}

let point3d: Point3d = {
  x: 60,
  y: 120,
  z: 180
};
```

```ts
class Foo {
  constructor(public x = 0) { }

  public getFooX(): number {
    return this.x;
  }
}

class Bar {
  constructor(public x = 0) { }

  public get barX(): number {
    return this.x;
  }
}

const foo = new Foo(123);
foo.getFooX();  // 123

const bar = new Bar(123);
bar.barX;  // 123
```

## 型別兼容性

```ts
interface Foo {
  bar: string;
}

class Thing {
  // ...
}

let thing: Foo;
// 結構性的型別
thing = new Thing();
```

### 通用型別

```ts
const ng: Ng[] = [new Angular(), new Material(), new Firebase()];
```

### 型別查找

```ts
interface Foo {
  bar: string;
  baz: number;
}

type K1 = keyof Foo;  // "bar" | "baz"
```

映射

```ts
interface Foo {
  thing: string;
}

interface Bar {
  thing: string;
}

type Foo<T> = {
  [F in keyof T]?: T[F];
};

type Bar = Foo<Foo>;
```

型別轉換

```ts
type Readonly<T> = {
  readonly [F in keyof T]: T[F];
};
```

```ts
Partial
```

```ts
Record
```

```ts
Pick
```

## 泛型

```ts
function identity<T>(arg: T): T {
  return arg;
}

identity(123);  // 123
identity('abc');  // 'abc'
identity(true);  // true
```

```ts
class Thing<T> {
  foo: Thing<T>;
  bar: Thing<T>;
}

let nt = new Thing<number>();
let st = new Thing<string>();

nt.foo = new Thing<boolean>();
```

```ts
class Thing<T, U> {
  key: T;
  value: U;
}

let thing = new Thing<string, boolean>();

thing.key = 'foo';  // OK
thing.value = true;  // OK
```

## 型別斷言

```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo = {} as Foo;
foo.bar = 123;
foo.baz = 'abc';

foo;  // { bar: 123, baz: 'abc' }
```

```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo = <Foo>{};
foo.bar = 123;
foo.baz = 'abc';

foo;  // { bar: 123, baz: 'abc' }
```

```ts
interface Foo {
  bar: number;
  baz: string;
  ts: string;
}

let foo = <Foo>{
  bar: 123,
  baz: 'abc'
};
foo.ts = 'TypeScript';

foo;  // { bar: 123, baz: 'abc', ts: 'TypeScript' }
```

```ts
interface Foo {
  bar: number;
  baz: string;
}

let foo: Foo = {
  bar: 123,
  baz: 'abc'
};

foo;  // { bar: 123, baz: 'abc' }
```

## 冪運算子

```ts
let x = 2 ** 5;  // Math.pow(2, 5)
x;  // 32
```

```ts
let x = 2 * 5 ** 2;
x;  // 2 * 25 = 50
```

```ts
let x = -(2 ** 3);
x;  // -8

// 或者
let y = (-2) ** 3;
y;  // -8

// 錯誤
let z = -2 ** 3;
z;  // Error
```

## 混入

```ts
class Disposable {
  isDisposed: boolean;
  dispose() {
    this.isDisposed = true;
  }
}

class Activatable {
  isActive: boolean;
  activate() {
    this.isActive = true;
  }
  deactivate() {
    this.isActive = false;
  }
}

class SmartObject implements Disposable, Activatable { }
```

```ts
interface IFoo {
  add(): number;
}

class Foo implements IFoo {
  public add() {  // 混入後，就不用再給型別了
    return 123;
  }
}
```

## 合併宣告

```ts
// 合併介面
interface Box {
  height: number;
  width: number;
}

interface Box {
  scale: number;
}

const box: Box = { height: 5, width: 6, scale: 10 };
```

## 似陣列

## 似物件

## 代理

```ts
const proxy = new Proxy(<TARGET>, <HANDLER>);
```

攔截讀取屬性值

```ts
const proxy = new Proxy({}, {
  foo(target, property) {
    return 99;
  }
});

proxy.thing;  // 99
```

## 反映

```ts
const thing: object = {
  foo: 1,
  get bar() {
    return this.foo;
  },
}

Reflect.get(thing, 'foo');  // 1
Reflect.get(thing, 'bar');  // 1
```

## 承諾

```ts
const foo = () => {
  console.log(1);

  const bar = new Promise((resolve, reject) => {
    setTimeout(() => resolve(console.log(2)), 0);
  });

  console.log(3);

  return bar;
};

foo().then(() => console.log(4));
// 1
// 3
// 2
// 4
```

平行

```ts
Promise.all([
    p1(), p2()
  ])
  .then((data) => {
    console.log(data[0]);  // p1 結果
    console.log(data[1]);  // p2 結果
  });
```

平行且競賽

```ts
Promise.race([
    p1(),  // 假設 p1 為主體
    p2()  // p2 不一定要執行，通常是 p1 的超時處理
  ])
  .then(() => {
    // ...
  });
```

錯誤處理

```ts
foo().then(() => console.log(4))
  .catch(error => console.error(error));
```

鏈接

```ts
foo().then(() => console.log(4))
  .then(() => console.log(6))
  .then(() => console.log(8))
  .catch(error => console.error(error));
```

## 迭代器

## 產生器

使用星號 `*` 宣告函式為產生器

`function* name() { ... }`

```ts
function* foo(x) {
  let y = x * (yield);  // 在這裡暫停
  return y;
};

let it = foo(2);  // 通常會已使用 `it` 來控制產生器
it.next();  // 執行 foo 函式，但會停在 yield 的地方

let result = it.next(3);  // 執行 foo 函式，這次會從暫停的地方開始
result.value;  // 2 * 3 = 6
```

如果想要平行執行多個承諾，又要讓多個承諾都解析完後，執行其它任務時，該怎麼做？

這時就必須搭配產生器的使用

```ts

```

```ts
const foo: object = {
  *bar() {
    let index: number = 0;
    while (true) yield index++;
  }
};

const it = foo.bar();
it.next().value;  // 0
it.next().value;  // 1
```

## 非同步

```ts
async function foo(x) {  // 10
  let bar = await Promise
    .resolve(x)
    .then(x => x + 2)  // 10 + 2 = 12
    .then(x => x - 3)  // 12 - 3 = 9
    .then(x => x * 4)  // 9 * 4 = 36
    .then(x => x / 5);  // 36 / 5 = 0
  return bar;
}

foo(10).then(x => console.log(x));  // 7.2
```

```ts
async function sleep(x) {
  return new Promise((resolve, reject) => {
    setTimeout(() => resolve(console.log(2)), x);
  });
}

async function bar() {
  console.log(1);
  await sleep(1000);  // 等待了一秒
  setTimeout(() => console.log(3), 1000);  // 等待了一秒，加自己的一秒
}

bar();
// 1
// 2 (一秒後打印)
// 3 (兩秒後打印)
```

```ts
async function* foo() {
  yield 1;
  await sleep(100);
  yield* [2, 3];
  yield* (async function *() {
    await sleep(100);
    yield 4;
  })();
}
```

```ts
const foo: object = {
  async bar() {
    await sleep(1000);
  }
};
```

非同步比較

```ts
function foo(x) {
  return bar(x).then(data => foo(data));
}

// vs

function* foo(x) {
  let data = yield bar(x);
  let baz = yield foo(data);
  return baz;
}

// vs

async function foo(x) {
  let data = await bar(x);
  let baz = await foo(data);
  return baz;
}

// 還有一個 Observable，如果要在現在環境使用 Observable，需要透過 RxJS 來實現
```

## 修飾器

### 修飾類別

```ts
const Foo = (value: any) => {
  return (target: any) => {
    target.Foo = value;
    console.log(value);
  }
}

@Foo(123)
class Thing { }  // 這個類別會打印出 123
```

### 修飾屬性

```ts
const Foo = (value: any) => {
  return (target: any, key: any, descriptor: any) => {
    console.log(value);
    return console.log(target, key, descriptor);
  }
}

class Thing {
  @Foo('bar')
  baz() { }
}
```

### 修飾參數

```ts
const Foo = (target: any, key: any, index: number) => {
  console.log(key, index);
}

class Bar {
  print(@Foo baz): void {
    console.log(baz);
  }
}

new Bar().print(123);
```