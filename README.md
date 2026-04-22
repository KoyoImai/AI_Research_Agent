# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude Codeで構築することを目的とします．

## 構成と環境

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## ステップ2:Claude Codeの動作確認
ステップ2では，MCPサーバーの導入を含めたClaude Codeの動作確認を行います．

### プロジェクトディレクトリの作成とclaudeの起動
ステップ1ではホームディレクトリでClaude Codeを起動していたため，新しくプロジェクトディレクトリの作成から行なっていきます．
今回は，`~/research/project1`をプロジェクトディレクトリとします．
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
ここでは，
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


