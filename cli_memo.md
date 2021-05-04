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


