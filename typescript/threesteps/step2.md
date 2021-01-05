# Reactを使ったクライアントアプリ

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
`npm install --save bootstrap-css-only react react-dom node-fetch`{{execute}}

### 型定義のインストール
`npm install --save-dev @types/node @types/react @types/react-dom @types/node-fetch`{{execute}}

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

### index.htmlの作成
`example/dest/public/index.html`{{open}}を作成します。

```html
<!DOCTYPE html>
<html lang="ja">
    <head>
        <meta charset="utf-8">
        <title>Member</title>
    </head>
    <body>
        <div id="root"></div>
        <script src="./client.js"></script>
    </body>
</html>
```{{copy}}

### ソースファイルの編集
`example/src/client/client.tsx`{{open}}に以下の変更を行います。

```
import React, { Component } from "react";
import { render } from "react-dom";
import "./style.css";

interface AppProps { }
interface AppState {
    message: string,
    now: string,
    show: boolean
}

class SampleApp extends Component<AppProps, AppState> {
    constructor(props: AppProps) {
        super(props);
        this.state = {
            message: '', 
            now: '',
            show: false
        }
        this.execute = this.execute.bind(this);
    }

    async execute(){
        const res = await fetch('./now', {method: 'GET'});
        const data = await res.json() as {message: string, datetime: string};
        console.dir(data);
        this.setState({message: data.message, now: data.datetime, show: true});
    }
    public render() {
        return (
        <div className="container">
            <div className="row">
                <div className="col-4">
                    <button type="button" className="btn btn-primary" onClick={this.execute}>実行</button>
                </div>
                {(() => {
                    if(this.state.show){
                        return (
                        <div className="col-6">
                            {this.state.now}
                        </div>);
                    }
                })()}
            </div>
        </div>
        );
    }
}

render(<SampleApp />, document.getElementById("root"));
```{{copy}}

### ビルド
`npx webpack`{{execute}}

### package.jsonの修正
package.jsonのscriptsに追加しておきましょう。
```
"scripts": {
    "build:client": "webpack"
}
```{{copy}}

追加することで、
`npm run build:client`{{execute}}
を実施することでビルドできるようになります。
