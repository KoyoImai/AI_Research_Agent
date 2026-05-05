# ステップ6：Claude Code CLI の Agent Teams
ここでは，Claude CodeのAgent Teamsを構築していきます．


## ステップ6.1：Claude Code サブエージェントと Agent Teams について（再掲）

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


## ステップ6.2：Agent Teamsの有効化
Agent Teamsは，デフォルトで無効になっています．
まず，Agent Teamsを有効にするため，`~/.claude/settings.json`を開いて内容を編集します．
以下が私の環境における`~/.claude/settings.json`です．
```
{
  "theme": "dark",
  "env": {
    "CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS": "1"
  }
}
```
