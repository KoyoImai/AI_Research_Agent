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



