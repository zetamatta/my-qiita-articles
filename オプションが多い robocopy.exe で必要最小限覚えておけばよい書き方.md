---
title: オプションが多い robocopy.exe で必要最小限覚えておけばよい書き方
tags: Windows コマンドプロンプト cmd.exe
author: zetamatta
slide: false
---
robocopy.exe は Windows に標準で導入されており、使いやすいコマンドライン向けファイルコピーコマンドだが、オプションが多くてとっつきが悪い。しかし、実際に覚えるべきオプションは2,3個で十分だ。

```
robocopy SRC DST /option …
```

パスの階層
=========

DST（コピー先）の書き方は、UNIX の `cp` などとは違い、SRC（コピー元）とちょうど 1:1 で対応するようなパス階層とする。

`foo\xxx\ `というフォルダーを `bar\` 以下に `bar\xxx\` という体裁で複製したい時、UNIX ならば

```
cp foo/xxx bar/
```

と書けばよかった（`xxx` は省略でき、cp側で足してくれた）が、robocopy・xcopy は

```
robocopy foo\xxx bar\xxx
```

と **1:1 で対応するファイル名まで、きっちり書かねばならない**。(これは xcopy 以降の伝統。copy は必ずしもそうではない）

日常的に使うオプション
====================

だいたい４つで済む。UNIXとは違う、オプションを末尾につける点に注意！

* **`/E`** … サブディレクトリ以下も再帰的にたどってコピー。同じ機能に `/S` もあるが、`/S` は空のサブディレクトリはコピーしてくれないので、ほとんど使わない。E は「empty もコピーするよ」の E 
* **`/MIR`** … ディレクトリのミラーリング。`/E` の機能を含んでいるが、コピー元に存在しないファイルが、コピー先にあったら「削除」する。
* **`/MOVE`** … 移動。コピー元を削除する。move.exe が使えないシーン（ドライブをまたがるディレクトリ移動）では重宝する。`/E` と組み合わせることが多い。似た機能に `/MOV` があるが、こちらはディレクトリは相手にしてくれないので、あまり使わない。
* **`/?`** ヘルプ。他のオプションはこれで見よう。

使用例
======

ヒストリを検索したがいいのがなかった。以下でご容赦いただきたい

```
$ e:
$ cd e:\Users\Hogehoge
$ robocopy go %USERPROFILE%\go /move /E
```

起動ディスクを変更した際に、他のドライブ(E:)に置き去りになった旧ホームディレクトリの go ディレクトリ以下を、新ホームディレクトリ以下に移動させようとしたらしい。（実際は nyagos 上で書いていたので、 `%USERPROFILE%` は `~` と表現したりしていた）

以上

