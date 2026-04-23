# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude Codeで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## ステップ3：Agentの構築
ステップ3では，実際にAgentを構築していきます．
また，ここからはVS CodeからDGX Sparkにssh接続して作業を進めていきます．
そのため，改行の方法も変わっているかもしれません．
チャットの入力が`option+Enter`で改行できます（ターミナルでもできたかもしれないですが，確かめていないです）．

### CLAUDE.mdの設計

**[参考1：Claude Code Docs（Claude があなたのプロジェクトを記憶する方法）](https://code.claude.com/docs/ja/memory)** \
**[参考2：効果的なCLAUDE.mdの書き方](https://zenn.dev/farstep/articles/how-to-write-a-great-claude-md)**

CLAUDE.mdは，Agentへの指示書のようなものです．
Claude Codeは，セッション開始時にCLAUDE.mdを自動的に読み込んで，記述された内容を会話のコンテキストに含めます．
これによって，異なるセッションにおいて，プロジェクトに関する共通の理解を持った状態から作業を行うことができます．
また，Claude Codeが同じミスを何度も繰り返す場合，CLAUDE.mdを修正してそのようなミスを防ぐことが可能になります．

VS Codeのエクスプローラーから`~/research/project1/CLAUDE.md`を開いて，CLAUDE.mdを修正していきます．
CLAUDE.mdの内容は，`/init`によって生成された初期状態のままなので，上書きで消してしまっても問題ありません．
```
```


