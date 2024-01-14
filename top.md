# Typescript

- typescriptはjavascriptのスーパーセット
- 拡張版であり、javascriptのコードをtypescriptとして扱うことができる
- スーパーセットのメリット
    - 学習しやすい
    - 資産が活かせる
    - 移行がしやすい

## 静的な検査

- typescriptは静的型付けである
    - javascriptは動的型付け
- typescriptは実行前にバグがあるか確認できる
- 開発の効率を上げ、安心感や品質を高めることができる

## 検査の仕組み

- 型システムは、データに型を設けることで、代入できる値や操作に制限をかけ、安全さを保証するシステムである
- 変数にどのような値を代入できるかを示したものを型という
- 型注釈を用いて型を指定し、それによる検査を行う
- 型が明白な場合は型推論によって自動的に型が判定される
    - 計算結果など
- そうすることで記述量を減らす
- Typescriptを実行する前に、Javascriptに変換する
- この変換をコンパイルという
- コンパイル時に検査する
- 型によってコードの理解が助かる
- 実行前にエラーがわかるのでリファクタリングが楽
- エラー検知や自動補完などをエディターが行えるようになる

## 基本的な型

### プリミティブな型

- `boolean`:真偽値
- `number`:数値
- `string`:文字列
- `bigint`:大きな整数
- `symbol`:一意な値
- `undefined`:値が定義されていない
- `null`:値が存在しない

```typestring
const isReady: boolean = false;
const age: number = 25;
const fullName: string = "John Doe";
const bigNumber: bigint = 100n;
const uniqueSymbol: symbol = Symbol("unique");
const notDefined: undefined = undefined;
const empty: null = null;
```

### 特殊な型

- `any`:変数の値が自由。操作も自由。その分安全性は下がる
- `unknown`:代入する値が自由。操作は制限付き。その分安全。
- `void`:値が存在しないことを示す。関数が何も返さないときに使用する。
- `never`:決して何も返さないことを示す。エラーや無限ループの戻り値として使用する。

```typescript
const a: any = 100; // 代入できる
console.log(a * 3); // 操作もできる
 
const x: unknown = 100; // 代入はできる
console.log(x * 3); // 操作はできない
 
// 戻り値のない関数
function doSomething(): void {}
 
// 戻り値を返すことがありえない関数
function throwError(): never {
  throw new Error();
}
```

- 型エイリアスは既存の方を組み合わせて新たな方を作る手法で、これによる可読性が高まる

```typescript
type StringOrNumber = string | number;
let value: StringOrNumber;
value = "hello"; // string型が代入可能
value = 123; // number型も代入可能
```

- 構造的部分型をtypescriptは採用していて、代入の互換性の観点での可否によって判定する

```typescript
type Summary = { name: string };
type Detail = { name: string; age: number };
 
const johnDetail: Detail = { name: "John", age: 28 };
const summary: Summary = johnDetail; // 代入できる。構造的部分型として互換があるため
 
const johnSummary: Summary = { name: "John" };
const detail: Detail = johnSummary; // 代入できない。構造的部分型として互換がない（ageを含まないため）
```

## 配列

- 配列リテラル`[]`を用いる

```typescript
const numbers = [1, 2, 3];
```

- 型注釈には`型名[]`または`Array<型名>`を用いる

```typescript
let numbers: number[];
let strings: Array<string>;
```

- 配列の値を指定するときは0からはじまるインデックスを用いる

```typescript
const colors = ["red", "green", "blue"];
console.log(colors[0]);
colors[1] = "yellow";
console.log(colors);
```

- 読み取り専用の配列を作ることもできる
- `readonly 型名[]`や`ReadonlyArray<型名>`で書ける

```typescript
const numbers: readonly number[] = [1, 2, 3];
const strings: ReadonlyArray<string> = ["hello", "world"];
 
numbers[0] = 4; // 値を変更できない
strings.push("!"); // 要素を追加できない
```

- `for ... of ...`で配列のループが書ける

```typescript
const numbers = [1, 2, 3];
 
for (const num of numbers) {
  console.log(num); // 1, 2, 3と出力される
}
```

- 配列の要素や型を指定したタプル型がある

```typescript
let tuple: [string, number];
tuple = ["hello", 10]; // 代入できる
tuple = [10, "hello"]; // 順序が正しくないため、代入できない
tuple = ["hello", 10, "world"]; // 要素が多すぎるため代入できない
```

- アクセス方法は配列と同じインデックス

```typescript
const tuple: [string, number] = ["hello", 10];
console.log(tuple[0]);
```

## オブジェクト

- オブジェクトリテラル`{}`を用いて、`{プロパティーキー:値, ...}`の形で定義できる。

```typescript
const john = { name: "John", age: 20 };
```

- ドット`.`で値にアクセスする。

```typescript
console.log(john.name);
```

- 型注釈は`{プロパティ1: 型1, プロパティ2: 型2, ...}`の形で記述する

```typescript
let obj: { name: string; age: number };
```

- キーの前に`readonly`をつけると、代入できなくなる。

```typescript
let obj: { readonly name: string; age: number };
obj = { name: "John", age: 20 };
obj.name = "Tom"; // エラーが発生
```

- オプショナルプロパティー`?`をキーのあとにつけたプロパティは省略可能になる

```typescript
let obj: { name: string; age?: number };
obj = { name: "John" }; // `age`プロパティがなくてもエラーにならない
```


- 関数をオブジェクトに定義できる(オブジェクトメソッド)

```typescript
const obj = {
  a: 1,
  b: 2,
  sum(): number {
    return this.a + this.b;
  },
};
console.log(obj.sum());
```

- インデックス型を利用して任意のキーを取得する
- 型注釈は`[キー名: キーの型]: 値の型`の形で定義する

```typescript
let obj: { [key: string]: number };
obj = { key1: 1, key2: 2 };
console.log(obj["key1"]);
console.log(obj["key2"]);
```

- 変数を用いて定義するとオブジェクトの書き方を省略できる

```typescript
const name = "John";
const age = 20;
const obj = { name, age };
console.log(obj);
```

- `?.`演算子(オプショナルチェーン)でプロパティがない場合も安全にアクセスできる

```typescript
function printLength(obj: { a?: string }) {
  console.log(obj.a?.length);
}
printLength({ a: "hello" });
> 5
printLength({});
> undefined
```

## Mapオブジェクト

- キーと対応する値を紐付けるオブジェクト
- キーはオブジェクトも含め任意の値を利用できる

```typescript
const map = new Map();
map.set("name", "John");
map.set("age", "20");
 
console.log(map.get("name"));
```

- 型注釈は`Map<キーの方, 値の型>`のように記述する・

```typescript
let people: Map<string, number>;
```

- `for..of`でループをする。このとき各要素はキーと値の配列となる。
- 要素を追加した順に呼び出される

```typescript
for (const [key, value] of map) {
  console.log(key, value);
}
```

## Setオブジェクト

- どんな要素も追加できるが、同じ要素は追加されない

```typescript
const set = new Set();
set.add(1);
set.add(2);
set.add(2); // 同じ値は追加されない。
 
console.log(set);
```

- 型注釈は`Set<要素の型>`の形で記述する。

```typescript
let numSet: Set<number>;
```

- `for...of`でループされ、順序はaddした順になる

```typescript
for (const value of set) {
  console.log(value);
}
```

## 列挙型

- 一連の文字列または数字の集まり
- `enum`キーワードで定義する

```typescript
enum Color {
  Red,
  Green,
  Blue,
}
```

- 文字列または数値で指定できる

```typescript
enum Color {
  Red = "red",
  Green = "green",
  Blue = "blue",
}
```

- 各値にアクセスするにはドット演算子を用いる

```typescript
const myColor: Color = Color.Red;
```

## ユニオン型

- 複数の型を同じ変数で扱える
- `型1 | 型2 | ...`のような書き方ができる

```typescript
let value: boolean | number;
value = true; // 代入できる
value = 100; // 代入できる
```

- 共通のリテラル型(特定の文字列や数値)のプロパティを持つ特別なユニオン型を判別可能なユニオン型といい、そのプロパティで判別する

```typescript
type Triangle = { kind: "triangle"; base: number; height: number };
type Rectangle = { kind: "rectangle"; width: number; height: number };
type Shape = Triangle | Rectangle;
 
function getArea(shape: Shape): number {
  // 共通のプロパティkindを利用して型を判定する
  switch (shape.kind) {
    case "triangle":
      // この節ではshapeがTriangle型に絞り込まれる
      return (shape.base * shape.height) / 2;
    case "rectangle":
      //  この節ではshapeがRectangle型に絞り込まれる
      return shape.width * shape.height;
  }
}
```

## インターセクション型

- インターセクション型は複数の方を組み合わせた新たな方を定義する
- `型1 & 型2 & ...`の形式を使う
- それぞれの方が持つプロパティとメソッドを持つ

```typescript
type Octopus = { swims: boolean };
type Cat = { nightVision: boolean };
type Octocat = Octopus & Cat;
 
const octocat: Octocat = { swims: true, nightVision: true };
console.log(octocat);
```

## 分割代入

- 分割代入で、配列に値を一度に代入できる

```typescript
const [a, b] = [1, 2];
console.log(a);
console.log(b);
```

- オブジェクトのプロパティを個別の変数へ一度に代入できる

```typescript
const obj = {
  name: "John",
  age: 20,
};
 
const { name, age } = obj;
console.log(name);
console.log(age);
```

## 条件分岐

- if-else文(単純な条件分岐)

```typescript
const age: number = 20;
 
if (age >= 20) {
  console.log("You are an adult.");
} else {
  console.log("You are a minor.");
}
```

- switch文(複数の条件分岐)

```typescript
const color: string = "blue";
 
switch (color) {
  case "red":
    console.log("Color is red.");
    break;
  case "blue":
    console.log("Color is blue.");
    break;
  default:
    console.log("Color is neither red nor blue.");
}
```

## 型の絞り込み

- 条件分岐の条件にtypeofを使うことで、型が自動的に絞り込まれる

```typescript
let value: string | number;
// 50%の確率でstring型またはnumber型の値を代入する
value = Math.random() < 0.5 ? "Hello" : 100;
 
if (typeof value === "string") {
  // この節ではvalueはstring型として扱われる
  console.log(value.toUpperCase());
} else {
  // この節ではvalueはnumber型として扱われる
  console.log(value * 3);
}
```

## 関数

- アロー関数や関数宣言に型注釈をつける

### アロー関数



```typescript
const greet = (name: string): string => {
  return `Hello ${name}`;
};
 
console.log(greet("John"));
```

### 関数宣言

```typescript
function greet(name: string): string {
  return `Hello ${name}`;
}
 
console.log(greet("John"));
```

### アロー関数

- アロー関数は簡潔な構文で関数を定義する方法

- 基本的な構文

```typescript
// 通常の関数宣言
function add(x: number, y: number): number {
  return x + y;
}

// アロー関数
const addArrow = (x: number, y: number): number => x + y;
```

- 以下のような特徴を持つ

1. 簡潔な構文: 単一の式を返す場合、波括弧`{}`や`return`キーワードを省略できる。
2. `this`の挙動: アロー関数では`this`の挙動が通常の関数と異なる。アロー関数内での`this`は、関数が定義されたコンテキストから継承される。
3. 省略可能な括弧: 引数が1つの場合、引数の括弧`()`も省略可能。

### 分割代入引数

- 関数の引数に配列またはオブジェクトリテラルを展開できる

```typescript
const printCoord = ({ x, y }: { x: number; y: number }) => {
  console.log(`Coordinate is (${x}, ${y})`);
};
 
printCoord({ x: 10, y: 20 });
```

### 型ガード関数

- 型ガード関数と呼ばれる特定の型か判定する関数を用いて、型を絞り込まれることができる。

```typescript
function isString(value: any): value is string {
  return typeof value === "string";
}
 
function printLength(value: any) {
  if (isString(value)) {
    // この節ではvalueはstring型として扱われる
    console.log(value.length);
  }
}
 
printLength("hello");
```

### オプション引数

- 関数の引数には`?`をつけることで任意にできる。これをオプション引数という。

```typescript
function greet(name?: string) {
  if (name === undefined) {
    return "Hello!";
  } else {
    return `Hello ${name}!`;
  }
}
 
console.log(greet("John"));
console.log(greet());
```

- 関数の引数に`=`を使ってデフォルトの値を設定することができる。これをデフォルト引数という。

```typescript
function greet(name: string = "Mystery") {
  return `Hello ${name}!`;
}
 
console.log(greet("John"));
console.log(greet());
```

- `...`を使って任意の数の引数である残余引数を設定できる。

```typescript
function sum(...numbers: number[]) {
  return numbers.reduce((total, num) => total + num, 0);
}
 
console.log(sum(1, 2, 3, 4, 5));
```

## クラス


- JavaScriptのクラス構文はそのまま利用できる。
- `this`でアクセスできる
- `constructor`で初期化

```typescript
class Person {
  // コンストラクタで初期化
  constructor(firstName, lastName) {
    this.firstName = firstName;
    this.lastName = lastName;
  }

  // メソッド
  getFullName() {
    return `${this.firstName} ${this.lastName}`;
  }
}

// クラスのインスタンスを作成
const person = new Person("John", "Doe");

// メソッドの呼び出し
console.log(person.getFullName()); // 出力: John Doe

```

- フィールド(クラスのプロパティ)の宣言に型注釈をつけることができる

```typescript
class Person {
  name: string;
  age: number;
 
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
 
  introduce(): void {
    console.log(`My name is ${this.name} and I am ${this.age} years old.`);
  }
}
 
const john = new Person("John", 20);
john.introduce();
```

- アクセス修飾子は3つある
- アクセス修飾子によって、カプセル化と安全性を高める

```typescript
class Person {
  public name: string;
  private age: number;
 
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
 
  introduce(): void {
    console.log(`My name is ${this.name} and I am ${this.age} years old.`);
  }
}
 
const john = new Person("John", 20);
console.log(john.name); // 'John'が出力される
console.log(john.age); // エラー（privateなのでアクセスできない）
```

- `public`はデフォルトで設定されるアクセスで、どこからもアクセス可能

```typescript
class Example {
  public name: string;

  constructor(name: string) {
    this.name = name;
  }
}

const instance = new Example("John");
console.log(instance.name); // アクセス可能

```

- `protected`はクラス自体とそのサブクラスからアクセス可能。外部からはアクセス不可。
```typescript
class Example {
  protected name: string;

  constructor(name: string) {
    this.name = name;
  }
}

class SubExample extends Example {
  printName() {
    console.log(this.name); // アクセス可能
  }
}

const instance = new Example("John");
console.log(instance.name); // エラー: アクセス不可
```

- `private`はそのクラスでしかアクセスできない

```typescript
class Example {
  private name: string;

  constructor(name: string) {
    this.name = name;
  }

  printName() {
    console.log(this.name); // アクセス可能
  }
}

const instance = new Example("John");
console.log(instance.name); // エラー: アクセス不可
```

- readonly修飾子でプロパティを読み取り専用にする
- アクセス修飾子と併用可能で、`アクセス修飾子 readonly プロパティ名`のように書く

```typescript
class Person {
  readonly name: string;
  private readonly age: number;
 
  constructor(name: string, age: number) {
    this.name = name;
    this.age = age;
  }
 
  introduce(): void {
    console.log(`My name is ${this.name} and I am ${this.age} years old.`);
  }
}
 
const john = new Person("John", 20);
john.name = "Tom"; // エラー（readonlyのため変更不可）
```

- コンストラクタパラメータにアクセス修飾子をつけるとクラスのプロパテdぃと定義される

```typescript
class Person {
  constructor(public name: string, private age: number) {}
 
  introduce(): void {
    console.log(`My name is ${this.name} and I am ${this.age} years old.`);
  }
}
 
const john = new Person("John", 20);
john.introduce();
```

- フィールド宣言時に初期値を設定できる

```typescript
class Counter {
  count = 0; // 初期値を0に設定
  //    ^^^初期化子
 
  increment(): void {
    this.count++;
  }
}
 
const counter = new Counter();
console.log(counter.count);
counter.increment();
console.log(counter.count);
```

- `static`キーワードを使い、インスタンスではなく、クラスに紐づいたメソッドを定義できる
- インスタンスを作らなくてもアクセスできる

```typestrict
class MyClass {
  static x = 0;
 
  static printX(): void {
    console.log(MyClass.x);
  }
}
 
MyClass.printX();
```

- メソッドで`this`を返すことで、メソッドの呼び出しを直列につなげるメソッドチェーンを可能にする

```typescript
class MyClass {
  value = 1;
 
  increment(): this {
    this.value++;
    return this;
  }
 
  add(v: number): this {
    this.value += v;
    return this;
  }
 
  print(): this {
    console.log(this.value);
    return this;
  }
}
 
new MyClass().increment().add(3).print();
```

- `extends`キーワードによってクラスを継承し、スーパークラスのプロパティ、メソッドの値をサブクラスからアクセスできるようになる。

```typescript
class Animal {
  name: string;
 
  constructor(name: string) {
    this.name = name;
  }
 
  greet(): string {
    return `Hello, my name is ${this.name}`;
  }
}
 
class Dog extends Animal {
  bark(): string {
    return "Woof!";
  }
}
 
const dog = new Dog("Max");
console.log(dog.greet());
console.log(dog.bark());
```

- `instanceif`はオブジェクトが特定のクラスかそのサブクラスのインスタンスか判定する
- `オブジェクト名 instanceof クラス名`という書き方をする

```typescript
class Animal {}
class Dog extends Animal {}
 
const dog = new Dog();
 
console.log(dog instanceof Dog);
console.log(dog instanceof Animal);
```

- `abstract`キーワードで抽象クラスを定義できる。この抽象クラスは、インスタンス化出来ず、継承用の基底クラスに使用される。

```typescript
abstract class Animal {
  abstract makeSound(): void;
 
  move(): void {
    console.log("roaming the earth...");
  }
}
 
class Dog extends Animal {
  makeSound(): void {
    console.log("Woof Woof");
  }
}
 
const dog = new Dog();
dog.move();
dog.makeSound();
```

- ゲッターは`get`キーワードで定義されるオブジェクトのプロパティの値を取得するメソッド
- セッターは`set`キーワードで定義されるオブジェクトのプロパティの値を設定するメソッド
- これらは他のメソッドとは違い、プロパティの値を操作するときに暗黙的に呼び出される

```typescript
class Circle {
  private _radius: number;
 
  constructor(radius: number) {
    this._radius = radius;
  }
 
  // ゲッター
  get radius(): number {
    return this._radius;
  }
 
  // セッター
  set radius(radius: number) {
    if (radius <= 0) {
      throw new Error("Invalid radius value");
    }
    this._radius = radius;
  }
}
 
const circle = new Circle(5);
console.log(circle.radius);
circle.radius = 3;
console.log(circle.radius);
circle.radius = -2;
// 例外: 'Invalid radius value'
```

- インターフェースはプロパティ、メソッド、クラスなどの形状を定義し。特定のクラスやオブジェクトに形状を保つことを強制する
- 特定のオブジェクトが期待された動きをすることで信頼性を高める

```typescript
interface Printable {
  print(): void;
}
 
class MyClass implements Printable {
  print(): void {
    console.log("Hello, world!");
  }
}
```

- インターフェースはオブジェクトの形状を定義し、プロパティやメソッドのシグネチャ(定義の持つ情報)を記述できる。

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
  sum(): number;
}
 
const point: Point = {
  x: 10,
  y: 20,
  sum: function () {
    return this.x + this.y;
  },
};
```

- インターフェース内でreadonly修飾子を使用して、プロパティを読み取り専用にできる
- これによって、プロパティの値が一旦設定されると変更できなくなる

```typescript
interface Point {
  readonly x: number;
  readonly y: number;
}
 
const p1: Point = { x: 10, y: 20 };
p1.x = 5;
```

## 例外処理

- 例外処理のためにtry,catch,finallyブロックを使用できる。
- 例外が発生した場合catchブロックが実行される
- どんな場合でも`finally`を最後に実行することでクリーナップができる。

```typescript
try {
  throw new Error("An error occurred!");
} catch (error) {
  console.log(error);
}
```

- tryブロック内で、エラーを検出し、catchブロックはエラーをハンドリングする
- finallyブロックはエラーの有無に関係なく実行される

```typescript
try {
  throw new Error("Oops, something went wrong.");
} catch (error) {
  console.log(error);
} finally {
  console.log("This is the finally block. It always gets executed.");
}
```

- Errorクラスを継承したかスラムエラークラスを定義でき、具体的なエラータイプを作成できる

```typescript
class CustomError extends Error {
  code = "CustomError";
 
  constructor(message?: string) {
    super(message);
  }
}
 
try {
  throw new CustomError("This is a custom error");
} catch (error) {
  if (error instanceof CustomError) {
    console.log(`${error.code}: ${error.message}`);
  }
}
```

## 非同期処理

- TypeScriptは非同期処理をサポートしている

- `Promise`は非同期操作の最終的な完了(または失敗)とその結果の値を返す

```typescript
const promise = new Promise((resolve, reject) => {
  setTimeout(() => {
    resolve("Promise resolved");
  }, 2000);
});
 
promise.then((data) => {
  console.log(data);
});
```

- `async`や`await`を用いると読みやすい同期的に見える非同期コードを書ける

```typescript
function delay(ms: number) {
  return new Promise((resolve) => setTimeout(resolve, ms));
}
 
async function asyncFunction() {
  console.log("Start");
  await delay(2000);
  console.log("End");
}
 
asyncFunction();
 
// 2秒後
```

```typescript
// 非同期処理をシミュレートするための適当な関数
function wait(ms: number): Promise<string> {
  return new Promise(resolve => setTimeout(() => resolve("処理完了"), ms));
}

// 非同期関数を定義
async function asyncExample() {
  console.log("処理を開始");

  // 非同期処理を待つ
  const result1 = await wait(2000);
  console.log(result1);

  const result2 = await wait(1000);
  console.log(result2);

  console.log("処理が完了しました");
}

// 非同期関数を呼び出し
asyncExample();
```

## ジェネリックス

- 型の再利用性が向上し、型の一貫性を保てる
- ジェネリックスを使用すると、型変数を導入でき、機能の一部を一般化できる

```typescript
// Tが型変数
function identity<T>(arg: T): T {
  return arg;
}
 
// 型変数Tにstringを割り当てる
const output1 = identity<string>("myString");
 
// 型変数Tにnumberを割り当てる
const output2 = identity<number>(100);
```

## モジュール

- 他のモジュールと共有したりするコードと、内部限定で使用するコードを分けることができる

```typescript
export function greet(name: string) {
  return `Hello, ${name}!`;
}
```

```typescript
import { greet } from "./greeter";
console.log(greet("TypeScript"));
```

- モジュール外部に公開するときは`export`、利用するには`import`を使用する

```typescript
export function square(x: number) {
  return x * x;
}
 
export function cube(x: number) {
  return x * x * x;
}
```

```typescript
import { square, cube } from "./math";
 
console.log(square(2));
console.log(cube(2));
```

- `default`キーワードを使用すると、モジュールがデフォルトで1つの値のみをエクスポートする
- `default export`は`import`する際に、別名を指定できる
- `default`でエクスポートする変数は決まっているので、インポートする際に名前を変えても問題ない

```typescript
// greeter.ts
export default function greet(name: string) {
  return `Hello, ${name}!`;
}
```

```typescript
// main.ts
import greetFunction from "./greeter";
console.log(greetFunction("TypeScript"));
```

- モジュールは一度エクスポートされたものを再度エクスポートできる
- `export 機能 from 元のファイル名`のような書き方をする

```typescript
// math.ts
export function add(x: number, y: number) {
  return x + y;
}
```

```typescript
// index.ts
// 再エクスポート
export { add } from "./math";
```

```typescript
// main.ts
import { add } from "./index";
console.log(add(2, 3));
```

- 型だけをエクスポートやインポートできる
- `export type`や`import type {...} from`のような書き方をする。

```typescript
// types.ts
export type MyObject = {
  name: string;
  age: number;
};
```

```typescript
// main.ts
import type { MyObject } from "./types";
//     ^^^^型インポート
 
const obj: MyObject = {
  name: "TypeScript",
  age: 3,
};
```

## 型レベルプログラミング

- TypeScriptには型レベルでプログラミングするための様々な機能が搭載されている。

- `typeof`演算子は変数名から型を計算できる

```typescript
const object = {
  name: "TypeScript",
  version: 3.9,
};
 
type ObjectType = typeof object;
```

- `keyof`演算子を使うと、object型のすべてのキーを文字列のユニオン型として取得できる

```typescript
type Point = {
  x: number;
  y: number;
};
 
type Key = keyof Point;
const key1: Key = "x"; // 代入OK
const key2: Key = "y"; // 代入OK
const key3: Key = "z"; // 代入不可
```

### ユーティリティ型

- `Required`はオプションプロパティを必須プロパティにする。

```typescript
type Person = {
  name: string;
  age?: number;
};
 
type RequiredPerson = Required<Person>;
// ageがオプションでなくなっている点に注目
```

- `Partial`は、型のすべてのプロパティをオプションに変換する

```typescript
type Person = {
  name: string;
  age: number;
};
 
type OptionalPerson = Partial<Person>;
```

- `Readonly`はすべてのプロパティを読み取り専用にする

```typescript
type Person = {
  name: string;
  age: number;
};
 
type ReadonlyPerson = Readonly<Person>;
```

- `Record`はオブジェクトの指定されたプロパティを特定の方にするユーティリティ型

```typescript
type ThreeLetterRecord = Record<"one" | "two" | "three", string>;
```

- `Pick`はオブジェクトから特定のプロパティを取り出すユーティリティ型

```typescript
type Person = {
  name: string;
  age: number;
  address: string;
};
 
type PersonNameAndAge = Pick<Person, "name" | "age">;
```

- `Omit`はオブジェクトから特定のプロパティを省いた型を作るユーティリティ型

```typescript
type Person = {
  name: string;
  age: number;
  address: string;
};
 
type PersonWithoutAddress = Omit<Person, "address">;
```

- `Exclude`はユニオン型から特定のプロパティを除外するユーティリティ型

```typescript
type T1 = number | string | boolean;
type T2 = Exclude<T1, boolean>;
```

- `Extract`は2つのユニオン型の共通する部分を抽出するユーティリティ型

```typescript
type T1 = number | string | boolean;
type T2 = string | boolean;
type T3 = Extract<T1, T2>;
```

- `NonNullable`は、nullまたはundefinedを含む型をいずれも除外するユーティリティf型

```typescript
type T1 = string | null | undefined;
type T2 = NonNullable<T1>;
```

### Mapped types

- 既存の型から新しい型を生成する機能
- オブジェクトの各プロパティをマップし、新しいオブジェクトを生成する。

```typescript
type Person = {
  name: string;
  age: number;
};
 
type ReadOnlyPerson = { readonly [K in keyof Person]: Person[K] };
```

### インデックスアクセス型

- 型のプロパティの型を取得できる

```typescript
type Person = {
  name: string;
  age: number;
};
 
type Name = Person["name"];
```

```typescript
type Person = {
  name: string;
  age: number;
  city: string;
};

type AgeType = Person['age'];

const ageVariable: AgeType = 25;
```