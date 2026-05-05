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
ターミナル上で以下のコマンドを実行し，Filesystem MCPサーバーのパッケージをインストールします（Claude Code上ではないです）．
```
npm install -g @modelcontextprotocol/server-filesystem
```
```
claude mcp add --scope user filesystem mcp-server-filesystem /home/mprg/research
```



