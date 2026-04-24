# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude CodeとCodexで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境

**[参考1：Claude Code Docs](https://code.claude.com/docs/en/best-practices)** \
**[参考2：Claude Code Orchestra: Claude Code × Codex CLI × Gemini CLIの最適解を探る](https://zenn.dev/mkj/articles/claude-code-orchestra_20260120)** \
**[参考3：Claude × Codex × Gemini を"併用"する設計 ─ セカンドオピニオン運用フレーム](https://qiita.com/nogataka/items/b2b4a84ba611ccaf8447)**

## 現状の疑問
### 疑問1
VS Codeの拡張機能にあるClaude CodeやCodexを使うのは可能なのか？
ローカルpc（手元のmac book）でDGX Sparkにssh接続して，ローカルpcのVS CodeでClaude CodeとCodexを起動していると，ローカルpcを閉じたらAgentの稼働も止まるのでは？

### 回答1
一旦CLIで環境を構築し，VS Codeの拡張機能は後から確かめる．

### 疑問2
gitやarXivなどの接続はどうする？

### 回答2
MCPサーバーを導入予定．

### 

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## [ステップ3：シンプルなAgentの構築と動作確認（ABNで実行中）](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step3)

## [ステップ4：Codex CLI のインストール・動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step4)

## ステップ5：
