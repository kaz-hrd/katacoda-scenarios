# React

### ソースディレクトリの作成
`mkdir src/client`{{execute}}

### TypeScriptコンパイラの初期設定
- 以下のコマンドでtsconfig.jsonが生成されます。tsconfig.jsonはTypeScriptコンパイラ（tsc）の設定ファイルです。<br />
 `npx tsc --init`{{execute}}

- 今回は、サーバ用とクライアント用のtsconfig.jsonの２ファイルを使用するので、tsconfig.jsonをrenameします。<br />
 `mv tsconfig.json tsconfig.client.json`{{execute}}

