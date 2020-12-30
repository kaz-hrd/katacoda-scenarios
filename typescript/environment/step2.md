# React

### ソースディレクトリの作成
`mkdir src/client`{{execute}}

### TypeScriptコンパイラの初期設定
- 以下のコマンドでtsconfig.jsonが生成されます。tsconfig.jsonはTypeScriptコンパイラ（tsc）の設定ファイルです。<br />
 `npx tsc --init`{{execute}}

- 今回は、サーバ用とクライアント用のtsconfig.jsonの２ファイルを使用するので、tsconfig.jsonをrenameします。<br />
 `mv tsconfig.json tsconfig.client.json`{{execute}}

### TypeScriptコンパイラ（tsc）の設定ファイルの変更
`example/tsconfig.client.json`{{open}}に以下の変更を行います。

```
{
  "compilerOptions": {
      "sourceMap": true,
      "target": "ES2019",
      "module": "esnext",
      "jsx": "react",
      "lib": [
        "es2017",
        "dom"
      ],
      "allowSyntheticDefaultImports": true
  },
  "include": [
    "./src/client/*"
  ]
}
```{{copy}}

### ライブラリのインストール
`npm install --save bootstrap-css-only react react-dom`{{execute}}

### 型定義のインストール
`npm install --save-dev @types/node @types/reac @types/react-dom`{{execute}}

### webpackと関連プラグインのインストール
`npm install --save-dev webpack webpack-cli ts-loader css-loader style-loader`{{execute}}

### クライアントコードの出力先ディレクトリを作成
`mkdir dest/public`{{execute}}

### webpackの設定
`example/webpack.config.js`{{open}}の作成します。以下の設定とします。

```
const path = require('path');

module.exports = [
    {
        target: "web", 
        mode: 'development',
        entry: './src/client/client.tsx',
        output: { // build時に出力する先
          path: path.join(__dirname, "dest/public"),
          filename: 'client.js'
        },
        module: {
          rules: [
            {
              test: /\.tsx?$/,
              use: {
                loader: "ts-loader",
                options: {
                    transpileOnly: true,
                    configFile: "tsconfig.client.json",
                }
              }
            },
            {
                test: /\.css/,
                use: [
                  "style-loader",
                  {
                    loader: "css-loader",
                    options: { url: false }
                  }
                ]
              }
          ]
        },
        resolve: {
          extensions: [ // importできるファイルの拡張子
            '.ts', '.tsx', '.js', '.json'
          ],
        },
        devtool: 'inline-source-map',  // sourcemapを使えるようにする
    }
]
```{{copy}}

### style.cssの作成
`example/src/client/style.css`{{open}}に以下の変更を行います。

```css
@import '~bootstrap-css-only/css/bootstrap.min.css';
```{{copy}}

### ソースファイルの編集
`example/src/client/client.tsx`{{open}}に以下の変更を行います。

```
import React, { Component } from "react";

import "./style.css";



```{{copy}}