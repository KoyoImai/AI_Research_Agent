# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude CodeとCodexで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境

## 現状の疑問
### 疑問1
VS Codeの拡張機能にあるClaude CodeやCodexを使うのは可能なのか？
ローカルpc（手元のmac book）でDGX Sparkにssh接続して，ローカルpcのVS CodeでClaude CodeとCodexを起動していると，ローカルpcを閉じたらAgentの稼働も止まるのでは？

### 回答1
一旦CLIで環境を構築し，VS Codeの拡張機能は後から確かめる．

### 疑問2
gitやarXivなどの接続はどうする？

### 回答2
MCPサーバーを導入予定．

### 

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## [ステップ3：シンプルなAgentの構築と動作確認（ABNで実行中）](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step3)

## ステップ4：Codex CLI のインストール・動作確認

### Codex CLIのインストールと起動
まず，Codex CLIのインストールを行います．
以下のコマンドを，ターミナル上で実行してください．
```
npm i -g @openai/codex
```
インストールが完了したら，バージョン確認で以下を実行してください．
```
mprg@spark-09ef:~/research$ codex --version
codex-cli 0.124.0
```
バージョンを確認したら，実際にCodexを起動します．
以下のコマンドを実行してください．プロジェクトはClaude Codeと共通にするため，`~/research/project1`に移動します．
```
cd ~/research/project1
codex
```
codexを起動後，以下の画面が現れます．
ChatGPT Plusに登録しているので，`1`を選択して先に進みます．
```
  Welcome to Codex, OpenAI's command-line coding agent

  Sign in with ChatGPT to use Codex as part of your paid plan
  or connect an API key for usage-based billing

> 1. Sign in with ChatGPT
     Usage included with Plus, Pro, Business, and Enterprise plans

  2. Sign in with Device Code
     Sign in from another device with a one-time code

  3. Provide your own API key
     Pay for what you use

  Press Enter to continue
```
`1`を選択後，メールアドレスやパスワードを求められるので，自身のアカウントでログインしてください．
ログインが完了するとターミナルに以下の画面が表示されます．
ログインが完了したことを示す画面なので，そのまま`Enter`を押して先に進みます．
```
  Welcome to Codex, OpenAI's command-line coding agent

✓ Signed in with your ChatGPT account

  Before you start:

  Decide how much autonomy you want to grant Codex
  For more details see the Codex docs

  Codex can make mistakes
  Review the code it writes and commands it runs

  Powered by your ChatGPT account
  Uses your plan's rate limits and training data preferences

  Press Enter to continue
```
その後，以下のような確認画面が出ます．
`1`を選択して先に進みます．
```
  Welcome to Codex, OpenAI's command-line coding agent

✓ Signed in with your ChatGPT account

> You are in /home/mprg/research/project1

  Do you trust the contents of this directory? Working with untrusted contents comes with higher risk of prompt injection.
  Trusting the directory allows project-local config, hooks, and exec policies to load.

› 1. Yes, continue
  2. No, quit

  Press enter to continue
```
これでCodexの起動が完了し，いかのような画面が表示されます．
```
⚠ Codex's Linux sandbox uses bubblewrap and needs access to create user namespaces.

╭───────────────────────────────────────╮
│ >_ OpenAI Codex (v0.124.0)            │
│                                       │
│ model:     gpt-5.4   /model to change │
│ directory: ~/research/project1        │
╰───────────────────────────────────────╯

  Tip: GPT-5.5 is now available in Codex. It's our strongest agentic coding model yet, built to reason through large codebases,
  check assumptions with tools, and keep going until the work is done.

  Learn more: https://openai.com/index/introducing-gpt-5-5/

 
› Find and fix a bug in @filename
 
  gpt-5.4 default · ~/research/project1
```
上記画面の`› Find and fix a bug in @filename`とある部分がチャット欄です．
試しに`CLAUDE.mdを読んで，このプロジェクトの概要を説明してください．`と入力して，Claude Codeで行なったABNのプロジェクトを共有可能か確かめます．
以下のような確認画面がでるので，Claude Codeと同様に許可をして処理を実行させます．その後もファイルの読み取り許可なども求められると思いますが，基本的に許可を出して進めいきます．
```
 
  Would you like to run the following command?
 
  Reason: Do you want to allow read-only shell commands so I can inspect CLAUDE.md and summarize this project?
 
  $ find . -maxdepth 2 -type f | sort
 
› 1. Yes, proceed (y)
  2. Yes, and don't ask again for commands that start with `sort` (p)
  3. No, and tell Codex what to do differently (esc)
```
最終的には，以下のように確認ができていることが確かめられます．
表示された画面の一部抜粋です．
```
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• CLAUDE.md だけでも方針は読めました。実装の進み具合を補足するため、experiments/ と outputs/ の中だけ軽く確認します。

✔ You approved codex to run find experiments -maxdepth 3 -print | sort this time

✔ You approved codex to run find outputs -maxdepth 3 -print | sort this time

• Explored
  └ List experiments
    List outputs

──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────

• CLAUDE.md を読む限り、このプロジェクトは Attention Branch Network（ABN）を土台にして、研究の試行錯誤を半自動化する AI Agent を作
  る ものです。狙いは「仮説を立てる → 実装して実験する → 結果を分析して改善する」という研究ループを、エージェント主導で回せるよう
  にすることです。

  現在の研究対象は ABN 論文（arXiv:1812.10025） で、フェーズは 実装・動作確認、データセットは CIFAR-10 を第一優先、CIFAR-100 を第
  二優先 としています。運用面では DGX Spark 上の research-dev コンテナを前提にしていて、重い実験は避けて、まず軽量データセットで高
  速に回す方針です。

  このプロジェクトの特徴は、単なる実装リポジトリではなく、研究プロセス自体にルールを設けている 点です。特に重要なのは次の運用で
  す。

  - 実験前に必ず experiments/exp_NNN/design.md を作る
  - ユーザー承認なしに実験を走らせない
  - 実験報告では次の問いや改善案を複数出す
  - 新しく読んだ論文は papers/ に要約を残す
  - ImageNet は最後で、まず CIFAR 系で回す

  ディレクトリ構成もその研究フローに沿っています。

  - papers/: 論文 PDF と要約。実際に papers/abn_summary.md があります
  - experiments/exp_NNN/: 実験単位の作業場。design.md、run.py、results/ を持つ構成
  - outputs/figures/, outputs/notes.md: 可視化や横断メモ
  - .claude/commands/: Claude 用のカスタムコマンド置き場として想定

  実際の進捗としては、experiments/exp_001 と exp_002 がすでに存在し、run.py や results/、学習ログ、チェックポイント、比較画像など
  もあるので、構想段階ではなく、ABN の実装とベースライン比較を進めている途中の研究プロジェクト と見てよいです。

  必要なら次に、exp_001 と exp_002 の違いまで読んで、研究の現状をもう少し具体的に整理します。

 
› Find and fix a bug in @filename
 
  gpt-5.4 default · ~/research/project1
```

### AGENTS.mdの作成

**[参考1：Codexで使うAGENTS.mdの役割と実務](https://qiita.com/jtths474/items/4dbaeb37847fea67a502)**

Claude CodeにCLAUDE.mdがあるように，CodexにはAGENTS.mdがあります．
AGENTS.mdは，Codexのエージェントに向けたREADMEであり，ビルドやテスト手順，コード規約など，Agentが作業を行ううえで必要な指示をあらかじめ設定します．

まず，AGENTS.mdの初期化から行なっていきます．
以下のコマンドをCodex CLIのチャット欄で実行してください．
実行後いくつかの確認を求められます．内容を確認して処理の許可などを出してください．
```
\init
```
実行後以下のような内容でAGENTS.mdが作成されます．
今回は，CLAUDE.mdの内容に沿ったAGENTS.mdが作成されました．
```
# Repository Guidelines

## プロジェクト構成とモジュール配置
このリポジトリは再利用可能な Python パッケージではなく、ABN の研究実験を管理するための作業領域です。作業は実験単位で整理してください。

- `experiments/exp_NNN/`: 実験ごとのディレクトリ。`design.md`、`run.py` や `optimize.py` などの実行スクリプト、ログ・チェックポイント・図を保存する `results/` を含みます。
- `papers/`: 論文 PDF と読書メモ。例: `abn.pdf`, `abn_summary.md`
- `outputs/`: 実験横断のメモや共通の図
- `Dockerfile`: 研究用コンテナ定義
- `CLAUDE.md`: このリポジトリで動くエージェント向け運用ルール

新しい作業は `experiments/exp_003/` のように、次の番号のディレクトリを作って追加します。

## ビルド・テスト・開発コマンド
実行は `CLAUDE.md` に記載の常駐 Docker コンテナを前提とします。

- `docker exec research-dev python /workspace/project1/experiments/exp_001/run.py`: 特定実験を実行します。
- `docker exec research-dev python /workspace/project1/experiments/exp_002/optimize.py`: 最適化や追加分析を実行します。
- `docker exec research-dev python -m py_compile /workspace/project1/experiments/exp_002/run.py`: 実行前の簡易構文チェックです。

`/workspace/project1/...` のパスを正しく使うため、可能な限りコンテナ内でコマンドを実行してください。

## コーディング規約と命名
既存の実験スクリプトの書き方に合わせてください。

- Python は 4 スペースインデントを使います。
- 関数名・変数名は `snake_case`、`nn.Module` のクラス名は `PascalCase` を使います。
- ハイパーパラメータは各スクリプト冒頭に大文字定数でまとめます。
- 実験名は `exp_NNN` 形式とし、生成物はその実験配下の `results/` に保存します。
- 重複が明確な保守負債になるまでは、共有抽象化よりも自己完結した小さなスクリプトを優先します。

## テスト方針
専用のテストスイートはまだありません。変更時の最低限の確認は次のとおりです。

- 編集したスクリプトに対して `py_compile` を実行する
- 対応する `design.md` を作成し、承認を得てから実験を実行する
- 指標、ログ、生成した図を `experiments/exp_NNN/results/` に保存する

将来テストを追加する場合は、軽量なものを実験ディレクトリの近くか新設する `tests/` に配置してください。

## コミットとプルリクエスト
このワークスペースでは Git 履歴を参照できないため、単純で一貫した規約を使ってください。

- コミットメッセージは命令形かつスコープ付きで書きます。例: `exp_003: add CIFAR-100 ABN baseline`
- PR には仮説、実装変更、実行コマンド、主要指標、次のアクションを簡潔にまとめます。
- 振る舞いの変化を実験結果で示す場合は、対応する `design.md`、図、ログを添付または参照します。

## エージェント向け注意事項
`experiments/exp_NNN/design.md` の承認なしに新しい実験を実行してはいけません。反復は CIFAR-10 または CIFAR-100 を優先し、新しく読んだ論文は `papers/` に要約を追加し、結果報告では必ず次の問いや改善案も提示してください。
```

### Codex AgentとClaude Code Agentの視点補完
Codex AgentとClaude Codeはそれぞれ，AGENTS.mdとCLAUDE.mdを参照しています．
それらを自動で参照できるように違いの`.md`ファイルに以下の文章を追加してみます．
```
## 参照ファイル
- AGENTS.md：コーディング規約・Dockerコマンド・Git規約を参照すること
```
```
## 参照ファイル
- CLAUDE.md：研究の行動原則・フェーズ・承認ゲートを参照すること
```


## ステップ5：
