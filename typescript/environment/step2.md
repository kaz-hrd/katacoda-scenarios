# Expressを使用したHttpサーバ
Expressを使用して、JSONデータを返却するHttpサーバを作成します。

### ソースディレクトリの作成
`mkdir src/server`{{execute}}

### TypeScriptコンパイラの初期設定
- 以下のコマンドでtsconfig.jsonが生成されます。tsconfig.jsonはTypeScriptコンパイラ（tsc）の設定ファイルです。<br />
 `npx tsc --init`{{execute}}

- 今回は、サーバ用とクライアント用のtsconfig.jsonの２ファイルを使用するので、tsconfig.jsonをrenameします。<br />
 `mv tsconfig.json tsconfig.server.json`{{execute}}

### TypeScriptコンパイラ（tsc）の設定ファイルの変更
`example/tsconfig.server.json`{{open}}に以下の変更を行います。<br />

    `JSON
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
    `{{copy}}

### Expressライブラリのインストール
`npm install --save express`{{execute}}

### 型定義のインストール
`npm install --save-dev @types/node @types/express`{{execute}}

### ソースファイルの編集
`example/src/server/server.ts`{{open}}に以下の変更を行います。<br />

    `
    import express, { Express, Request, Response} from 'express';

    class Server {
        private _express: Express;
        constructor(private _port: number){
        }
        start(){
            this._express = express();
            this._express.get('/now', (req, res)=> {
                this.processNow(req, res);
            });
            this._express.listen(this._port);
        }
        private processNow(req: Request, res: Response){
            const result =  {message: "Hello World.", datetime: (new Date()).toLocaleString() };
            res.json(result);
        }
    }

    let port: number
    if(process.argv.length >= 3) {
        port = parseInt(process.argv[2]);
    }else{
        console.error("Error: Illegal argument.")
        process.exit(1);
    }

    const server = new Server(port);
    server.start();
    `{{copy}}

### ビルド
tscコマンドにて、JavaScriptに変換します<br />
`npx tsc -p tsconfig.server.json`{{execute}}

### サーバの起動
`node ./dest/server.js 80 &`{{execute}}

### package.jsonの修正
package.jsonのscriptsに追加したほうが少し楽かと思います
`
  "scripts": {
    "build:server": "tsc -p tsconfig.server.json",
    "start": "node ./dest/server.js 80 &"
  }
`{{copy}}

package.jsonに蒸気を追加すると、以下のようなコマンドにてビルドやサーバの起動が行えます
- `npm run build:server`{{execute}}
- `npm run start`{{execute}}

### アクセステスト
`curl localhost/now`{{execute}}

### URL
https://[[HOST_SUBDOMAIN]]-80-[[KATACODA_HOST]].environments.katacoda.com/now
