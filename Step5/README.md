# ステップ5：Claude Code CLI × Codex CLI の前準備
Claude Code CLI × Codex CLI による研究環境を構築するため，サブエージェントなどについてまとめます．
Claude Code CLI × Codex CLI で研究を進めるため，一度ABNのプロジェクトをバックアップだけとって削除しました．

## ステップ5.1：Claude Code サブエージェントと Agent Teams

**[参考１：Claude Code Docs of Sub Agent](https://code.claude.com/docs/ja/sub-agents)** \
**[参考２：Claude Code Docs of Agent Teams](https://code.claude.com/docs/ja/agent-teams)** \
**[参考３：Claude Code のサブエージェント機能の基本的な使い方](https://zenn.dev/ino_h/articles/2025-09-10-claude-code-subagents-basics)** \
**[参考４：Claude Codeの「サブエージェント」と「Agent Teams」は何が違うのか──設計レイヤで理解する使い分け](https://qiita.com/nogataka/items/df6c43496b2da9d41311)**

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




