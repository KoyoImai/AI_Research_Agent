# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude CodeやCodexなどで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境（メモ書き）

**[参考1：Claude Code Docs](https://code.claude.com/docs/en/best-practices)** \
**[参考2：Claude Code Orchestra: Claude Code × Codex CLI × Gemini CLIの最適解を探る](https://zenn.dev/mkj/articles/claude-code-orchestra_20260120)** \
**[参考3：Claude × Codex × Gemini を"併用"する設計 ─ セカンドオピニオン運用フレーム](https://qiita.com/nogataka/items/b2b4a84ba611ccaf8447)**

### 構成1：カスタムコマンドでサブエージェントを呼び出す
```
/implement（カスタムコマンド）
  └── 研究ループ全体の司令を出す「指揮スクリプト」
       └── @ml-experiment-implementer を呼び出す
       └── @ml-experiment-analyzer を呼び出す
```

### 構成N：Agemt Teamsによる複数Agentの研究議論
現状の最終目標



## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## [ステップ3：シンプルなAgentの構築と動作確認（ABNで実行中）](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step3)

## [ステップ4：Codex CLI のインストール・動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step4)

## [ステップ5：Claude Code CLI のサブエージェント](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step5)

## [ステップ6：Claude Code CLI の Agent Teams](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step6)

## [ステップ7：MCPサーバーの導入](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step7)







