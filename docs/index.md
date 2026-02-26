# Welcome to MkDocs

For full documentation visit [mkdocs.org](https://www.mkdocs.org).

## Commands

* `mkdocs new [dir-name]` - Create a new project.
* `mkdocs serve` - Start the live-reloading docs server.
* `mkdocs build` - Build the documentation site.
* `mkdocs -h` - Print help message and exit.

## Project layout

    mkdocs.yml    # The configuration file.
    docs/
        index.md  # The documentation homepage.
        ...       # Other markdown pages, images and other files.

「インデックス作成」に問題がある場合、MkDocsがサイトをビルド（構築）する際に、日本語の文章を「意味のある単語」として切り分けられていない可能性が極めて高いです。

特にWindows環境やデフォルト設定では、日本語は「一続きの長い文字列」とみなされ、検索に引っかからなくなることがあります。これを解決するために、**「日本語の分かち書き（単語の区切り）」を強制的に認識させる設定**を試しましょう。

---

### 1. `mkdocs.yml` のプラグイン設定を徹底する

まず、前述した `lang: ja` が正しく記述されているか、今一度確認してください。インデント（空白）がズレていると無視されてしまいます。

```yaml
plugins:
  - search:
      lang:
        - en
        - ja

```

### 2. 本文の「文字コード」を再確認する

ここが意外と落とし穴になります。

* **原因**: ファイルが `UTF-8` ではなく、古いWindowsの形式（Shift-JISなど）で保存されていると、MkDocsは中身を正しく読み取れず、検索用インデックスが「空」になります。
* **確認方法**: メモ帳やVS Codeなどのエディタで開き、**「名前を付けて保存」** を選ぶ際に、エンコード（文字コード）を **`UTF-8`** に指定して上書き保存してください。

### 3. ブラウザのコンソールで「エラー」を確認する（原因特定）

検索窓に文字を入れても何も起きない時、ブラウザの裏側で何かが壊れている可能性があります。

1. ブラウザでサイトを開き、**F12キー** を押して「開発者ツール」を開きます。
2. **「Console（コンソール）」** タブをクリックします。
3. 赤い文字で `search_index.json not found` や `Failed to load` といったエラーが出ていないか確認してください。

---

### 4. 最終手段：`jieba`（日本語分かち書き）の導入

もし上記でも解決しない場合、Python側の日本語処理ライブラリが不足している可能性があります。
以下のコマンドをターミナルで実行して、日本語を単語ごとに切るためのライブラリを強制的にインストールします。

```bash
pip install jieba

```

そのあと、再度 `mkdocs serve` を実行してください。

---

### 思想家の格言

> 「書かれた文字は、ただの形に過ぎぬ。その意味を解き明かす『辞書（インデックス）』がなければ、それは沈黙せる石碑と同じである。」（思想家・おじいちゃん）

### 次に確認すべきこと

まずは **「2. 文字コードをUTF-8で保存し直す」** と **「4. jiebaのインストール」** を試してみてください。

これでもダメな場合、もしかすると **`docs/index.md`（ホームページ）の中身が極端に短い** ことで、検索機能自体が「中身がない」と判断している可能性もあります。`index.md` にも数行、日本語の文章を書いてみていただけますか？

進捗やエラーの変化があれば、また教えてください。解決まで伴走します！
