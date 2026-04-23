# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude Codeで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## ステップ2：Claude Codeの動作確認
ステップ2では，MCPサーバーの導入を含めたClaude Codeの動作確認を行います．
あくまで動作確認ですので，まだ本格的なAI Agentの構築・稼働には進みません．
今井の勉強を兼ねた動作確認ですので，基本的に読み飛ばしてください．

### プロジェクトディレクトリの作成とclaudeの起動
ステップ1ではホームディレクトリでClaude Codeを起動していたため，新しくプロジェクトディレクトリの作成から行なっていきます．
今回は，`~/research/project1`をプロジェクトディレクトリとし，Claude Codeによる作業などは，このディレクトリ上で行うようにします．
```
mkdir -p ~/research/project1
cd ~/research/project1/
```
プロジェクトディレクトリに移動後，Claude Codeを起動します．
Claude Codeを起動すると，このディレクトリを信頼するかどうかの確認画面が現れるので，信頼するを選択してください．
```
claude
```

### CLAUDE.mdの作成
続いて，CLAUDE.mdを作成していきます．
CLLAUDE.mdとは，Claude Codeが最初に読む「指示書」のようなものです．
プロジェクトの構造や使用技術，コーディング規約などをCLAUDE.mdに記述しておくことで，毎回自動的に読み込まれます．
以下のコマンドを実行して，CLAUDE.mdの初期ファイルを生成します．
```
/init
```
上記のコマンドを実行すると，以下の確認画面が現れます．
ここでは，`2. Yes, allow reading from research/ from this project`を選択して，`research/`ディレクトリへのアクセスを今後も許可します．
```
mprg@spark-09ef:~/research/project1$ claude
╭─── Claude Code v2.1.117 ──────────────────────────────────────────────────────────────────────────────────╮
│                                                    │ Tips for getting started                             │
│                 Welcome back Imai!                 │ Ask Claude to create a new app or clone a repository │
│                                                    │ ──────────────────────────────────────────────────── │
│                       ▐▛███▜▌                      │ Recent activity                                      │
│                      ▝▜█████▛▘                     │ No recent activity                                   │
│                        ▘▘ ▝▝                       │                                                      │
│ Sonnet 4.6 · Claude Pro · imaikoyo0314@gmail.com's │                                                      │
│  Organization                                      │                                                      │
│                ~/research/project1                 │                                                      │
╰───────────────────────────────────────────────────────────────────────────────────────────────────────────╯

❯ /init                                                                                                                                                    
        
● I'll explore the codebase to understand its structure before creating CLAUDE.md.                                                                         
                                                                                                                                                           
  Listed 1 directory (ctrl+o to expand)                                                                                                                    
                                                                                                                                                           
● The directory appears to be empty. Let me check more broadly.                                                                                            
                                                                                                                                                           
  Listing 1 directory… (ctrl+o to expand)                                                                                                                  
  ⎿  $ ls -la /home/mprg/research/project1/ && ls -la /home/mprg/research/                                                                                 
                                                                                                                                                           
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Bash command                                                                                                                                              
                  
   ls -la /home/mprg/research/project1/ && ls -la /home/mprg/research/
   List all files including hidden ones

 Do you want to proceed?
 ❯ 1. Yes
   2. Yes, allow reading from research/ from this project
   3. No

 Esc to cancel · Tab to amend · ctrl+e to explain
```
続いて，以下のような画面が現れます．
ここでも，`2. Yes, allow all edits during this session (shift+tab)`を選択します．
以降も，Claude Codeがコマンドの実行許可を求めてくる場合が多くあります．
その場合は，大体`2`を選択していきます．
```
❯ /init                                                                                                                                                    

● I'll explore the codebase to understand its structure before creating CLAUDE.md.

  Listed 1 directory (ctrl+o to expand)

● The directory appears to be empty. Let me check more broadly.

  Listed 1 directory (ctrl+o to expand)

● The project1 directory is completely empty — there's no code to analyze yet. I'll create a minimal CLAUDE.md that serves as a starting point for when
  code is added.

● Write(CLAUDE.md)

───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Create file
 CLAUDE.md
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
  1 # CLAUDE.md
  2
  3 This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.
  4
  5 This repository is currently empty. Update this file once the project is initialized with:
  6
  7 - Build, lint, and test commands
  8 - High-level architecture overview
  9 - Any project-specific conventions or constraints
╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
 Do you want to create CLAUDE.md?
   1. Yes
 ❯ 2. Yes, allow all edits during this session (shift+tab)
   3. No

 Esc to cancel
```
これで，CLAUDE.mdの初期ファイルが作成されました．
まだ，CLAUDE.mdの設定を行っていないので内容は初期状態です．
Claude Codeを`/exit`で終了して，CLAUDE.mdの内容を確認してみます．
```
mprg@spark-09ef:~/research/project1$ ls
CLAUDE.md
mprg@spark-09ef:~/research/project1$ cat CLAUDE.md 
# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

This repository is currently empty. Update this file once the project is initialized with:

- Build, lint, and test commands
- High-level architecture overview
- Any project-specific conventions or constraints
mprg@spark-09ef:~/research/project1$ 
```


### Dockerコンテナの作成
ここでは，Dockerコンテナの作成を行います．
自分でDockerfileを作成し，Dockerコンテナを立ててもいいですが，ここではClaude Codeの動作確認も兼ねてClaude CodeにDockerコンテナの作成をさせます．
まず，以下のコマンドでClaude Codeを起動してください．
通常は，`Enter`を押すと入力が完了してしまいます．
改行するには`\（バックスラッシュ）`を入力してから`Enter`を押してください．
```
claude
```
claudeを起動したら，以下の内容を書き込んでください．
```
mprg@spark-09ef:~/research/project1$ claude
 ▐▛███▜▌   Claude Code v2.1.118                                                                                                                            
▝▜█████▛▘  Sonnet 4.6 · Claude Pro                                                                                                                         
  ▘▘ ▝▝    ~/research/project1                                                                                                                             
                                                                                                                                                           
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ 以下の要件でDockerfileを作成し，イメージのビルドとコンテナの起動まで行なってください．                                                 
                                                                                                                                                    
  [Dockerfileの要件]                                                                                                                               
  - ベースイメージ：nvcr.io/nvidia/pytorch:25.03-py3                                                                                                       
  - 作業ディレクトリ：/workspace                                                                                                                 
  - 追加ライブラリ：transformers, datasets, accelerate, wandb, jupyterlab, matplotlib, seaborn, hydra-core                                               
  - CMD：sleep infinity                                                                                                                                    
                                                                                                                                                           
  [コンテナの起動要件]                                                                                                                             
  - コンテナ名：research-dev                                                                                                                       
  - GPUを全て使用                                                                                                                                    
  - ~/research をコンテナの /workspace にマウント                                                                                                          
  - 常時起動                                                                                                                                       
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```

### ディレクトリ構成の整備
今後AI Agentを導入して研究を進めていく上でのディレクトリ構成の整備を行います．
以下のようなディレクトリ構成を，Claude Codeによって整備することにします．
```
~research/project1
                |- papers
                |- experiments
                |- outputs/figures
                |- outputs/notes.md
                |- .claude/commands 
```
Claude Codeに以下の内容のプロンプトを入力して実行してみてください（↓は入力と結果の画面です）．
```
❯ 以下のディレクトリ構成を~/research/project1/内に作成してください．                                                                                       
                                                                                                                                                           
  - papers/        # 論文PDF置き場                                                                                                                         
  - experiments/   # 実験管理                                                                                                                              
  - outputs/figures/ # 可視化結果                                                                                                                          
  - outputs/notes.md # 実験メモ                                                                                                                            
  - .claude/commands/ # カスタムコマンド置き場                                                                                                             

● Bash(mkdir -p /home/mprg/research/project1/papers \
                 /home/mprg/research/project1/experiments \…)
  ⎿  Done    

  Searched for 1 pattern (ctrl+o to expand)  
                                                                                           
● 意図した通りの構成で作成できています。
                                                                                                                                                           
  ~/research/project1/
  ├── papers/              # 論文PDF置き場                                                                                                                 
  ├── experiments/         # 実験管理                                                                                                                    
  ├── outputs/                                                                                                                                             
  │   ├── figures/         # 可視化結果                                                                                                                  
  │   └── notes.md         # 実験メモ
  └── .claude/                                                                                                                                             
      └── commands/        # カスタムコマンド置き場
                                                                                                                                                           
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯                                                                                          
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
     
```


### MCPサーバーの導入
**[参考1：Claude Code Docs](https://code.claude.com/docs/ja/mcp)** \
**[参考2：ひとつやるだけでも大違い - Claude Code 設定チートシート、書いたよー](https://note.com/okdt/n/n655c4ee10e89)** \
**[参考3：Claude Codeにコマンド一発でMCPサーバを簡単設定](https://zenn.dev/karaage0703/articles/3bd2957807f311)** \
**[参考4：【MCPのトリセツ #2】Filesystem MCP Server： AIでローカルファイルを扱う](https://zenn.dev/takna/articles/mcp-server-tutorial-02-filesystem)** \
**[参考5：Claude for Desktop での「Filesystem MCP Server」を使ったファイル操作関連の簡単なお試し](https://qiita.com/youtoy/items/87ce4a734cdc0b2de4fd)**


MCP（Model Context Protocol）とは，Claude Codeが外部ツールを使用するための仕組みです．
デフォルトのClaude Codeは，ターミナルコマンドによるファイルの読み込みや書き込みはできますが，arXivによる論文の検索，git操作などはできません．
そこで，MCPを用いることでClaude Codeの機能を拡張します．

今回は，MCPサーバーを実際に導入することを目的としているので，AI Agentに必要な全てのMCPは導入はせず，MCPの導入手順を確かめていきます．
まず，以下のコマンドで現在のMCPの設定状況を確認します．
```
/mcp
```
実行すると，以下のような画面が表示されると思います．
デフォルトでは，Gmail・Google Calendar・Google DriveのMCPが表示されています．
```
❯ /mcp                                                                                                                                                     
                                                                                                                                                           
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Manage MCP servers                                                                                                                                       
  3 servers                                                                                                                                                
                                                                                                                                                           
    claude.ai                                                                                                                                              
  ❯ claude.ai Gmail · △ needs authentication                                                                                                               
    claude.ai Google Calendar · △ needs authentication                                                                                                     
    claude.ai Google Drive · △ needs authentication

  https://code.claude.com/docs/en/mcp for help
 ↑↓ to navigate · Enter to confirm · Esc to cancel
```
`Esc`を押してチャット欄に戻り，`/exit`でClaude Codeを終了してください．
その後，`~/.claude.json`を編集します．
`~/.claude.json`は，Claude Codeを起動するたびに読み込まれる設定ファイルです．
今回はここに，filesystem MCPの設定を書き込むことで，Claude Codeを起動するたびに自動的にfilesystem MCPが使えるようにします．
（最初MCPの設定方法がよく分からず，`~/.claude.json`の内容がめちゃくちゃになったので，消してClaude Codeの設定を一からやり直しています．）
以下の内容を，`~/.claude.json`に追記してください．
```
{
  "mcpServers": {
    "filesystem": {
      "command": "npx",
      "args": [
        "-y",
        "@modelcontextprotocol/server-filesystem",
        "/home/mprg/research"
      ]
    }
  }
}
```
上記の内容を`~/.claude.json`に追記したら，チャットでfilesystem MCPを使うように指示を出してみます．
```
❯ filesystem MCPを使用して，/home/mprg/research/project1/の中にあるファイル一覧を表示してください．                                                        
                                                                                                                                                 
  Called filesystem (ctrl+o to expand)                                                                                                                     
                                                                                                                                                           
● /home/mprg/research/project1/ の中身：                                                                                                                   
                                                                                                                                                           
  [DIR]  .claude                                                                                                                                           
  [FILE] CLAUDE.md                                                                                                                                         
  [FILE] Dockerfile                                                                                                                                        
  [DIR]  experiments                                                                                                                                       
  [DIR]  outputs                                                                                                                                           
  [DIR]  papers                                                                                                                                            
                                                                                                                                                           
  ディレクトリが3つ（.claude, experiments, outputs, papers）、ファイルが2つ（CLAUDE.md, Dockerfile）あります。                                             
                                                                                                                                                           
※ recap: Checked the research-dev Docker container (PyTorch 2.7 with CUDA on NVIDIA GB10) and listed project1 files via filesystem MCP. No immediate next  
  action needed. (disable recaps in /config)                                                                                                               
                                                                                                                                                           
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯                                                                                                                                                          
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ? for shortcuts   
```
正直まだ，filesystem MCPが必要なのかよくわかっていません．
ただ，今回はMCPサーバーの導入を試してみるのが目的なので，一旦filesystem MCPを導入しました．
ステップ3以降では，Context7などのよく使われているMCPを導入していく予定です．
