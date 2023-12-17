# `npm` チートシート

`npm` コマンドの自分用チートシートです。パッケージ利用者側の立場で利用頻度の高いコマンドを並べています。

すべてのコマンドは公式ドキュメントで確認できます:

- [CLI Commands | npm Docs](https://docs.npmjs.com/cli/v10/commands)

## 最終確認時バージョン

- `npm`: `10.2.4`

## コマンド

### `npm audit`

パッケージのセキュリティステータスをチェックし既知の脆弱性をリストアップする。

<details>
<summary>出力サンプル:</summary>

```bash
npm audit
```

```text
# npm audit report

@cypress/request  <=2.88.12
Severity: moderate
Server-Side Request Forgery in Request - https://github.com/advisories/GHSA-p8p7-x288-28g6
fix available via `npm audit fix --force`
Will install cypress@13.1.0, which is a breaking change
node_modules/@cypress/request
  cypress  4.3.0 - 12.17.4
  Depends on vulnerable versions of @cypress/request
  node_modules/cypress
```

</details>

引数として `fix` か `signatures` を受け付ける。

```bash
npm audit fix
npm audit signatures
```

- `fix`: 脆弱性修正のためにパッケージ更新を試みる
- `signatures`: ダウンロードされたパッケージのシグネチャをチェックする

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-audit)

### `npm ci` / `npm clean-install`

`npm install` の CI / デプロイ用。 `npm install` と同様パッケージのインストールを行うが、クリーンインストールを行う点が異なる。

`npm ci` の特徴:

- `package-lock.json` か `npm-shrinkwrap.json` が必須
- `package-lock.json` と `package.json` の間に依存関係の不一致があればエラーで終了する
- 個別のパッケージの追加には使えない（＝すべてのパッケージをインストールする用途でのみ使える）
- `node_modules` がすでにある場合は先に削除してからパッケージのインストールを行う
- `package.json` や `package-lock.json` への書き込みを行わない

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-ci)

### `npm completion`

サブコマンドのタブ補完を有効にするためのコマンドを出力する。

使い方:

```bash
# bash
npm completion >> ~/.bashrc

# zsh
npm completion >> ~/.zshrc
```

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-completion)

### `npm explain`

インストール済みのパッケージの依存チェーンを出力する。

<details>
<summary>出力サンプル:</summary>

```bash
npm eplain axios
```

```text
axios@0.21.4
node_modules/axios
  axios@"^0.21.1" from gatsby@5.12.12
  node_modules/gatsby
    gatsby@"^5.2.0" from the root project

axios@1.6.2 dev
node_modules/wait-on/node_modules/axios
  axios@"^1.6.1" from wait-on@7.2.0
  node_modules/wait-on
    wait-on@"7.2.0" from start-server-and-test@2.0.3
    node_modules/start-server-and-test
      dev start-server-and-test@"^2.0.0" from the root project
```

</details>

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-explain)

### `npm init`

`package.json` ファイルを作成する。プロジェクトの新規作成用。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-init)

### `npm install` / `npm i`

パッケージをインストールする。

引数としてパッケージ名が渡されたときはそのパッケージをインストールする。パッケージ名が渡されなかったときは `package.json` `npm-shrinkwrap.json` `package-lock.json` などで指定されているパッケージをインストールする。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-install)

### `npm ls` / `npm list`

インストールされているパッケージのバージョンを出力する。

引数なしの場合は `package.json` で指定されているパッケージだけを表示する:

<details>
<summary>出力サンプル:</summary>

```bash
npm ls
```

```text
project@version /path/to/the/project/root/
├── @fontsource/montserrat@4.5.14
├── @google-analytics/data@3.2.2
├── @supabase/supabase-js@2.21.0
├── @testing-library/cypress@9.0.0
├── autoprefixer@10.4.14
├── babel-jest@29.5.0
...
```

</details>

`--all` オプションが指定された場合は依存先パッケージも含めてツリー構造（＝依存関係グラフ）で表示する:

```bash
npm ls --all
```

`--all` に加えて `--depth` を使うと深さを指定できる:

```bash
npm ls --all --depth=2
```

引数でパッケージが指定された場合は該当するパッケージに関連するものだけ表示する:

<details>
<summary>出力サンプル:</summary>

```bash
❯ npm ls axios
```

```text
myproject@1.0.0 /path/to/the/project/dir
├─┬ gatsby@5.12.12
│ └── axios@0.21.4
└─┬ start-server-and-test@2.0.3
  └─┬ wait-on@7.2.0
    └── axios@1.6.2
```

</details>

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-ls)

### `npm outdated`

インストールされているパッケージのうち最新でないもの（＝更新可能なもの）の一覧を出力する。

デフォルトでは `package.json` で指定されたものだけをチェックする:

```bash
npm outdated
```

`--all` オプションが指定された場合は依存先も含めて表示する:

```bash
npm outdated --all
```

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-outdated)

### `npm repo`

パッケージリポジトリページをブラウザで開く。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-repo)

サンプル:

```bash
npm repo axios
```

### `npm run-script` / `npm run`

`package.json` の `scripts` で定義されたスクリプトを実行する。

サンプル:

```bash
npm run dev
```

#### 引数の渡し方

対象のスクリプトにオプション（＝ `-` で始まる引数）を渡したいときは `--` で区切って渡す必要がある:

```bash
npm run test -- --grep="pattern"
```

スクリプトが指定されなかった場合は利用可能なスクリプトの一覧を出力する:

```bash
npm run
```

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-run-script)

### `npm start`

`package.json` の `scripts.start` で指定されたスクリプトを実行する。

`scripts.start` がない場合は `node server.js` が実行される。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-start)

### `npm stop`

`package.json` の `scripts.stop` で指定されたスクリプトを実行する。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-stop)

### `npm test`

`package.json` の `scripts.test` で指定されたスクリプトを実行する。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-test)

### `npm uninstall` / `npm un` / `npm remove` / `npm rm`

パッケージをアンインストールする。

`package.json` や `npm-shrinkwrap.json` `package-lock.json` に記載された該当パッケージの情報も削除する。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-uninstall)

### `npm update` `npm up`

semver 制約を守りながらパッケージを更新する。

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-update)

### `npm view`

パッケージに関する情報を出力する。

<details>
<summary>出力サンプル:</summary>

```bash
npm view axios
```

```text
axios@1.6.2 | MIT | deps: 3 | versions: 85
Promise based HTTP client for the browser and node.js
https://axios-http.com

keywords: xhr, http, ajax, promise, node

dist
.tarball: https://registry.npmjs.org/axios/-/axios-1.6.2.tgz
.shasum: de67d42c755b571d3e698df1b6504cde9b0ee9f2
.integrity: sha512-7i24Ri4pmDRfJTR7LDBhsOTtcm+9kjX5WiY1X3wIisx6G9So3pfMkEiU7emUBe46oceVImccTEM3k6C5dbVW8A==
.unpackedSize: 1.8 MB

dependencies:
follow-redirects: ^1.15.0 form-data: ^4.0.0         proxy-from-env: ^1.1.0

maintainers:
- mzabriskie <mzabriskie@gmail.com>
- nickuraltsev <nick.uraltsev@gmail.com>
- emilyemorehouse <emilyemorehouse@gmail.com>
- jasonsaayman <jasonsaayman@gmail.com>

dist-tags:
latest: 1.6.2        next: 1.2.0-alpha.1

published a month ago by jasonsaayman <jasonsaayman@gmail.com>
```

</details>

[公式ドキュメント](https://docs.npmjs.com/cli/v10/commands/npm-view/)

## 逆引き

### 更新可能なパッケージを調べたい

```bash
npm outdated
```

パッケージ名だけを一覧表示したいとき:

```bash
npm outdated --json | jq -r '.keys[]'
```

### 特定のパッケージに依存しているパッケージを調べたい

```bash
npm ls [パッケージ名]
# or
npm explain [パッケージ名]
```

<details>
<summary>出力サンプル:</summary>

```bash
npm ls axios
```

```text
myproject@1.0.0 /path/to/the/project/dir
├─┬ gatsby@5.12.12
│ └── axios@0.21.4
└─┬ start-server-and-test@2.0.3
  └─┬ wait-on@7.2.0
    └── axios@1.6.2
```

</details>

### 特定のパッケージの依存先パッケージを調べたい

```bash
npm view [パッケージ名]
# or
npm view [パッケージ名] dependencies
# or
npm view [パッケージ名] dependencies devDependencies
```

<details>
<summary>出力サンプル:</summary>

```bash
npm view axios
```

```text
axios@1.6.2 | MIT | deps: 3 | versions: 85
Promise based HTTP client for the browser and node.js
https://axios-http.com

keywords: xhr, http, ajax, promise, node

dist
.tarball: https://registry.npmjs.org/axios/-/axios-1.6.2.tgz
.shasum: de67d42c755b571d3e698df1b6504cde9b0ee9f2
.integrity: sha512-7i24Ri4pmDRfJTR7LDBhsOTtcm+9kjX5WiY1X3wIisx6G9So3pfMkEiU7emUBe46oceVImccTEM3k6C5dbVW8A==
.unpackedSize: 1.8 MB

dependencies:
follow-redirects: ^1.15.0 form-data: ^4.0.0         proxy-from-env: ^1.1.0

maintainers:
- mzabriskie <mzabriskie@gmail.com>
- nickuraltsev <nick.uraltsev@gmail.com>
- emilyemorehouse <emilyemorehouse@gmail.com>
- jasonsaayman <jasonsaayman@gmail.com>

dist-tags:
latest: 1.6.2        next: 1.2.0-alpha.1

published a month ago by jasonsaayman <jasonsaayman@gmail.com>
```

</details>

<details>
<summary>出力サンプル <code>dependencies</code> 付き:</summary>

```bash
npm view axios dependencies
```

```text
{
  'follow-redirects': '^1.15.0',
  'form-data': '^4.0.0',
  'proxy-from-env': '^1.1.0'
}
```

</details>

### 依存ツリー（依存関係グラフ）をすべて表示したい

```bash
npm ls -a
```

### パッケージの公式ページを開きたい

```bash
npm repo [パッケージ名]
```
