# VoiceTerm

[English](README.md) · [简体中文](README.zh-CN.md) · [日本語](README.ja.md) · [Español](README.es.md)

**Codex と tmux のための、音声ファーストなターミナル協働。**

VoiceTerm は、AI コーディングアシスタントとユーザーが `tmux` を通じて同じローカルターミナルセッションを扱えるようにします。プロジェクト用の名前付きセッションを開始し、アシスタントにそのセッションの出力確認やタスク実行を依頼する、音声中心の利用を想定しています。

> VoiceTerm は安全なワークフローを標準化するためのものです。サンドボックスではなく、Codex、OS、またはターミナルの権限を迂回することはできません。

## VoiceTerm とは？

iTerm2、Ghostty、または別のターミナルでセッションを表示・操作し続けながら、承認後に Codex が同じ `tmux` セッションの出力を読み取り、コマンドを送信できます。確認フローは音声会話内で進めます。

次のような用途に向いています。

- 音声会話を続けながら、開発、テスト、ログ確認を行う。
- 複数プロジェクト、または一つのプロジェクト内の並行タスクを扱う。
- 明確な音声確認ルールで誤操作を減らす。

## 必要条件

- Skills を利用できる Codex。
- shell を実行できるターミナルエミュレーター。
- インストール済みの `tmux`。

一般的なパッケージマネージャーで tmux をインストールします。

```bash
# macOS（Homebrew）
brew install tmux

# Ubuntu / Debian / WSL Ubuntu
sudo apt install tmux

# Fedora
sudo dnf install tmux
```

Windows では **WSL** 内で tmux を実行してください。ネイティブの Windows PowerShell 自体は tmux 環境ではありません。PowerShell は macOS、Linux、または WSL 内の shell として利用できます。

## VoiceTerm のインストール

このリポジトリをクローンし、Skill フォルダーを Codex のグローバル Skills ディレクトリへコピーします。

```bash
git clone git@github.com:houyongsheng/voiceterm-skill.git
cd voiceterm-skill
mkdir -p ~/.codex/skills
cp -R skills/voice-term ~/.codex/skills/voice-term
```

Windows では同じコマンドを WSL で実行してください。インストール後に新しい Codex タスクを開始します。Skill が検出されない場合は Codex を再起動してください。

## クイックスタート

対象プロジェクトのルートでターミナルを開き、`project-purpose` 形式のセッションを作成します。

```bash
tmux new -s hsk3-web
```

次に Codex へ「VoiceTerm を使って `hsk3-web` を操作して」と依頼します。

既存セッションに接続する場合：

```bash
tmux attach -t hsk3-web
```

用途の接尾辞には `web`、`api`、`test`、`log`、`fix` のような分かりやすい語を使います。同じ用途で別のセッションが必要なら、`hsk3-test-2` のように短い番号を追加します。

## 複数のプロジェクトとターミナル

| 用途 | 推奨セッション名 |
| --- | --- |
| `hsk3` のフロントエンド作業 | `hsk3-web` |
| `hsk3` のテスト | `hsk3-test` |
| `hsk3` のサービスログ | `hsk3-log` |
| 二つ目のテストタスク | `hsk3-test-2` |

VoiceTerm はプロジェクト名と用途をセッション名に照合します。対象が曖昧な場合は、推測せず確認します。

## 安全モデル

- 対象セッションの選択後は、通常の読み取り確認を実行できます。
- プロジェクトの変更、プロセス制御、テスト、依存関係のインストールには、明確な音声確認が必要です。
- 破壊的操作、Git 履歴変更、push、デプロイ、アカウント変更、機密データへのアクセスには、操作ごとの具体的な確認が必要です。
- パスワード、ワンタイムコード、リカバリーコード、API キーは常にユーザー自身が入力します。
- Codex と OS の権限プロンプトはユーザーが管理します。
- コマンド名の接頭辞で広範囲に許可しないでください。プロジェクト内読み取り、指定公開ドメインからの取得、プロジェクトのテスト実行など、効果に応じて許可します。

## リポジトリ構成

```text
skills/voice-term/   インストール可能な VoiceTerm Skill
README.md            English guide
README.zh-CN.md      简体中文指南
README.ja.md         日本語ガイド
README.es.md         Guía en español
```

## ステータス

これはソースからインストールする初期リリースです。さらに多くのターミナルとプラットフォームでワークフローを検証した後、パッケージ化されたプラグイン配布を追加できます。
