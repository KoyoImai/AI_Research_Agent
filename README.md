# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude Codeで構築することを目的とします．

## 構成と環境

## ステップ1：Claude Codeの環境構築
ステップ1では，Claude Codeの環境構築とセットアップを行います．

### Node.jsのインストール
以下のコマンドを実行して`Node.js`をインストールします．
```
# nvmをダウンロードしてインストール
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.3/install.sh | bash

# .bachrcの再読み込み
source ~/.bashrc

# Node.jsをインストール
nvm install --lts

# バージョン確認（特定のバージョンを入れたい場合は，上記のコマンドを修正してバージョンを指定してください）
node --version
npm --version
```

### Claude Code CLIのインストール

```
npm install -g @anthropic-ai/claude-code
```

### tmuxセッションの作成
```
tmux new -s research-agent
```

### Claude Codeの起動
ここから，Claude Codeを起動して認証を行います．
```
claude
```
上記のコマンドを実行すると，以下のような画面が現れます．
```
mprg@spark-09ef:~$ claude
Welcome to Claude Code v2.1.117 
…………………………………………………………………………………………………………………………………………………………

                                                     
                                                       
            ░░░░░░                             
    ░░░   ░░░░░░░░░░                           
   ░░░░░░░░░░░░░░░░░░░                                 
                                                      
                           ░░░░                     ██    
                         ░░░░░░░░░░               ██▒▒██  
                                            ▒▒      ██   ▒
       █████████                          ▒▒░░▒▒      ▒ ▒▒
      ██▄█████▄██                           ▒▒         ▒▒
       █████████                           ░          ▒
…………………█ █   █ █……………………………………………………………………░…………………………▒…………

 Let's get started.

 Choose the text style that looks best with your terminal
 To change this later, run /theme

 ❯ 1. Auto (match terminal)
   2. Dark mode ✔
   3. Light mode
   4. Dark mode (colorblind-friendly)
   5. Light mode (colorblind-friendly)
   6. Dark mode (ANSI colors only)
   7. Light mode (ANSI colors only)

 ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
  1  function greet() {
  2 -  console.log("Hello, World!");                                                                                                                      
  2 +  console.log("Hello, Claude!");                                                                                                                     
  3  }
 ╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌╌
  Syntax theme: GitHub (ctrl+t to disable)          
```
ここではテーマを聞かれているだけなので，好きなテーマを選択します．
テーマ選択後，以下の画面が表示されます．
```
mprg@spark-09ef:~$ claude
Welcome to Claude Code v2.1.117 
…………………………………………………………………………………………………………………………………………………………

     *                                       █████▓▓░
                                 *         ███▓░     ░░
            ░░░░░░                        ███▓░
    ░░░   ░░░░░░░░░░                      ███▓░
   ░░░░░░░░░░░░░░░░░░░    *                ██▓░░      ▓
                                             ░▓▓███▓▓░
 *                                 ░░░░                   
                                 ░░░░░░░░                 
                               ░░░░░░░░░░░░░░░░           
       █████████                                        * 
      ██▄█████▄██                        *               
       █████████      *                                
…………………█ █   █ █………………………………………………………………………………………………………………

                   
 Claude Code can be used with your Claude subscription or billed based on API usage through your Console account.
                                                         
 Select login method:            

 ❯ 1. Claude account with subscription · Pro, Max, Team, or Enterprise
                 
   2. Anthropic Console account · API usage billing
                                     
   3. 3rd-party platform · Amazon Bedrock, Microsoft Foundry, or Vertex AI
```

