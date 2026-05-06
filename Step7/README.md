# ステップ7：MCPサーバーの導入

**[参考1：Claude Code Docs](https://code.claude.com/docs/ja/mcp)** \
**[参考2：Claude Codeにコマンド一発でMCPサーバを簡単設定](https://zenn.dev/karaage0703/articles/3bd2957807f311)**

MCPサーバーとは，Claude Codeと他のアプリケーションとのやり取りを仲介してくれる通信サーバーのことです．
MCPサーバーを立てて，それを介してデータのやり取りを行うことで，特定の情報を取得したり，アプリケーションの操作を行うことができます．
Anthropicや開発者コミュニティが公開しているMCPサーバーをそのまま導入することで，arXivやgithubなどと連携ができます．
もしくは，オリジナルのMCPサーバーを自作することもできます．
以下では，MCPサーバーの導入とその使用方法についてまとめていきます．


## MCPサーバー1：Filesystem MCP
まず，Filesystem MCPサーバーの導入から始めます．
Filesystem MCPは，ローカルに存在するファイルを操作するためのサーバーです（?）．
ターミナル上で以下のコマンドを実行し，Filesystem MCPサーバーのパッケージをインストールします（Claude Code上ではないです）．
```
npm install -g @modelcontextprotocol/server-filesystem
```
```
claude mcp add --scope user filesystem mcp-server-filesystem /home/mprg/research
```
導入が完了したら，Claude Codeを起動してFilesystem MCPが導入できているかを確認します．
```
 ▐▛███▜▌   Claude Code v2.1.128
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

❯ /mcp                                                                                                                            
                                                                                                                                  
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  Manage MCP servers                                                                                                              
  4 servers                                                                                                                       
                                                                                                                                  
    User MCPs (/home/mprg/.claude.json)                                                                                           
  ❯ filesystem · ✔ connected · 14 tools                                                                                           
                                                                                                                                  
    claude.ai
    claude.ai Gmail · △ needs authentication
    claude.ai Google Calendar · △ needs authentication
    claude.ai Google Drive · △ needs authentication

  https://code.claude.com/docs/en/mcp for help
 ↑↓ to navigate · Enter to confirm · Esc to cancel
```
その後，`filesystem MCPを使用して，/home/mprg/research/project1/の中にあるファイル一覧を表示してください．`とチャット欄に入力して動作を確認してみてください．

## MCPサーバー2：arXiv MCP
arXiv MCP サーバーを導入します．
以下のコマンドを実行して必要なパッケージをインストールしてください．
```
npm install -g @cyanheads/arxiv-mcp-server
```
インストールが完了したら，Claude Codeに登録を行います．
以下のコマンドを実行してください．
```
claude mcp add --scope user arxiv arxiv-mcp-server
```
これで登録が完了しました．
Claude Codeを起動して`/mcp`コマンドで登録ができているかを確かめてみてください．
その後`arXiv MCPを使用して，Attention Branch Networkに関する論文を3件検索してください．`のようなプロンプトを入力して動作を確認してみてください．

（5/5時点だと，16号館4階のネットワークからだとアクセス制限が出てarXiv MCPサーバーが使えないみたいです．．．）



## MCPサーバー3：Codex MCP
Codexはステップ4ですでにインストール済みなので，以下のコマンドを実行してMCPとして登録してください．
```
claude mcp add --scope user codex codex mcp-server
```



## MCPサーバー4：Gemini CLI MCP
まず，Gemini CLIをインストールします．
以下のコマンドを実行してGemini CLIをインストールしてください．
```
npx https://github.com/google-gemini/gemini-cli
```
そうすると以下のような画面が現れます．
```
mprg@spark-fb97:~/research/project1$ npx https://github.com/google-gemini/gemini-cli
Need to install the following packages:
github:google-gemini/gemini-cli
Ok to proceed? (y) y

 ▝▜▄      ▗█▀▀▜▙▝█▛▀▀▌▜██▖▟██▘▜█▘▜██▖▝█▛▝█▛
   ▝▜▄    █▌     █▙▟  ▐█▝█▛▐█ ▐█ ▐█▝█▖█▌ █▌
  ▗▟▀     ▜▙ ▝█▛ █▌▝ ▖▐█   ▐█ ▐█ ▐█ ▝██▌ █▌
 ▝▀        ▀▀▀▀▘▝▀▀▀▀▘▀▀▘  ▀▀▘▀▀▘▀▀▘ ▝▀▀▝▀▀

 Gemini CLI v0.42.0-nightly.20260428.g59b2dea0e



Tips for getting started:
1. Create GEMINI.md files to customize your interactions
2. /help for more information
3. Ask coding questions, edit code or run commands
4. Be specific for the best results

ℹ Skipping project agents due to untrusted folder. To enable, ensure that the project root is trusted.

 ╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 │
 │ > Do you want to connect VS Code to Gemini CLI?
 │ If you select Yes, we'll install an extension that allows the CLI to access your open files and display diffs directly in VS
 │ Code.
 │
 │ ● 1. Yes                                                                                                                      
 │   2. No (esc)
 │   3. No, don't ask again
 │
 ╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```
今回はVS Codeとの連携は不要なので`2`を選択します．
続いて以下のように信頼するプロジェクトを聞かれます．
今回は`2`を選択します．
```

 ▝▜▄      ▗█▀▀▜▙▝█▛▀▀▌▜██▖▟██▘▜█▘▜██▖▝█▛▝█▛
   ▝▜▄    █▌     █▙▟  ▐█▝█▛▐█ ▐█ ▐█▝█▖█▌ █▌
  ▗▟▀     ▜▙ ▝█▛ █▌▝ ▖▐█   ▐█ ▐█ ▐█ ▝██▌ █▌
 ▝▀        ▀▀▀▀▘▝▀▀▀▀▘▀▀▘  ▀▀▘▀▀▘▀▀▘ ▝▀▀▝▀▀

 Gemini CLI v0.42.0-nightly.20260428.g59b2dea0e



Tips for getting started:
1. Create GEMINI.md files to customize your interactions
2. /help for more information
3. Ask coding questions, edit code or run commands
4. Be specific for the best results

ℹ Skipping project agents due to untrusted folder. To enable, ensure that the project root is trusted.

 ╭──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
 │                                                                                                                              │
 │ Do you trust the files in this folder?                                                                                       │
 │                                                                                                                              │
 │ Trusting a folder allows Gemini CLI to load its local configurations, including custom commands, hooks, MCP servers, agent   │
 │ skills, and settings. These configurations could execute code on your behalf or change the behavior of the CLI.              │
 │                                                                                                                              │
 │                                                                                                                              │
 │ ● 1. Trust folder (project1)                                                                                                 │
 │   2. Trust parent folder (research)                                                                                          │
 │   3. Don't trust                                                                                                             │
 │                                                                                                                              │
 ╰──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```
続いて，以下のような画面が現れます．
`3`を選択して次に進んでください．
```

 ▝▜▄      ▗█▀▀▜▙▝█▛▀▀▌▜██▖▟██▘▜█▘▜██▖▝█▛▝█▛
   ▝▜▄    █▌     █▙▟  ▐█▝█▛▐█ ▐█ ▐█▝█▖█▌ █▌
  ▗▟▀     ▜▙ ▝█▛ █▌▝ ▖▐█   ▐█ ▐█ ▐█ ▝██▌ █▌
 ▝▀        ▀▀▀▀▘▝▀▀▀▀▘▀▀▘  ▀▀▘▀▀▘▀▀▘ ▝▀▀▝▀▀

 Gemini CLI v0.42.0-nightly.20260428.g59b2dea0e



Tips for getting started:
1. Create GEMINI.md files to customize your interactions
2. /help for more information
3. Ask coding questions, edit code or run commands
4. Be specific for the best results

 ╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 │
 │ > Do you want to connect VS Code to Gemini CLI?
 │ If you select Yes, we'll install an extension that allows the CLI to access your open files and display diffs directly in VS
 │ Code.
 │
 │ ● 1. Yes                                                                                                                      
 │   2. No (esc)
 │   3. No, don't ask again
 │
 ╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```
そうすると以下のような画面が現れます．
Gemini CLIの認証方法を選択します．
今回は最も手軽だと思われる`1`を選択します．
`1`を選択後，ターミナル上にurlが出現するので，コピペするなりで開いて指示通りに進んで行ってください．
```

 ▝▜▄      ▗█▀▀▜▙▝█▛▀▀▌▜██▖▟██▘▜█▘▜██▖▝█▛▝█▛
   ▝▜▄    █▌     █▙▟  ▐█▝█▛▐█ ▐█ ▐█▝█▖█▌ █▌
  ▗▟▀     ▜▙ ▝█▛ █▌▝ ▖▐█   ▐█ ▐█ ▐█ ▝██▌ █▌
 ▝▀        ▀▀▀▀▘▝▀▀▀▀▘▀▀▘  ▀▀▘▀▀▘▀▀▘ ▝▀▀▝▀▀

 Gemini CLI v0.42.0-nightly.20260428.g59b2dea0e



Tips for getting started:
1. Create GEMINI.md files to customize your interactions
2. /help for more information
3. Ask coding questions, edit code or run commands
4. Be specific for the best results

╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│                                                                                                                                │
│ ? Get started                                                                                                                  │
│                                                                                                                                │
│   How would you like to authenticate for this project?                                                                         │
│                                                                                                                                │
│   ● 1. Sign in with Google                                                                                                     │
│     2. Use Gemini API Key                                                                                                      │
│     3. Vertex AI                                                                                                               │
│                                                                                                                                │
│   No authentication method selected.                                                                                           │
│                                                                                                                                │
│   (Use Enter to select)                                                                                                        │
│                                                                                                                                │
│   Terms of Services and Privacy Notice for Gemini CLI                                                                          │
│                                                                                                                                │
│   https://geminicli.com/docs/resources/tos-privacy/                                                                            │
│                                                                                                                                │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯
```
最後にこのような画面が現れます．
```
 ▝▜▄     Gemini CLI v0.42.0-nightly.20260428.g59b2dea0e
   ▝▜▄
  ▗▟▀    Signed in with Google /auth
 ▝▀      Plan: Gemini Code Assist /upgrade

╭────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╮
│ We're making changes to Gemini CLI that may impact your workflow.                                                              │
│ What's Changing: We are adding more robust detection of policy-violating use cases and changing how we prioritize traffic.     │
│ How it affects you: This may result in higher capacity-related errors during periods of high traffic.                          │
│ Read more: https://goo.gle/geminicli-updates                                                                                   │
╰────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────╯

Tips for getting started:
1. Create GEMINI.md files to customize your interactions
2. /help for more information
3. Ask coding questions, edit code or run commands
4. Be specific for the best results


                                                                                                                  ? for shortcuts
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 Shift+Tab to accept edits
▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄▄
 >   Type your message or @path/to/file                                                                                           
▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀▀
 workspace (/directory)                         sandbox                           /model                                    quota
 ~/research/project1                            no sandbox                        Auto (Gemini 3)                         0% used

```
`/quit`とチャット欄に入力してGemini CLIを終了してください．

続いて，Gemini CLI をMCPサーバーとして登録していきます．
以下のコマンドを実行してください．
```
npm install -g mcp-gemini-cli
claude mcp add --scope user gemini-cli -- mcp-gemini-cli --allow-npx
```




