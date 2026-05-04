# ステップ5：Claude Code CLI × Codex CLI の前準備
Claude Code CLI × Codex CLI による研究環境を構築するため，サブエージェントなどについてまとめます．
Claude Code CLI × Codex CLI で研究を進めるため，一度ABNのプロジェクトをバックアップだけとって削除しました．

## ステップ5.1：Claude Code サブエージェントと Agent Teams について

**[参考１：Claude Code Docs of Sub Agent](https://code.claude.com/docs/ja/sub-agents)** \
**[参考２：Claude Code Docs of Agent Teams](https://code.claude.com/docs/ja/agent-teams)** \
**[参考３：Claude Code のサブエージェント機能の基本的な使い方](https://zenn.dev/ino_h/articles/2025-09-10-claude-code-subagents-basics)** \
**[参考４：Claude Codeの「サブエージェント」と「Agent Teams」は何が違うのか──設計レイヤで理解する使い分け](https://qiita.com/nogataka/items/df6c43496b2da9d41311)**

![](https://github.com/KoyoImai/AI_Research_Agent/blob/main/Step5/images/Subagents%20vs%20Agent%20Teams.avif)

### サブエージェントとは
サブエージェントとは，Claude Code内で動作する専門特化型のAIアシスタントです．
親エージェント（メインセッション）がサブエージェントを起動し，タスク完了後に結果だけを受け取ります．
以下のような特徴があります．
- 独立したコンテキスト：専用のコンテキストウィンドウで動作
- カスタマイズされたシステムプロンプト：特定のタスクに最適化された指示
- 制限されたツールアクセス：必要なツールのみに権限を制限可能
- 再利用可能：プロジェクト間での共有と再利用が可能


### Agent Teams とは
Agent Teamsとは，複数の独立したClaude Codeインスタンスをチームとして協調動作させる仕組みです．
1つのCluade Codeがチームリーダーとなり，複数のチームメイトにタスクを割り当て，共有タスクリスト，進捗管理などを通じて協調させます．
各チームメイトは，独立したClaude Codeセッションとして働き，それぞれが独自のコンテキストウィンドウを持ちます．
さらに，チームメイト同士がチームリーダーを介さずにメッセージを送り合うことができます．


### Claude Code のサブエージェントと Agent Teamsの違い
| 観点         | Subagents               | Agent Teams           |
| ---------- | ----------------------- | --------------------- |
| 実体         | 1つのClaude Code内の専門Agent | 複数のClaude Codeセッション   |
| context    | 独立contextだが結果はmainへ返す   | 各teammateが完全に独立       |
| 通信         | 基本的にmain agentへ報告       | teammate同士が直接通信       |
| 調整         | main agentが管理           | shared task listで自己調整 |
| 用途         | 調査・レビュー・局所作業            | 議論・並列探索・共同検証          |
| token cost | 比較的低い                   | 高い                    |

サブエージェントとAgent Teamsについて，
```
結果を報告する必要がある迅速で焦点を絞ったワーカーが必要な場合は subagents を使用してください。
チームメンバーが調査結果を共有し、互いに検証し、独立して調整する必要がある場合は、エージェントチームを使用してください。
```
という内容がClaude Code Docsにあります．
まだしっかりと理解できていないので，ここから実際に動かしてみます．

最終的には，Agent Teamsの複数のチームメイトが，それぞれ内部でサブエージェントを起動するみたいな構成になるのかなと考えています．



## ステップ5.2：Claude Code サブエージェントの導入
ここからは実際にClaude Codeでサブエージェントを導入して動作確認を行います．
サブエージェントは，`.claude/agents/`に保存されます．
以下のコマンドでディレクトリを作成します．
```
mkdir -p ./.claude/agents
```

### implementerエージェント
続いて，`/agents`コマンドを使用してサブエージェントを作成していきます．
Claude Code CLIのチャット欄に以下を入力して実行してください．
```
/agents
```
上記コマンドを実行すると，以下のような画面になります．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Agents  Running   Library                                                                                                     
                                                                                                                              
    Create new agent                                                                                                          
                                                                                                                              
  No agents found. Create specialized subagents that Claude can delegate to.                                                  
                                                                                                                                  
  Each subagent has its own context window, custom system prompt, and specific tools.                                             
                                                                                                                                  
  Try creating: Code Reviewer, Code Simplifier, Security Reviewer, Tech Lead, or UX Reviewer.                                     
                                                                                                                                  
  ──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────  
  ────                                                                                                                            
                                                                                                                                  
    Built-in (always available):                                                                                                  
    claude-code-guide · haiku                                                                                                     
    Explore · haiku                                                                                                               
    general-purpose · inherit                                                                                                     
    Plan · inherit                                                                                                                
    statusline-setup · sonnet                                                                                                     
                                                                                                                                  
                                                                                                                                  
  ←/→ switch tabs · ↑↓ navigate · Enter select · Esc close 
```
`Agents`，`Running`，`Library`という3つのタブが上段にあると思います．
`←/→`キーでタブを移動できるので，`Library`へ移動し，`Create new agent`を選択して`Enter`を押してください．
そうすると以下のような画面に移ります．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Choose location                                                                                                             
                                                                                                                              
  ❯ 1. Project (.claude/agents/)                                                                                              
    2. Personal (~/.claude/agents/)                                                                                           
                                                                                                                                  
   ↑↓ to navigate · Enter to select · Esc to cancel         
```
上記の画面では，サブエージェントの保存先を聞かれているので，今回は`1. Project (.claude/agents/)`を選択し`Enter`を押します．
`1. Project (.claude/agents/)`を指定した場合，作成するサブエージェントは起動中のプロジェクト内でのみ有効になります．
続いて以下のような画面に移ります．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Creation method                                                                                                             
                                                                                                                              
  ❯ 1. Generate with Claude (recommended)                                                                                     
    2. Manual configuration                                                                                                   
                                                                                                                                  
   ↑↓ to navigate · Enter to select · Esc to go back    
```
今回は`1. Generate with Claude (recommended)`を選択して`Enter`を押します．
そうすると，プロンプトの入力が求められると思います．
ここで入力したプロンプトをもとにサブエージェントを定義するYAMLファイルが自動で生成されます．
今回は以下の通りにプロンプトを入力しました．（`option + Enter`でチャット欄の改行ができます．）
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Describe what this agent should do and when it should be used (be comprehensive for best results)                           
                                                                                                                              
  Please describe what the agent should do                                                                                    
                                                                                                                              
  機械学習の実験コードを実装する専門エージェント．                                                                                
  以下の役割を担う：                                                                                                              
  - design.mdを最初に読み込んで，実装方針を把握する           
  - experiments/exp_NNN/run.py を実装する                                                                                         
  - 実装後にdocker exec research-dev python -m py_compile で構文チェックを行う                                                    
  - エラーが発生した場合は自律的に修正する                                                                                        
  - 完了後、実装内容の概要をメインエージェントに報告する                                                                          
                                                                                                                                  
  コーディング規約：                                                                                                              
  - Pythonは4スペースインデント                                                                                                   
  - ハイパーパラメータはファイル冒頭に大文字定数でまとめる                                                                        
  - 関数名・変数名はsnake_case、クラス名はPascalCase                                                                              
                                                                                                                                  
  使用ツール：Read, Write, Edit, Bash, Glob, Grep                                                                                 
                                                                                                                                  
   Enter to submit · ctrl+g to open in editor · Esc to go back
```
プロンプトを入力して`Enter`を押すと以下のような画面が現れます．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Select tools                                                                                                                
                                                                                                                              
                                                                                                                              
    [ Continue ]                                                                                                              
  ────────────────────────────────────────                                                                                        
    ☐ All tools                                                                                                                   
    ☒ Read-only tools                                                                                                             
    ☒ Edit tools                                                                                                                  
  ❯ ☒ Execution tools                                                                                                             
    ☐ MCP tools                                                                                                                   
    ☐ Other tools                                                                                                                 
  ────────────────────────────────────────                                                                                        
    [ Show advanced options ]                                                                                                     
                                                                                                                                  
  8 of 43 tools selected                                                                                                          
                                                                                                                                  
   Enter to toggle selection · ↑↓ to navigate · Esc to go back   
```
ここでは，サブエージェントに与える権限を指定します．
`上下キー`で移動，`Enter`でチェックの付け外しができます．
上記画面の通り権限を付与して，`Continue`で`Enter`を押します．
続いて，以下のような画面が現れます．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Select model                                                                                                                
                                                                                                                              
  Model determines the agent's reasoning capabilities and speed.                                                              
                                                                                                                              
  ❯ 1. Sonnet ✔             Balanced performance - best for most agents                                                           
    2. Opus                 Most capable for complex reasoning tasks                                                              
    3. Haiku                Fast and efficient for simple tasks                                                                   
    4. Inherit from parent  Use the same model as the main conversation                                                           
                                                                                                                                  
   ↑↓ to navigate · Enter to select · Esc to go back
```
ここでは，このサブエージェントで使用するモデルを選択します．
今回は`Sonnet`を選択して`Enter`を押します．
続いて以下のような画面が現れます．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Choose background color                                                                                                     
                                                                                                                              
  ❯ Automatic color                                                                                                           
      Red                                                                                                                     
      Blue                                                                                                                        
      Green                                                                                                                       
      Yellow                                                                                                                      
      Purple                                                                                                                      
      Orange                                                                                                                      
      Pink                                                                                                                        
      Cyan                                                                                                                        
                                                                                                                                  
                                                                                                                                  
  Preview:  @ml-experiment-implementer                                                                                            
                                                                                                                                  
   ↑↓ to navigate · Enter to select · Esc to go back  
```
ここでは，サブエージェントの背景色を選択します．
機能そのものに影響しないので好きな色を選択してください．
今回は`Automatic color`を選択します．
続いて，以下のような画面が現れます．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /agents                                                                                                                         
                             
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Create new agent                                                                                                              
  Configure agent memory                                                                                                      
                                                                                                                              
  ❯ 1. Project scope (.claude/agent-memory/) (Recommended)                                                                    
    2. None (no persistent memory)                                                                                            
    3. User scope (~/.claude/agent-memory/)                                                                                       
    4. Local scope (.claude/agent-memory-local/)                                                                                  
                                                                                                                                  
   ↑↓ to navigate · Enter to select · Esc to go back 
```
ここでは，サブエージェントのメモリの保存先を選択します．
`1. Project scope (.claude/agent-memory/) (Recommended)`を選択します．
これで，今回のプロジェクト内で実験の記録と管理がしやすくなります．
次の画面に移ったら`s`を押してサブエージェントのファイルを保存してください．
`project1/.claude/agents`にサブエージェントの設定が記述された`.md`ファイルが保存されていると思います．

今回は，`project1/.claude/agents/implementer.md`が作成されました．
ファイル名が長い場合や内容がおかしい場合は，手動で修正して大丈夫です，
今回生成された`implementer.md`はこの[githubディレクトリ](https://github.com/KoyoImai/AI_Research_Agent/blob/main/Step5/implementer.md?plain=1)に置いておきます．


### analyzerエージェント
続いて，analyzerエージェントの作成に進みます．
implementerエージェントと同様にまず以下を実行してください．
```
/agents
```
実行したら，以下の画面が出るので，`Library`タブの`Create new agent`を選択して`Enter`を押してください．
```
 ▐▛███▜▌   Claude Code v2.1.126
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /clear                                                                                                                                                                        
  ⎿  (no content)  
                                                                                                                          
❯ /agents                                                                                                                                                                       
                                                                                                                                                                                
────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Agents  Running   Library                                                                                                                                                     

  ❯ Create new agent                                                                                                                                                            
                                                                                                                                                                                
    Project agents                                                                                                                                                              
    ml-experiment-implementer · sonnet · project memory                                                                                                                         
                                                                                                                                                                                
    Built-in agents (always available)                                                                                                                                          
    claude-code-guide · haiku                                                                                                                                                   
    Explore · haiku                                                                                                                                                             
    general-purpose · inherit
    Plan · inherit                                                                                                                                                              
    statusline-setup · sonnet                                                                                                                                                   
                                                                                                                                                                                
                                                                                                                                                                                
  ←/→ switch tabs · ↑↓ navigate · Enter select · Esc close  
```
以降もimplementerエージェントと同様に，`1. Project (.claude/agents/)`，`1. Generate with Claude (recommended)`を順番に選択して`Enter`を押してください．
その後，どのようなエージェントを作成するかプロンプトを入力するので，以下の内容でエージェントを作成します．
```
機械学習の実験結果を分析する専門エージェント．
以下の役割を担う：
- experiments/exp_NNN/results/ 以下の実験結果ファイルを読み込む
- 精度・損失・学習曲線などの結果を定量的に分析する
- ベースラインとの比較を行う
- 改善案を3〜5個提案する
- 分析レポートをメインエージェントに返す

使用ツール：Read, Glob, Grep
```
続いてエージェントに与える権限を聞かれます．
`Read-only tools`と`Edit tools`に権限を与えます．
モデルは`sonner`とし，色は`Automatic color`とします．
最後に`1. Project scope (.claude/agent-memory/) (Recommended)`を選択後，`s`を押して一旦完了です．


## ステップ5.3：Claude Code サブエージェントの動作確認



## ステップ5.4：Claude Code Agent Teams の導入



