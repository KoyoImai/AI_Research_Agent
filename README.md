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
最終的には，以下のように確認


## ステップ5：
