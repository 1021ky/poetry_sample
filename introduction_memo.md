# poetryを試したときのメモ

## 参照したドキュメント

https://python-poetry.org/docs/

## 試した環境

* PC: MacBook Pro (Retina, 13-inch, Early 2015)
* zsh:5.8 (x86_64-apple-darwin20.0)
* pyenv: 1.2.26

## 事前にやったこと

python3.9.1で試したかったので、pyenvで設定

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % pyenv local 3.9.1
vaivailx@MacBook-Pro-2 poetry_sample % python -V
Python 3.9.1
vaivailx@MacBook-Pro-2 poetry_sample %
```

githubでリポジトリを作成して、gitignoreはpython向けに作成した。
pyenv local 3.9.1って実行したらできる.python-versionってこの.gitignoreだとデフォルトでは無視するようになっているんだね。

## [Installation](https://python-poetry.org/docs/#installation)

書いてあるとおりにインストール

```zsh
curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
```

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % curl -sSL https://raw.githubusercontent.com/python-poetry/poetry/master/get-poetry.py | python -
Retrieving Poetry metadata

# Welcome to Poetry!

This will download and install the latest version of Poetry,
a dependency and package manager for Python.

It will add the `poetry` command to Poetry's bin directory, located at:

$HOME/.poetry/bin

This path will then be added to your `PATH` environment variable by
modifying the profile files located at:

$HOME/.profile
$HOME/.zshrc
$HOME/.bash_profile

You can uninstall at any time by executing this script with the --uninstall option,
and these changes will be reverted.

Installing version: 1.1.6
  - Downloading poetry-1.1.6-darwin.tar.gz (51.82MB)

Poetry (1.1.6) is installed now. Great!

To get started you need Poetry's bin directory ($HOME/.poetry/bin) in your `PATH`
environment variable. Next time you log in this will be done
automatically.

To configure your current shell run `source $HOME/.poetry/env`

vaivailx@MacBook-Pro-2 poetry_sample %
```

インストール時の文言を読むと、

> It will add the `poetry` command to Poetry's bin directory, located at:
>
> $HOME/.poetry/bin

ホームディレクトリの下にpoetryコマンドのバイナリが置かれると。。

中身を見てみると、シンプルなmain処理を記述したpythonファイルだった。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % cat ~/.poetry/bin/poetry
#!/usr/bin/env python3
# -*- coding: utf-8 -*-
import glob
import sys
import os

lib = os.path.normpath(os.path.join(os.path.realpath(__file__), "../..", "lib"))
vendors = os.path.join(lib, "poetry", "_vendor")
current_vendors = os.path.join(
    vendors, "py{}".format(".".join(str(v) for v in sys.version_info[:2]))
)

sys.path.insert(0, lib)
sys.path.insert(0, current_vendors)

if __name__ == "__main__":
    from poetry.console import main

    main()
vaivailx@MacBook-Pro-2 poetry_sample %
```

> This path will then be added to your `PATH` environment variable by
> modifying the profile files located at:
>
> $HOME/.profile
> $HOME/.zshrc
> $HOME/.bash_profile

環境変数PATHにpoetry実行できるようにパスを足しといたとのこと。

見てみた。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % grep poetry $HOME/.profile
grep: /Users/vaivailx/.profile: No such file or directory
vaivailx@MacBook-Pro-2 poetry_sample % grep poetry $HOME/.zshrc
export PATH="$HOME/.poetry/bin:$PATH"
vaivailx@MacBook-Pro-2 poetry_sample % grep poetry $HOME/.bash_profile
export PATH="$HOME/.poetry/bin:$PATH"
vaivailx@MacBook-Pro-2 poetry_sample %
```

.profileはもともとなかったので、わざわざ作って追記まではしないよう。
ファイルがあったら追記してくれるのか。

> You can uninstall at any time by executing this script with the --uninstall option,
> and these changes will be reverted.

このスクリプト（get-poetry）に、--uninstallオプションをつけて実行すればアンイストールできると。

> Poetry (1.1.6) is installed now. Great!

ほんとに1.1.6なのか試してみる。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % poetry --version
zsh: command not found: poetry
```

コマンドがない。まだzshrcに書き込んだものが反映されていないからかな。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % source ~/.zshrc
vaivailx@MacBook-Pro-2 poetry_sample % poetry --version
Poetry version 1.1.6
vaivailx@MacBook-Pro-2 poetry_sample %
```

Great!たしかに1.1.6。

```zsh
> To get started you need Poetry's bin directory ($HOME/.poetry/bin) in your `PATH`
> environment variable. Next time you log in this will be done
> automatically.
>
> To configure your current shell run `source $HOME/.poetry/env`
```

最後に書いていた。今のシェルでパスを設定したいなら、sourceコマンドを打てと。

$HOME/.poetry/envの中身は？

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % cat ~/.poetry/env
export PATH="$HOME/.poetry/bin:$PATH"%
vaivailx@MacBook-Pro-2 poetry_sample %
```

.zshrcに追記されたものと同じ内容。
シンプルにpoetryのパスを追加することだけできるように、このファイルがあるのだろうか？

# TODO poetryのenvファイルの役割について調べてみる

インストールは終わったのでドキュメントを読みすすめる。

> You only need to install Poetry once. It will automatically pick up the current Python version and use it to create virtualenvs accordingly.

poetryは1回インストールするだけでよいと。
いい感じに今使っているpythonバージョンを検出して、そのPythonで仮想環境をつくってくれると。
ちょっとよくわからない。
詳細は[こちら](https://python-poetry.org/docs/managing-environments/)とある

TODO https://python-poetry.org/docs/managing-environments/ を読む

次、[Installing with pip](https://python-poetry.org/docs/#installing-with-pip)を読むと、どうやらpipでもpoetryをインストールできるらしい。

けれども、他のパッケージの依存関係とコンフリクトする可能性があるため、気をつけなさいとのこと。

[Installing with pipx](https://python-poetry.org/docs/#installing-with-pipx)を読むと、pipxでもインストールできるらしい。
pipxだと、仮想環境とは独立したところにインストールできるため、他のパッケージとのコンフリクトを気にしなくて良いとのこと。

### インストールまとめ

インストールする方法で推奨されるのは、

* [osx / linux / bashonwindows install instructions](https://python-poetry.org/docs/#osx-linux-bashonwindows-install-instructions)で行う、get-poetry.pyを使ったインストール
* [Installing with pipx](https://python-poetry.org/docs/#installing-with-pipx)で行う、pipxを使ったインストール

パスの設定はよしなにしてくれるけど、すぐpoetryコマンドを実行したいならばsourceコマンドでPATHを通そう。

## [Updating poetry](https://python-poetry.org/docs/#updating-poetry)

Poetryのアップデートは以下のコマンドで行う。

```zsh
poetry self update
```

オプションを指定することでpreviewバージョンや特定のバージョンにアップデートできる

## [Enable tab completion for Bash, Fish, or Zsh](https://python-poetry.org/docs/#enable-tab-completion-for-bash-fish-or-zsh)

poetryコマンドのサブコマンドの補完はbash, zsh, fishそれぞれに用意されている。

## Enable tab completion for Bash, Fish, or Zsh

シェルの種類ごとに補完を有効にする方法が書いてある。

zshは以下

```zsh
poetry completions zsh > ~/.zfunc/_poetry
```

あと、zshの場合は追加で以下のことをする必要があるとのこと

> For zsh, you must then add the following line in your ~/.zshrc before compinit

> fpath+=~/.zfunc

言われたとおり書き込んで、その後、sourceで読み込んだら、
tabで補完が聞くようになった。

fpathってなんだろ？って思ったけど、[こちら](https://qiita.com/yuku_t/items/77c23390e52168a2754a)で紹介されていた。fpathはautoloadコマンドが探索するディレクトリを定義しておくもの。

ここまででインストールのドキュメントは終わり。
