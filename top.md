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

