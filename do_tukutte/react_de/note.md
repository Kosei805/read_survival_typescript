# Reactでいいねボタンを作ろう

## Reactとは？

- webアプリケーションのUIを作るためのパッケージ
- 短く簡潔に書けるようになる
- 状態の変化がわかりやすくなる

## Reactの3大特徴

- 仮想DOM
    - DOM(document object model)はHTMLをJavaScriptから参照・操作する仕組み
    - 仮想にすると
        - バグが起こりにくい
        - 最適化処理が行われる

- 宣言的UI
    - Reactを使わない場合は命令的UIで実装が面倒
    - Reactでの宣言的UIは、目標を書くだけ

- コンポーネントベース
    - 小さなコンポーネントを組み合わせて大きなコンポーネントを作る
    - コンポーネントを再利用する
    - オープンソースのコンポーネントを使える

## 開発

- JavaScriptの中にXML(HTMLのようなもの)を直接書ける部分をJSXという
- TypeScriptとは無関係だが、利便性のために搭載されている。

```typescript
import React from "react";
import "./App.css";
 
function App() {
  return (
    <div className="App">
      <header className="App-header">TypeScriptはいいぞ</header>
    </div>
  );
}
 
export default App;
```

- ReactのJSXでは自分の関数をタグとして使うことができる
- <~/></~>といった子要素を持たない場合は末尾を<~ />と書くことで短くできる。これはJSX特有である。

```typescript
import React from "react";
import "./App.css";
 
function App() {
  return (
    <div className="App">
      <header className="App-header">
        <LikeButton />
      </header>
    </div>
  );
}
 
function LikeButton() {
  return <span>いいねボタン予定地</span>;
}
 
export default App;
```