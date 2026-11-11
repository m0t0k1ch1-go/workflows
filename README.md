# Reusable workflows for m0t0k1ch1-go

m0t0k1ch1-go 配下の Go リポジトリで共有する、再利用可能なワークフロー。

## 使い方

各リポジトリの `.github/workflows/` に、以下の呼び出し用のワークフローを置く。また、`lint.yaml` は staticcheck を `go tool staticcheck` で実行するため、各リポジトリでは `go tool staticcheck` を実行できるようにしておく。

### `lint.yaml`

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

### `test.yaml`

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
