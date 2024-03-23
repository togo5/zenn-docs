---
title: ".devcontainer/Dockerfile で npm install して not found となったとき"
emoji: "🔨"
type: "tech" # tech: 技術記事 / idea: アイデア
topics: ["docker", "vscode"]
published: true
---

# 問題

`.devcontainer/Dockerfile` で `RUN npm install` と記述したところビルド時にエラーが発生しました。

```shell
npm ERR! enoent ENOENT: no such file or directory, open '/hoge/package.json'
```

原因はビルドコンテキストがデフォルトの `.devcontainer/` であったため、上位のディレクトリの `package.json` が読み込めなかったためでした。


# 解決策

`.devcontainer/devcontainer.json` に context の設定を加えるとエラーなくビルドされるようになりました。

```json:.devcontainer/devcontainer.json
{
  "build": {
    "context": "..",
    "dockerfile": "Dockerfile"
  }
}
```

# 参考

https://docs.docker.com/build/building/context/

https://containers.dev/implementors/json_reference/