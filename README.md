# （非公式） Dfinity ドキュメント翻訳プロジェクト

本リポジトリは、[こちら](https://github.com/Japan-DfinityInfoHub/docs)で実施している Dfinity ドキュメント翻訳プロジェクトの子リポジトリです。

> Dfinity 公式ドキュメントは、[dfinity/docs](https://github.com/dfinity/docs) のリポジトリに全てのドキュメントファイルが集約されているわけではなく、[dfinity/motoko](https://github.com/dfinity/motoko) などの他のリポジトリに置かれた AsciiDoc ファイルが集約されて一つのドキュメントとなっています。

本リポジトリでは、[dfinity/motoko-base](https://github.com/dfinity/motoko-base) のドキュメントを翻訳しています。
なお、motoko-base ではソースコードのコメントからドキュメントを自動生成しています。

## 翻訳手順

翻訳の状況は、親リポジトリである Japan-DfinityInfoHub/docs の[翻訳の概要と進捗状況](https://github.com/Japan-DfinityInfoHub/docs/issues/17)の issues を確認してください。

### 手順 1: 翻訳を始める準備

まずは、このリポジトリを右上から Fork してください。

そして、リポジトリをクローンします。`your` には、あなたの GitHub のユーザーネームを入れてください。

```
$ git clone https://github.com/your/motoko-base
$ cd motoko-base
```

翻訳作業を行うためのブランチを作成します。
どのファイルを翻訳するかは、Japan-DfinityInfoHub/docs の[翻訳の概要と進捗状況](https://github.com/Japan-DfinityInfoHub/docs/issues/17)の翻訳ページ一覧を確認して、翻訳したい箇所をコメントしてください。

ここでは、例として `Order.mo` を翻訳するためのブランチを作成します。

```
$ git checkout -b Order.mo
```

これで、翻訳を始める準備は完了です。エディタを使って、翻訳箇所のファイルを編集します。
今回の例の場合、ファイルの場所は、`./src` に、上で示した `Order.mo` を足した、`./src/Order.mo` です。

### 手順 3: 翻訳

はじめに、すでにあるコードを既存のコメントを含めて **全て** コピーし、ファイルの下部にペーストし、コメントアウトします。
コメントアウトは、`/*` と `*/` で囲って行ってください。

```
/*
/// Order

module {

  /// A type to represent an order.
  public type Order = {
    #less;
    #equal;
    #greater;
  };
...

*/
```

その後、コメント部分を翻訳してください。

```

  /// Order を表す型
  public type Order = {
    #less;
    #equal;
    #greater;
  };
```

### 手順 4: 翻訳内容の確認

翻訳した文章を確認するために、ローカルビルドします。`doc` ディレクトリ内で `make_docs.sh` を実行してください。
なお、事項には dfx に付属している `mo-doc` へのパスが通っている必要があります。

```
$ ./make_docs.sh
```

ビルド後、`doc/_out/html` ディレクトリ以下の html ファイルを直接開きます。

```
$ open doc/_out/html/Order.html
```

ブラウザが開き、翻訳が反映されていることが確認できます。

### 手順 5: 翻訳内容のプルリクを出す

翻訳が終わったら、ローカルリポジトリにコミットしたあと、自分のリモートリポジトリにプッシュします。
コミットが複数になった場合、なるべく[１つのコミットにまとめて](https://dev.classmethod.jp/articles/git-rebase-fixup/)いただければありがたいですが、難しければそのままでも OK です。

```
$ git add src/Order.mo
$ git commit -m "translated: Order.mo"
$ git push origin Order.mo
```

最後に、Github から[プルリクを出します](https://qiita.com/samurai_runner/items/7442521bce2d6ac9330b)。
このとき、出し先が Japan-DfinityInfoHub/motoko-base になるようにします。
間違えて本家の dfinity/motoko-base に出してしまわないように気をつけてください。

以上です！メンテナーがレビューをして問題なければマージされます。


The Motoko base library
=======================

This repository contains the Motoko base library. It is intended to be used with the [`moc` compiler](https://github.com/dfinity/motoko) (and tools that wrap it, like `dfx`).

Usage
-----

If you are installing Motoko through the DFINITY SDK releases, then this base
library is already included.

If you build your project using the [vessel package manager] your package-set most likely already includes base, but if it doesn't or you want to override its version add an entry like so to your `package-set.dhall`:

```
  {
    name = "base",
    repo = "https://github.com/dfinity/motoko-base",
    version = "dfx-0.9.2",
    dependencies = [] : List Text
  }
```

The package _name_ `"base"` appears when importing its modules in Motoko (e.g., `import "mo:base/Nat"`).  The _repo_ may either be your local clone path, or this public repository url, as above.  The _version_ can be any git branch name (among other things).  There are no dependencies.  See [vessel package manager] docs for more details.

[vessel package manager]: https://github.com/kritzcreek/vessel

Building/testing
----------------

In `test/`, run

    make

This will expect `moc` to be installed and in your `$PATH`.

Running the tests also requires `wasmtime` and `vessel` to be installed.

If you installed `moc` some other way, you can instruct the `Makefile` to use
that compiler:

    make MOC=moc

Documentation
-------------

The documentation can be generated in `doc/` by running

    ./make_docs.sh

which creates `_out/html/index.html`.

The `next-moc` branch
---------------------

The `next-moc` branch contains changes that make base compatible with the
in-development version of `moc`. This repository's public CI does _not_ run
on that branch.

External contributions are best made against `master`.

Contributing
------------

Please read the [Interface Design Guide for Motoko Base Library](doc/design.md) before making a pull request.
