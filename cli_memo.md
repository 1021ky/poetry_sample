# poetryを試したときのメモ(Commands)

頻繁に使いそうなものだけピックアップしてメモ

## 参照したドキュメント

https://python-poetry.org/docs/cli/

## 試した環境

* PC: MacBook Pro (Retina, 13-inch, Early 2015)
* zsh:5.8 (x86_64-apple-darwin20.0)
* pyenv: 1.2.26

## 事前にやったこと

BasicUsage_memo.mdのとおり

## [install](https://python-poetry.org/docs/cli/#install)

* `poetry install` :pyproject.tomlに書かれている依存パッケージを全部インストールするとき
* `poetry install --no-dev`: development dependenciesに書かれている依存パッケージはインストールしないとき。
* `poetry install --remove-untracked`: lockファイルには書かれているけど、不要になった古い依存パッケージを削除するときに使う。
* `poetry install --no-root`: pypoetry.tomlをおいているプロジェクトはパッケージとしてインストールしないときに使う

## [run](https://python-poetry.org/docs/cli/#run)

* `poetry run python ***`: 指定したPythonスクリプトを仮想環境内で実行する
* `poetry run ***`: pyproject.tomlに定義してあるスクリプトを実行する。

poetry runで実行するスクリプトの定義は、pyproject.tomlの[tool.poetry.scripts]で行う。
サンプルは以下。

```zsh
vaivailx@MacBook-Pro-2 poetry-demo % cat poetry_demo/main.py
def main():
        print("poetry run")

if __name__ == "__main__":
        main()
vaivailx@MacBook-Pro-2 poetry-demo % poetry run my-scripts
poetry run
vaivailx@MacBook-Pro-2 poetry-demo %
```

アプリケーションのエントリーポイントはどこかとか説明せず、開発者には`poetry run ***`したら動くとだけ言っておけばいいようにできるのがいいのかな？

## [check](https://python-poetry.org/docs/cli/#check)

pyproject.tomlの構文チェックをしてくれる。
CIで定期的に動かしておくといいかも。

## [export](https://python-poetry.org/docs/cli/#export)

lockファイルを指定したフォーマットに変換して出力できる。
現時点では、フォーマットはrequirements.txtにしかできない。
ファイル名は自由に指定できる。

--devオプションを付ければdevelopment dependenciesの内容も含めることができる。

pipしか使っていない開発者も使えるのでいいね。あとは、コンテナイメージを作るときも使えそうかな。


## [env](https://python-poetry.org/docs/cli/#env)

* `poetry env info`: 仮想環境の詳細な情報を出力する

poetry run python -Vを実行したら期待していないpythonのバージョンになっていて原因を知りたいときに便利。

[managing-environments](https://python-poetry.org/docs/managing-environments/)にかかれているのだけど、pyenvと組み合わせるときは、pyenv localを実行してから、poetryを使うらしい。

例には以下のようにあるので、試してみる。

```zsh
pyenv install 2.7.15
pyenv local 2.7.15  # Activate Python 2.7 for the current project
poetry install
```

3.8.1でためす。

```zsh
vaivailx@MacBook-Pro-2 ssss % pyenv local 3.8.1
vaivailx@MacBook-Pro-2 ssss % poetry install -v
Creating virtualenv ssss-5cr1lhoe-py3.9 in /Users/vaivailx/Library/Caches/pypoetry/virtualenvs
Using virtualenv: /Users/vaivailx/Library/Caches/pypoetry/virtualenvs/ssss-5cr1lhoe-py3.9
Installing dependencies from lock file

Finding the necessary packages for the current system

Package operations: 5 installs, 0 updates, 0 removals

  • Installing certifi (2020.12.5)
  • Installing chardet (4.0.0)
  • Installing idna (2.10)
  • Installing urllib3 (1.26.4)
  • Installing requests (2.25.1)
vaivailx@MacBook-Pro-2 ssss % poetry run python -V
Python 3.9.2
vaivailx@MacBook-Pro-2 ssss %
```

なぜか3.9.2。これsystemで入っているpythonなんだよな。。なんでだろ？
ドキュメントに書かれている動きと違う。

```zsh
vaivailx@MacBook-Pro-2 ssss % poetry env info

Virtualenv
Python:         3.9.2
Implementation: CPython
Path:           /Users/vaivailx/Library/Caches/pypoetry/virtualenvs/ssss-5cr1lhoe-py3.9
Valid:          True

System
Platform: darwin
OS:       posix
Python:   /usr/local/Cellar/python@3.9/3.9.2_1/Frameworks/Python.framework/Versions/3.9
vaivailx@MacBook-Pro-2 ssss %
```

ちなみにpyproject.tomlはこう

```zsh
vaivailx@MacBook-Pro-2 ssss % cat pyproject.toml
[tool.poetry]
name = "ssss"
version = "0.1.0"
description = ""
authors = ["k_yamanaka <vaivailx+git@gmail.com>"]

[tool.poetry.dependencies]
python = "^3.8"
requests = "^2.25.1"

[tool.poetry.dev-dependencies]

[build-system]
requires = ["poetry-core>=1.0.0"]
build-backend = "poetry.core.masonry.api"
vaivailx@MacBook-Pro-2 ssss %
```

pyenvで3.8.1の仮想環境を作ると、3.8.1になる。

```zsh
vaivailx@MacBook-Pro-2 ssss % pyenv virtualenv-delete poetry_ssss
pyenv-virtualenv: remove /Users/vaivailx/.pyenv/versions/3.8.1/envs/poetry_ssss? y
vaivailx@MacBook-Pro-2 ssss % pyenv virtualenv 3.8.1 poetry_ssss
Looking in links: /var/folders/3c/93rbry1s0nq0rygk72hjrckc0000gn/T/tmpn1lfysuh
Requirement already satisfied: setuptools in /Users/vaivailx/.pyenv/versions/3.8.1/envs/poetry_ssss/lib/python3.8/site-packages (41.2.0)
Requirement already satisfied: pip in /Users/vaivailx/.pyenv/versions/3.8.1/envs/poetry_ssss/lib/python3.8/site-packages (19.2.3)
vaivailx@MacBook-Pro-2 ssss % pyenv local poetry_ssss
vaivailx@MacBook-Pro-2 ssss % poetry install
Updating dependencies
Resolving dependencies... (0.4s)

Writing lock file

Package operations: 5 installs, 0 updates, 0 removals

  • Installing certifi (2020.12.5)
  • Installing chardet (4.0.0)
  • Installing idna (2.10)
  • Installing urllib3 (1.26.4)
  • Installing requests (2.25.1)
vaivailx@MacBook-Pro-2 ssss % poetry run python -V
Python 3.8.1
vaivailx@MacBook-Pro-2 ssss % poetry env info

Virtualenv
Python:         3.8.1
Implementation: CPython
Path:           /Users/vaivailx/.pyenv/versions/3.8.1/envs/poetry_ssss
Valid:          True

System
Platform: darwin
OS:       posix
Python:   /Users/vaivailx/.pyenv/versions/3.8.1/Python.framework/Versions/3.8
vaivailx@MacBook-Pro-2 ssss %
```

