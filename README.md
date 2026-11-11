# Reusable workflows for m0t0k1ch1-go

m0t0k1ch1-go 配下の Go リポジトリで共有する、再利用可能なワークフロー。

## ワークフロー

- `.github/workflows/lint.yaml`：依存をダウンロードして検証し（`go mod download`、`go mod verify`）、`go vet` と staticcheck を実行する。
- `.github/workflows/test.yaml`：依存をダウンロードして検証し、`go test` を `-race` を付けて実行して、カバレッジを Codecov にアップロードする。

## 呼び出し側の要件

- リポジトリのルートに `go.mod` と `go.sum` を置く。Go のバージョンは `go.mod` から読み込む。
- staticcheck を `go.mod` に tool として登録する（`go get -tool honnef.co/go/tools/cmd/staticcheck`）。`lint.yaml` は、staticcheck を `go tool staticcheck` で実行する。
- `test.yaml` を呼び出すジョブに、`id-token: write` の権限を与える。Codecov へのアップロードに OIDC を使うためである。呼び出されたワークフローは、呼び出し側が与えた権限を超える権限を持てない。

## 使い方

各リポジトリの `.github/workflows/` に、以下の呼び出し用のワークフローを置く。

`lint.yaml`：

```yaml
name: lint
on:
  push:
    branches:
      - "**"
jobs:
  lint:
    uses: m0t0k1ch1-go/workflows/.github/workflows/lint.yaml@main
```

`test.yaml`：

```yaml
name: test
on:
  push:
    branches:
      - "**"
jobs:
  test:
    permissions:
      id-token: write
    uses: m0t0k1ch1-go/workflows/.github/workflows/test.yaml@main
```

参照先は `main` ブランチとする。そのため、本リポジトリの `main` ブランチへの変更は、すべての呼び出し側リポジトリの CI に即座に反映される。本リポジトリのワークフローを変更するときは、マージする前に、呼び出し側リポジトリの作業ブランチで参照先を本リポジトリの作業ブランチに変え、CI が成功することを確認する。
