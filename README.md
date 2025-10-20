# tutorial-spec-driven

## プロジェクト概要
- 本リポジトリは、エージェントを活用しながら仕様（spec）を起点に開発を進める体験型チュートリアルです。
- プレーヤー自身が開発するお題（テーマ）を選び、仕様策定 → 実装 → 振り返りまでをロール別エージェントで回すことで、spec駆動開発の楽しさとワークフローを学びます。
- ロールごとのプレイブックは `third_party/spec-driven-template/AGENTS.md` から確認できます。まずはこちらを読み、進め方の全体像を把握してください。

## 動作要件
- Python 3.11 以上
- 必要に応じて `pip` などのPythonパッケージマネージャ
- python以外でも開発可能、`pyprojects.toml`, `main.py`, `pythoon-version`を消去してください。

## セットアップ
1. リポジトリをクローンします。
   ```bash
   git clone <このリポジトリのURL>
   cd tutorial-spec-driven
   ```
2. third_partyにspec-driven-templateをclone
   ```bash
   cd third_party
   git clone https://github.com/Motoki0705/spec-driven-template
   ```
3. third_partyの.gitを消去し、親repoに統合
   ```bash
   rm -rf third_party/spec-driven-template
   git add third_party/spec-driven-template
   git commit -m "Vendor third_party/spec-driven-template"
   ```
4. codexをインストールしていない場合はインストール1
   ```bash
   npm i -g @openai/codex  # インストール 
   codex                   # 起動
   ```


## uvでの環境構築と実行
- まだ`uv`をインストールしていない場合は以下のコマンドを実行してください。
  ```bash
  # macOS / Linux
  curl -LsSf https://astral.sh/uv/install.sh | sh
  # Windows (PowerShell)
  powershell -ExecutionPolicy ByPass -c "irm https://astral.sh/uv/install.ps1 | iex"
  ```
- インストール後は最新版へ更新しておくと安心です。
  ```bash
  uv self update
  ```
- 仮想環境の作成:
  ```bash
  uv venv
  source .venv/bin/activate  # Windowsの場合は .venv\Scripts\activate
  ```
- 依存関係の同期 (`pyproject.toml` / `uv.lock` を利用):
  ```bash
  uv sync
  ```
- `pyproject.toml` では PyTorch 系ライブラリを CPU 版ホイールから取得し、`torch>=2.3`（CPU ビルド）と `numpy<2` を固定しています。追加のオプションなしで `uv sync` / `uv run` を実行すれば CPU 互換の依存関係をまとめて導入できます。
- `example.md` の実装を検証するスクリプトを実行する際は `uv run` を利用すると依存関係が揃った環境でコマンドを実行できます。
  ```bash
  uv run python dinov3_demo.py
  # 実装が進めば uv run python path/to/your_script.py で example.md に沿った処理を確認
  ```
- 依存パッケージを更新した場合は `uv sync` を再実行して環境を最新化してください。

## hugging faceの認証方法
- example.mdを実装したい場合はhugginf faceの認証が必要です。  
- セットアップ方法を [hugging_face_setup.md](docs/hugging_face_setup.md)にまとめました。  

## チュートリアルの進め方
1. `codex` を起動し、セットアップ手順のステップ5に記載したプロンプト  
   ```
   あなたはplannerです。example.mdを実装したいです。 @third_partys/pec-driven-template/docs/SETUP.md に従ってセットアップを完了してください。
   ```
   をそのまま投入して planner ロールを開始します。
2. planner は `third_party/spec-driven-template/AGENTS.md` と関連ドキュメントを参照しながら `example.md` の要件を整理し、`spec/` や `state/` に必要な初期データを書き込みます。作業ログは `state/trace/`、結果は `state/reports/` に残してください。
3. planner が準備したチェックポイントを受け取り、coder ロールで実装を進めます。`uv run python ...` を用いた検証やテスト実行をこまめに行い、仕様との差異があれば都度 planner へフィードバックします。
4. coder の成果物を judge ロールでレビューし、必要に応じて planner/coder と対話しながら修正を進めます。問題がなければ `state/tickets/` を `Done` へ更新し、振り返りを記録してチュートリアル完了です。

## ディレクトリガイド
- `main.py` : 動作確認用の最小スクリプト。
- `pyproject.toml` : プロジェクトのメタデータ。
- `third_party/spec-driven-template/` : エージェント運用テンプレート集。spec/state編集時の参照元。
- `AGENTS.md` : ロール別ドキュメントへの導線。最初に開き、読み進め順を確認してください。

## さいごに
お題の設計から振り返りまでを自分のペースで回し、spec駆動開発とエージェント協働の面白さを味わってください。繰り返すほど、仕様作成と状態管理の勘所が身についていきます。
