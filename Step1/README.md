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
上記画面では，アカウントの種類の選択を求められています．
今回の環境構築では，Claude CodeのProプラン以上に加入している人を対象としているので，`1. Claude account with subscription · Pro, Max, Team, or Enterprise`を選択してください．
`2. Anthropic Console account · API usage billing`を選択すると，API課金を追加されるので気をつけてください．
`1`を選択後，以下のような画面が表示されます．
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

 Browser didn't open? Use the url below to sign in (c to copy)                                                                                             
                                                                                                                                                           
https://claude.com/cai/oauth/authorize?code=true&client_id=9d1c250a-e61b-44d9-88ed-5944d1962f5e&response_type=code&redirect_uri=https%3A%2F%2Fplatform.clau
de.com%2Foauth%2Fcode%2Fcallback&scope=org%3Acreate_api_key+user%3Aprofile+user%3Ainference+user%3Asessions%3Aclaude_code+user%3Amcp_servers+user%3Afile_up
load&code_challenge=kgS-azVxoI7pBuku7TMN2tPz3wAk_Phfei0jC4PvP-k&code_challenge_method=S256&state=-3YE0ovKJfa7613w3KkpKmA852lp6LqYOcYXnKy_U-0               
                                                                                                                                                           
                                                                                                                                                           
 Paste code here if prompted >
```
このとき，ブラウザが直接開かれるか，ブラウザが開かれない場合は表示されたurlをコピーしてClaude.aiにログインしてください．
ログインすると認証コードが表示されるので，それをコピーして`Paste code here if prompted >`の箇所に貼り付けて`Enter`を押してください．
そうすると，ログインが完了し以下のような画面が表示されます．
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

 Logged in as imaikoyo0314@gmail.com                                                                                                                       
 Login successful. Press Enter to continue…  
```
その後，以下のような画面が表示されます．
これは，Claude Codeを使用するうえでの注意事項です．
読み終わったら`Enter`を押してください．
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

 Security notes:                                                                                                                                           
                                                                                                                                                           
 1. Claude can make mistakes                                                                                                                               
    You should always review Claude's responses, especially when                                                                                           
    running code.                                                                                                                                          
                                                                                                                                                           
 2. Due to prompt injection risks, only use it with code you trust                                                                                         
    Learn more: https://code.claude.com/docs/en/security                                                                                                   

 Press Enter to continue…       
```
続いて，以下のような画面が表示されます．
`Enter`を押して，次に進んでください．
```
mprg@spark-09ef:~$ claude
                                
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Accessing workspace:
                                                     
 /home/mprg                                            
                                               
 Quick safety check: Is this a project you created or one you trust? (Like your own code, a well-known open source project, or work from your team). If
 not, take a moment to review what's in this folder first.
                                                      
 Claude Code'll be able to read, edit, and execute files here.
                                                          
 Security guide                                           
                                                          
 ❯ 1. Yes, I trust this folder                           
   2. No, exit                                         
                                                          
 Enter to confirm · Esc to cancel
```
最後にこのような画面が表示されます．
```
mprg@spark-09ef:~$ claude
╭─── Claude Code v2.1.117 ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                                    │ Tips for getting started                                                                           │
│                 Welcome back Imai!                 │ Run /init to create a CLAUDE.md file with instructions for Claude                                  │
│                                                    │ Note: You have launched claude in your home directory. For the best experience, launch it in a pr… │
│                       ▐▛███▜▌                      │ ────────────────────────────────────────────────────────────────────────────────────────────────── │
│                      ▝▜█████▛▘                     │ Recent activity                                                                                    │
│                        ▘▘ ▝▝                       │ No recent activity                                                                                 │
│ Sonnet 4.6 · Claude Pro · imaikoyo0314@gmail.com's │                                                                                                    │
│  Organization                                      │                                                                                                    │
│                     /home/mprg                     │                                                                                                    │
╰─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
                                                          
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯                                                        
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  ? for shortcuts   
```
これでClaude Codeの環境構築と起動の確認は完了です．
今回はホームディレクトリ（`/home/mprg`）でClaude Codeを起動しているため，次ステップ以降ではプロジェクトディレクトリでClaude Codeを起動し直してから進めていきます．

`/exit`と入力すれば起動中のClaude Codeを終了できます．

