# github-actions-exercise

GitHub Actions のワークフロー/Dev Container 構成を題材に、静的解析（lint）や Dev Container CI を動かすための練習用リポジトリです。

主に以下を扱います。

- GitHub Actions ワークフローの lint（`actionlint` / `ghalint` / `zizmor`）
- Dev Container のビルド検証（GitHub Actions から `devcontainers/ci` を実行）
- `Taskfile.yml` によるタスク実行

## 必要なもの

- `task`（go-task）
- lint ツール群：`actionlint` / `ghalint` / `zizmor`
- Dockerfile lint（CI で使用）：`hadolint`
- そのほか（環境確認で使用）：`git`, `ssh`

このリポジトリでは Dev Container での利用を想定しており、Dev Container 内では `aqua` により必要な CLI がインストールされます（設定は `.devcontainer/github-actions/aqua.yaml`）。

Dev Container を使わずに実行する場合は、上記ツールをそれぞれインストールして `PATH` に通した上で `task environment:check` が通る状態にしてください。

## セットアップ（推奨: Dev Container）

1. Dev Container 用の環境変数ファイルを作成する

   ```bash
   cp .devcontainer/.env.example .devcontainer/.env
Dev Container の設定は `.devcontainer/github-actions/devcontainer.json` にあります。

## 使い方

### タスク一覧

利用可能なタスクは次で確認できます。

```bash
task
# または
task --list-all --sort none
```

### 環境チェック

必要なツールが揃っているか確認します。

```bash
task environment:check
```

### Lint

リポジトリ内の GitHub Actions 関連ファイルを lint します。

```bash
task lint
```

個別実行したい場合は以下を使います。

```bash
task lint:gha:actionlint
task lint:gha:ghalint
task lint:gha:zizmor
```

## CI（GitHub Actions）

ワークフローは `.github/workflows/run_devcontainer_ci.yaml` にあります。

- `.devcontainer/**` または当該ワークフローが変更されたときに動作します
- `hadolint` で Dockerfile をチェックします
- `devcontainers/ci` で Dev Container をビルドし、コンテナ内で `task environment:check` を実行します

## 補足

- Dev Container でインストールされるツールのバージョンは `.devcontainer/github-actions/aqua.yaml` を参照してください。
- `Taskfile.yml` はワークフロー lint を中心に最小構成です。必要に応じてタスクを追加してください。
