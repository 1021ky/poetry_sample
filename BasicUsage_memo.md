# poetryを試したときのメモ

## 参照したドキュメント

https://python-poetry.org/docs/basic-usage/

## 試した環境

* PC: MacBook Pro (Retina, 13-inch, Early 2015)
* zsh:5.8 (x86_64-apple-darwin20.0)
* pyenv: 1.2.26

## 事前にやったこと

introduction_memo.mdのとおり

## [Project setup](https://python-poetry.org/docs/basic-usage/#project-setup)

書いてあるとおり、`poetry-demo`プロジェクトを新規作成する。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % ls -la
total 72
drwxr-xr-x   9 vaivailx  staff   288  5  2 11:10 .
drwxr-xr-x  21 vaivailx  staff   672  5  1 15:16 ..
drwxr-xr-x  11 vaivailx  staff   352  5  2 11:10 .git
-rw-r--r--   1 vaivailx  staff  1799  5  1 15:15 .gitignore
-rw-r--r--   1 vaivailx  staff     6  5  1 15:19 .python-version
-rw-r--r--   1 vaivailx  staff  7932  5  2 11:13 BasicUsage_memo.md
-rw-r--r--   1 vaivailx  staff  1073  5  1 15:15 LICENSE
-rw-r--r--   1 vaivailx  staff    53  5  1 15:15 README.md
-rw-r--r--   1 vaivailx  staff  8404  5  2 11:07 introduction_memo.md
vaivailx@MacBook-Pro-2 poetry_sample % poetry new poetry-demo
Created package poetry_demo in poetry-demo
vaivailx@MacBook-Pro-2 poetry_sample % ls -la
total 72
drwxr-xr-x  10 vaivailx  staff   320  5  2 11:14 .
drwxr-xr-x  21 vaivailx  staff   672  5  1 15:16 ..
drwxr-xr-x  11 vaivailx  staff   352  5  2 11:10 .git
-rw-r--r--   1 vaivailx  staff  1799  5  1 15:15 .gitignore
-rw-r--r--   1 vaivailx  staff     6  5  1 15:19 .python-version
-rw-r--r--   1 vaivailx  staff  7932  5  2 11:13 BasicUsage_memo.md
-rw-r--r--   1 vaivailx  staff  1073  5  1 15:15 LICENSE
-rw-r--r--   1 vaivailx  staff    53  5  1 15:15 README.md
-rw-r--r--   1 vaivailx  staff  8404  5  2 11:07 introduction_memo.md
drwxr-xr-x   6 vaivailx  staff   192  5  2 11:14 poetry-demo
vaivailx@MacBook-Pro-2 poetry_sample %
```

poetry-demoというディレクトリができている。

```zsh
vaivailx@MacBook-Pro-2 poetry_sample % tree poetry-demo
poetry-demo
├── README.rst
├── poetry_demo
│   └── __init__.py
├── pyproject.toml
└── tests
    ├── __init__.py
    └── test_poetry_demo.py

2 directories, 5 files
vaivailx@MacBook-Pro-2 poetry_sample %
```

ドキュメントにあるとおりディレクトリとファイルが作られている。
実行したディレクトリ直下に作られるのか。

直下に作りたいときはpathに`.`を指定するようだ。
```zsh
vaivailx@MacBook-Pro-2 hoge % poetry new .
Created package hoge in .
vaivailx@MacBook-Pro-2 hoge % ls -la
total 8
drwxr-xr-x   6 vaivailx  staff  192  5  2 20:28 .
drwxr-xr-x  22 vaivailx  staff  704  5  2 11:25 ..
-rw-r--r--   1 vaivailx  staff    0  5  2 20:28 README.rst
drwxr-xr-x   3 vaivailx  staff   96  5  2 20:28 hoge
-rw-r--r--   1 vaivailx  staff  297  5  2 20:28 pyproject.toml
drwxr-xr-x   4 vaivailx  staff  128  5  2 20:28 tests
vaivailx@MacBook-Pro-2 hoge %
```

gitリポジトリを作りたいときは先に`git init`すればいい？
いやだめだった。
```zsh
vaivailx@MacBook-Pro-2 hoge % ls -la
total 0
drwxr-xr-x   3 vaivailx  staff   96  5  2 20:45 .
drwxr-xr-x  22 vaivailx  staff  704  5  2 20:45 ..
drwxr-xr-x   7 vaivailx  staff  224  5  2 20:45 .git
vaivailx@MacBook-Pro-2 hoge % poetry new .

  RuntimeError

  Destination /Users/vaivailx/work/python/hoge exists and is not empty

  at ~/.poetry/lib/poetry/console/commands/new.py:42 in handle
      38│
      39│         if path.exists():
      40│             if list(path.glob("*")):
      41│                 # Directory is not empty. Aborting.
    → 42│                 raise RuntimeError(
      43│                     "Destination {} "
      44│                     "exists and is not empty".format(path)
      45│                 )
      46│
vaivailx@MacBook-Pro-2 hoge %
```

逆に`poetry new`して、`git init`するのはできた。

```zsh
vaivailx@MacBook-Pro-2 hoge % git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /Users/vaivailx/work/python/hoge/.git/
vaivailx@MacBook-Pro-2 hoge % ls -la
total 8
drwxr-xr-x   7 vaivailx  staff  224  5  3 11:01 .
drwxr-xr-x  22 vaivailx  staff  704  5  2 21:07 ..
drwxr-xr-x   7 vaivailx  staff  224  5  3 11:01 .git
-rw-r--r--   1 vaivailx  staff    0  5  3 11:01 README.rst
drwxr-xr-x   3 vaivailx  staff   96  5  3 11:01 hoge
-rw-r--r--   1 vaivailx  staff  297  5  3 11:01 pyproject.toml
drwxr-xr-x   4 vaivailx  staff  128  5  3 11:01 tests
vaivailx@MacBook-Pro-2 hoge %
```

github WebUIでリポジトリを作成して、git cloneしてから`poetry new`したかったのだけど、forceオプションもないしできないのかな？
[issue](https://github.com/python-poetry/poetry/issues/1422#issuecomment-820964663)も上がっていた。

## [Initialising a pre-existing project](https://python-poetry.org/docs/basic-usage/#initialising-a-pre-existing-project)

あー。既存のプロジェクトでやるときは、`poetry init`なのか。
でもこちらは、pyproject.tomlしか作られないのね。

## [Specifying dependencies](https://python-poetry.org/docs/basic-usage/#specifying-dependencies)

必要なパッケージはpyproject.tomlの`tool.poetry.dependencies`に記述する。
記述方法は、直接ファイルを編集するか、`poetry add`で追記する。
パッケージ名とバージョンを記載する。
`poetry add`をするとついでにインストールまでしてくれる。

パッケージを探すリポジトリは`tool.poetry.repositories`で記述する。デフォルトはPyPI。だからコマンドで自動生成したpyproject.tomlにも`tool.poetry.repositories`で記述する。

## [Using your virtual environment](https://python-poetry.org/docs/basic-usage/#using-your-virtual-environment)

デフォルトでpoetryは仮想環境を{cache-dir}/virtualenvsに作る。
仮想環境のディレクトリはconfigで変更できる。

* `virtualenvs.in-project`をtrueに設定すれば、プロジェクトのディレクトリのrootに`.venv`という仮想環境のディレクトリが作られる。
* `virtualenvs.path`で設定すれば、仮想環境のディレクトリは任意のパスに変更できる。

仮想環境内でコマンドを実行する方法はいくつかある。

* poetry runコマンドを実行する
* 仮想環境を有効化する
  * `poetry shell`コマンドを実行する
  * `source $(poetry env info --path)/bin/activate`コマンドを実行する

poetry runを実行する

```zsh
vaivailx@MacBook-Pro-2 poetry_demo % cat main.py
print("poetry run")
vaivailx@MacBook-Pro-2 poetry_demo % poetry run python main.py
poetry run
vaivailx@MacBook-Pro-2 poetry_demo %
```

poetry shellを実行する

```zsh
vaivailx@MacBook-Pro-2 poetry-demo % poetry shell
Spawning shell within /Users/vaivailx/Library/Caches/pypoetry/virtualenvs/poetry-demo-tILhJv1y-py3.9
vaivailx@MacBook-Pro-2 poetry-demo % . /Users/vaivailx/Library/Caches/pypoetry/virtualenvs/poetry-demo-tILhJv1y-py3.9/bin/activate
(poetry-demo-tILhJv1y-py3.9) vaivailx@MacBook-Pro-2 poetry-demo % exit
vaivailx@MacBook-Pro-2 poetry-demo %
```

source $(poetry env info --path)/bin/activateを実行する

```zsh
vaivailx@MacBook-Pro-2 poetry-demo % source $(poetry env info --path)/bin/activate
(poetry-demo-tILhJv1y-py3.9) vaivailx@MacBook-Pro-2 poetry-demo % deactivate
vaivailx@MacBook-Pro-2 poetry-demo %
```

同じ仮想環境が有効化されている。ただし、poetry shellの出力には、
`Spawning shell within /Users/vaivailx/Library/Caches/pypoetry/virtualenvs/poetry-demo-tILhJv1y-py3.9`とでている。
これが、ドキュメントに書かれているとおり、shellは子プロセスを作成してから仮想環境を有効化するということなのだろう。
`poetry shell`実行後にpstreeで実際にプロセスを見てみると、zshのプロセスが起動してそのプロセスでpstreeを実行していた。

```zsh
(poetry-demo-tILhJv1y-py3.9) vaivailx@MacBook-Pro-2 poetry-demo % pstree | grep poetry -b1 -a2
27591- |   \-+- 07467 vaivailx /Applications/Visual Studio Code.app/Contents/Frameworks/Code Helper (Renderer).app/Contents/MacOS/Code Helper (Renderer) /Applications/Visual Studio Code.app/Contents/Resources/app/out/bootstrap-fork --type=ptyHost
27832- |     \-+= 30692 vaivailx /bin/zsh -l
27871: |       \-+= 02834 vaivailx /usr/local/Cellar/python@3.9/3.9.2_1/Frameworks/Python.framework/Versions/3.9/Resources/Python.app/Contents/MacOS/Python /Users/vaivailx/.poetry/bin/poetry shell
28062- |         \-+= 02838 vaivailx /bin/zsh -i  <-----※作られた子プロセス
28105- |           |-+= 03221 vaivailx pstree
```

sourceコマンドを実行すれば、実行したシェルのプロセスの仮想環境を起動（環境変数を一時的に変更）することができ、そのままpythonのアプリケーションを仮想環境内で実行できる。
しかし、poetryコマンドのプロセスで仮想環境内でpythonのアプリケーションを起動しようとすると、実行したpoetryコマンドは実行したシェルのプロセスの子プロセスなので、実行したシェルの環境変数は変更できない、つまり仮想環境を有効化できない。
そこで、poetry shellコマンド実行後に仮想環境の起動とpythonアプリケーション実行をするためにさらにshellの子プロセス作成して、実行している。

memo

activateは環境変数VIRTUAL_ENVを設定するコマンドだったのをexportで確認した。


## [Version constraints](https://python-poetry.org/docs/basic-usage/#version-constraints)


バージョンの比較は、すべてビルドバージョンまで比較する。マイナーバージョンとビルドバージョンは明記されていなかったら0と判定される。
メジャーバージョンが書かれていたら、メジャーバージョンを一致させたいと解釈される。
メジャーバージョンが0ならば、マイナーバージョンを一致させたいと解釈される。
メジャーバージョンもマイナーバージョンも0ならば、ビルドバージョンを一致させたいと解釈される。

* ^1.2.3と書いていた1.2.3以上で、かつ<2.0.0
* ^1.2と書いていたら、^1.2.0と書いているのと同じで1.2.0以上で、かつ<2.0.0
* ^1と書いていたら、^1.0.0と書いているのと同じで、1.0.0以上で、かつ<2.0.0
* ^0.2と書いていたら、^0.2.0と書いているのと同じで、0.2.0以上で、かつ<0.3.0

他にも依存しているものを、gitリポジトリ、url、ファイルパスなどで記述できる。詳細な書き方は[dependency-specification](https://python-poetry.org/docs/dependency-specification/)参照。

## [Installing dependencies](https://python-poetry.org/docs/basic-usage/#installing-dependencies)

依存しているパッケージのインストールには、poetry installを実行する。

コマンド実行時にpoetry.lockがpyproject.tomlと同じ階層にあるかないかで挙動は変わる。

lockファイルがあれば、lockファイルに書かれているとおりにインストールを行う。もし、なければpyprojectに書かれているとおりにインストールして、lockファイルを生成する。

lockファイルはバージョン管理する。依存するパッケージのバージョンが変わって、環境ごとにアプリケーションの挙動が変わるのを防ぐために。

依存するパッケージを最新化したいときは、poetry updateコマンドを実行する。