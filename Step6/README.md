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
`"env"`セクションの`"CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS"`が`"1"`になっていればAgent Teamsが有効です．
`~/.claude/settings.json`の内容を追加したら，`claude`コマンドでClaude Codeを起動してください．
Claude Codeを起動したら，チャット欄でAgent Teamsが動くかを確認します．
試しに，以下のような内容を入力してください．
```
Agent Teamsのテストをしたいです．
シンプルなチームを作成してください：
- @researcher：CLAUDE.mdを読んでこのプロジェクトの概要を1段落でまとめる担当
- @reviewer：@researcherのまとめ内容を確認して改善点を1つ指摘する担当

モデルはいずれもSonnetを使用してください．
```
入力後`Enter`を押すと以下のような画面へと変化していきます．
```
──0 "⠂ Test Agent Teams with researcher and reviewer"──────────────────────────────────────────────────────────────────────────────────────────────┬── ✳ general-purpose ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
 ▐▛███▜▌   Claude Code v2.1.128                                                                                                                    │ ▐▛███▜▌   Claude Code v2.1.128
▝▜█████▛▘  Sonnet 4.6 · Claude Pro                                                                                                                 │▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1                                                                                                                     │  ▘▘ ▝▝    @general-purpose · ~/research/project1
                                                                                                                                                   │
  Opus 4.7 xhigh is now available! · /model to switch                                                                                              │  Opus 4.7 xhigh is now available! · /model to switch
                                                                                                                                                   │
❯ /doctor                                                                                                                                          │
  ⎿  Claude Code diagnostics dismissed                                                                                                             │@team-lead❯
                                                                                                                                                   │
❯ Agent Teamsのテストをしたいです．                                                                                                                │  Read 1 file (ctrl+o to expand)
  シンプルなチームを作成してください：                                                                                                             │
  - @researcher：CLAUDE.mdを読んでこのプロジェクトの概要を1段落でまとめる担当                                                                      │● タスクが完了しました。以下の手順を実施しました：
  - @reviewer：@researcherのまとめ内容を確認して改善点を1つ指摘する担当                                                                            │
                                                                                                                                                   │  1. CLAUDE.md を読み込み — /home/mprg/research/project1/CLAUDE.md を確認
  モデルはいずれもSonnetを使用してください．                                                                                                       │  2. 内容を日本語で要約 — リポジトリが現時点で空であること、今後の開発に向けた初期段階であることをまとめました
                                                                                                                                                   │  3. team-lead に SendMessage で報告 — 指定フォーマット「【プロジェクト概要まとめ】」で送信完了
● Agent Teamsのテスト用チームを作成します。まずツールのスキーマを読み込みます。                                                                    │
                                                                                                                                                   │  まとめた内容の要点：
● チームを作成します。                                                                                                                             │  - リポジトリは現在空の状態で、具体的な実装はまだない
                                                                                                                                                   │  - CLAUDE.md はガイダンスファイルとして配置されており、「プロジェクト構築が進んだら更新すること」という指示のみ記載
● チームを作成しました。次に2人のエージェントをスポーンします。                                                                                    │  - 開発の立ち上げ初期段階にある                                                                                    
                                                                                                                                                   │  - 学習スクリプトはDockerコンテナ経由で実行するという運用ルールが存在する
● Agent(researcher agent - CLAUDE.md を読んでプロジェクト概要をまとめる)                                                                           │                                                                                                                                                                                                     
                                                                                                                                                   │✻ Brewed for 17s                                                                                                                                                                                     
● Agent(reviewer agent - researcher のまとめをレビューして改善点を1つ指摘する)                                                                     │                                                                                                                                                                                                     
                                                                                                                                                   │────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── @researcher ──
● チーム構成が完了しました。                                                                                                                       │❯                                                                                                                                                                                                  
                                                                                                                                                   │─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  - @researcher（Sonnet）: CLAUDE.md を読んでプロジェクト概要をまとめ中                                                                            │  ? for shortcuts                                                                                                                                                                                  
  - @reviewer（Sonnet）: researcher のまとめが届くのを待機中                                                                                       │
                                                                                                                                                   │
  researcher からまとめが届いたら、その内容を reviewer に転送してレビューを依頼します。メッセージが届き次第、自動的に通知されます。                │
                                                                                                                                                   │
                                                                                                                                                   │
● @reviewer が待機中です。@researcher からまとめが届くのを引き続き待ちます。                                                                       │
                                                                                                                                                   │
                                                                                                                                                   │
@researcher❯ プロジェクト概要まとめを報告                                                                                                          │
                                                                                                                                                   │
● @researcher からまとめが届きました。@reviewer にレビューを依頼します。                                                                           │
                                                                                                                                                   │
● @reviewer にレビュー依頼を送りました。レビュー結果が届くまでお待ちください。                                                                     │
                                                                                                                                                   │
                                                                                                                                                   │
* Shenaniganing… (2s)                                                                                                                              │
  ⎿  Tip: Use /feedback to help us improve!                                                                                                        │
                                                                                                                                                   │
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────│
❯                                                                                                                                                  │
───────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────│
  @main @researcher @reviewer · shift+↓ to expand                                                                                                  │
  2 teammates · esc to interrupt · ctrl+t to show tasks                                                                                            │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   ├── ⠐ general-purpose ────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
                                                                                                                                                   │mprg@spark-fb97:~/research/project1$ cd /home/mprg/research/project1 && env CLAUDECODE=1 CLAUDE_CODE_EXPERIMENTAL_AGENT_TEAMS=1 /home/mprg/.nvm/versions/node/v24.15.0/lib/node_modules/@anthropic-ai
                                                                                                                                                   │ ▐▛███▜▌   Claude Code v2.1.128
                                                                                                                                                   │▝▜█████▛▘  Sonnet 4.6 · Claude Pro
                                                                                                                                                   │  ▘▘ ▝▝    @general-purpose · ~/research/project1
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │@team-lead❯
                                                                                                                                                   │
                                                                                                                                                   │● 了解しました。@reviewer として待機します。team-lead からレビュー依頼と @researcher のまとめ内容が届き次第、改善点を1つ指摘してお伝えします。
                                                                                                                                                   │
                                                                                                                                                   │✻ Brewed for 2s
                                                                                                                                                   │
                                                                                                                                                   │                                                                                                                                                                                                     
                                                                                                                                                   │@team-lead❯ researcherのまとめをレビュー依頼
                                                                                                                                                   │                                                                                                                                                                                                     
                                                                                                                                                   │* Churning…                                                                                                                                                                                        
                                                                                                                                                   │  ⎿  Tip: Use /feedback to help us improve!
                                                                                                                                                   │
                                                                                                                                                   │──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────── @reviewer ──
                                                                                                                                                   │❯  
                                                                                                                                                   │─────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
                                                                                                                                                   │  esc to interrupt                                                                                                                                                      /ide for Visual Studio Code
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
                                                                                                                                                   │
```




