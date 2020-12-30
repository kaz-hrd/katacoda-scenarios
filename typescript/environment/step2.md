# Expressを使用したHttpサーバ

### ソースディレクトリの作成
`mkdir src/server`{{execute}}

### TypeScriptコンパイラの初期設定
- 以下のコマンドでtsconfig.jsonが生成されます。tsconfig.jsonはTypeScriptコンパイラ（tsc）の設定ファイルです。
 `npx tsc --init`{{execute}}

- 今回は、サーバ用とクライアント用のtsconfig.jsonの２ファイルを使用するので、tsconfig.jsonをrenameします。
 `mv tsconfig.json tsconfig.server.json`{{execute}}

### TypeScriptコンパイラ（tsc）の設定ファイルの変更
`tsconfig.server.json`{{open}}に以下の変更を行います。

    ```
    {
        "compilerOptions": {
            "target": "ES2019",
            "module": "commonjs",
            "outDir": "./dest",
            "strict": true,
            "strictPropertyInitialization": false ,
            "esModuleInterop": true,
            "skipLibCheck": true,
            "forceConsistentCasingInFileNames": true
        },
        "include": [
            "./src/server/*",
        ]
    }
    ```


