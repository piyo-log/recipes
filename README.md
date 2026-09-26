# ぴよのごはんログ

3歳の子どもの晩ごはんを記録するHugoブログです。外部テーマやJavaScriptの依存関係はありません。記事はMarkdownで管理します。

## ローカルで見る

Hugo 0.152.2でビルドを確認しています。プロジェクトのディレクトリで実行してください。

```sh
hugo server --baseURL http://localhost:1313/ --bind 127.0.0.1
```

ブラウザで <http://localhost:1313/> を開きます。下書きも表示する場合は `-D` を付けてください。

## 記録を追加する

```sh
hugo new content posts/first-cooking.md
```

`content/posts/first-cooking.md` が作られます。タイトル、説明、本文を編集し、公開する記事は `draft: false` にします。`date` が未来の記事は、通常のビルドには含まれません。

- `category`：例えば「献立を考える」「作ってみた」。記事に表示する分類です。
- `status`：「計画」または「記録」。計画の記事には、調理・試食前である旨を表示します。
- `slug`：記事URLの末尾です。公開後は原則として変えません。

献立を考えた段階と実際に作った結果を区別し、調理時間や食べた量は確認した内容を書いてください。保存方法などの情報には参照先を添えます。

## ビルドする

```sh
hugo --gc --minify --panicOnWarning
```

配信用ファイルは `public/` に生成されます。`public/` とHugoの生成キャッシュはGitに含めません。

既定のキャッシュディレクトリに書き込めない環境では、各コマンドに `--cacheDir "$PWD/.cache/hugo"` を付けると、このプロジェクト内にキャッシュを保存できます。

## 主なファイル

| 場所 | 用途 |
| --- | --- |
| `content/posts/` | ごはんの記録 |
| `content/about.md` | ブログの説明 |
| `data/dishes.toml` | 最初の8品の献立案 |
| `layouts/` | 表示用テンプレート |
| `assets/css/main.css` | 配色とレイアウト |
| `hugo.toml` | タイトル、説明、公開先URLなど |

最初の記事では `{{< eight-dishes >}}` で8品の一覧を表示しています。この記事の計画を変更するときは `data/dishes.toml` を編集してください。別の日の献立は新しい記事に書くと、最初の計画を残せます。

## GitHubと公開先

origin は `git@github.com:piyo-log/recipes.git` です。

公開先は [ぴよのごはんログ](https://piyo-log.github.io/recipes/) です。`hugo.toml` の `baseURL` もこのURLに設定しています。

`main` にpushすると、`.github/workflows/pages.yml` がHugoでビルドし、GitHub Pagesへ公開します。下書きの記事（`draft: true`）は公開されません。GitHubのActions画面から「Publish Hugo site」を選び、手動で再実行することもできます。

GitHub PagesのSourceは「GitHub Actions」を使用します。Hugoはローカルで確認した0.152.2に固定し、ダウンロード時にSHA-256を検証します。ActionsもコミットSHAで固定しています。公開処理で必要な書き込み権限はデプロイジョブに限定しています。

独自ドメインなどに公開先を変更する場合は、GitHub Pagesの設定と `hugo.toml` の `baseURL` を合わせて変更してください。CIではGitHub Pagesが返すURLを使ってビルドします。

設定の参考：[Hugo公式のGitHub Pages公開手順](https://gohugo.io/host-and-deploy/host-on-github-pages/)。
