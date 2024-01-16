# Next.jsで猫画像ジェネレーターを作ろう

- [ここ](https://typescriptbook.jp/tutorials/nextjs)を参照

## Next.jsの概要

- Reactをベースにしたフロントエンドフレームワーク
- Reactではwebpackを使わなければいけないが設定が面倒
- Next.jsはパフォーマンス最適化をフレームワークで内蔵している
- Vercelというホスティングサービスで簡単に公開できる
- pagesディレクトリ配下のディレクトリ構造がページのルーティングに対応
    - `/users`にアクセスしたとき、`pages/users.tsx`
    - `/`にアクセスしたとき、`pages/index.tsx`
- pagesデイぃレク取りに置かれたコンポーネントをページコンポーネントという
- Next.jsでは1ファイルにつき1コンポーネント作成する
- デフォルトエクスポートされた関数をページコンポーネントとして扱う
- `NextPage`はページコンポーネントを表す型
- `const [変数,変数の更新関数] = useState(初期値)`
- `useEffect(() => {処理内容の関数}, その関数を呼び出すタイミング)`
    - 呼び出すタイミングを`[]`と記述するとマウントされたときのみ実行になる
- Reactはクライアントサイドでのレンダリング(クライアントサイドでページ生成)
- Next.jsはサーバーサイドでのレンダリング(ページ生成)

### getServerSideProps

- ページがリクエストされるたびにサーバーサイドで実行され、ページのプロパティを返す関数
- クライアントサイドでルーティングが発生した場合も、この関数がサーバーサイドで実行
- サーバーサイドでのみ実行されるため、配信するファイルサイズの削減ができる
- データベースなどウェブに公開していないミドルウェアから直接データを取得できる

### getStaticProps

- 静的生成するページのデータを取得する関数
- ビルド時に実行
- ユーザーログインが不要なページや、内容のリアルタイムさが不要なページで利用される

### getinitialProps

- サーバーとクライアントの両方で動作する必要があるため、おすすめしない


## スタイルシート

- `.module.css`で終わるファイルはCSSモジュールと言い、CSSファイル内で定義したクラスをオブジェクトとして参照できる様になる