# bun run e2eコマンド

## 解決すべき問題

bunとHonoでHTTPサーバアプリケーションを作ろうと思う。そのサーバアプリをPlaywrightでテストしたい。すると次のようなオペレーションを頻繁に繰り返すことが必要になる。

1. `bun run src/index.tsx` とコマンドを実行してHTTPサーバを起動する
2. `bun test` とコマンドを実行してテストを実行する。テストは `http://localhost:3000/` にHTTPリクエストを上げる。そして戻ってきたHTTPレスポンスを吟味する。
3. テストが終了したら、HTTPサーバを停止する。

この操作のうち1と2はまあいい。3をどうやって実装すればいいのだろう？

## 解決方法

3をbashシェルで実装できる。

1. `ps aux`コマンドでプロセスのリストを取得する。
2. プロセスのリストを `bun run src/index.tsx` というコマンド文字列をキーとしてgrepすることによりHTTPサーバのプロセスを選び出す
3. awk `{print $2}` で行の2番目の項目つまりプロセスIDを取得する
4. 'kill プロセスID'を実行する

つまり

```
kill $(ps aux | grep '[0-9] bun run --hot src/index.tsx' | awk '{print $2}')
```

## 説明

[package.json](https://github.com/kazurayam/bun-run-e2e/blob/master/my-app/package.json)を参照。

テストを実行するには次のようにする。

```
$ bun run e2e
do something with the dev server...
```
