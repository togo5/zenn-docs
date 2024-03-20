---
title: ".devcontainer/Dockerfile 内で npm install すると not found となる"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["docker", "vscode"]
published: false
---

# 問題

以下のようなディレクトリ構成のときに Dockerfile 内で `RUN npm install` などと記述するとビルド時にエラーとなることがあります。

```shell
npm ERR! enoent ENOENT: no such file or directory, open '/hoge/package.json'
```

```
hoge
├ .devcontainer
│ ├ devcontainer.json
│ └ Dockerfile
├ src
│ └ ...
└ package.json
```



# 原因

デフォルトではビルドコンテキストが `Dockerfile` と同じディレクトリとなります。
そのため、上位のディレクトリにあるファイルを読み込むことができません。

# 解決

## VS Code

`.devcontainer.json` に context の設定を加えます。

```json:.devcontainer.json
{
  "build": {
    "context": "..",
    "dockerfile": "Dockerfile"
  }
}
```

## CLI

`-f` オプションを使用して `Dockerfile` と context のパスを指定します。

```shell
docker build -f .devcontainer/Dockerfile .
```

# 参考

https://docs.docker.com/build/building/context/

https://matsuand.github.io/docs.docker.jp.onthefly/develop/develop-images/dockerfile_best-practices/#%E3%83%93%E3%83%AB%E3%83%89%E3%82%B3%E3%83%B3%E3%83%86%E3%82%AD%E3%82%B9%E3%83%88%E3%81%AE%E7%90%86%E8%A7%A3

