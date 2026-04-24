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

### 

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## [ステップ3：シンプルなAgentの構築と動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step3)

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


## ステップ5：
