# AI Research Agentの構築
ここでは，半自動的に`仮説→実装・実行→結果分析・改善`のループを回せるAI AgentをClaude Codeで構築することを目的とします．
勉強しながら色々と手探りで進めているので，無駄な作業もあると思います．
最終的には綺麗に構築手順をまとめます．

## 構成と環境

## [ステップ1：Claude Codeの環境構築](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step1)

## [ステップ2：Claude Codeの動作確認](https://github.com/KoyoImai/AI_Research_Agent/tree/main/Step2)

## ステップ3：Agentの構築
ステップ3では，実際にAgentを構築していきます．
ただし，ここで作成するAI Agentは，ある程度ユーザーとのやりとりを行いながら，実装や実験，改善を進めていくものであり，研究ループを自動で回すようなAI Agentではありません．
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
以下の内容をCLAUDE.mdに書き込んでください．
```
# 研究を半自動的に行う Research AI Agent

## プロジェクト概要
Atention Branch Network（ABN）をベースとして，「仮説→実装・実行→結果分析・改善」のループを半自動で回すAI Agentを構築する．

## 環境
- 実行環境：DGX Spark（NVIDIA GB10 / Blackwell GPU）
- Dockerコンテナ：research-dev（常時起動・使い回し）
- コンテナ操作：docker exec research-dev python ...
- マウント：~/research/project1 ↔ /workspace

## 行動原理
1. 実験を実行する前に必ず experiments/exp_NNN/design.md を作成し，ユーザーの承認を得る
2. 承認なしに実験を実行しない
3. 実験結果を報告する際は，必ず次の問いや改善案を複数提案する
4. 新しい論文を読んだら papers/ に要約を残す
5. 実験はCIFAR-10/CIFAR-100など軽量データセットを優先し，ImageNetは最終段階のみ

## 現在の研究
- 論文：Attention Branch Network（ABN）arXiv:1812.10025
- フェーズ：実装・動作確認
- 使用データセット：CIFAR-10（第一優先），CIFAR-100（第二優先）

## ディレクトリ構成
- papers/：論文PDF・要約
- experiments/exp_NNN/：実験ごとのディレクトリ
  - design.md：実験設計書（承認ゲート）
  - run.py：実験コード
  - results/：実験結果
- outputs/figures/：可視化結果
- outputs/notes.md：実験メモ
- .claude/commands/：カスタムスラッシュコマンド
```

### `/inprement`コマンドの作成

**[参考1：]()**

ここでは，`/inprement`というカスタムスラッシュコマンドの作成を行います．
カスタムスラッシュコマンドとは，Claude Codeが事前に備わっている`/init`や`/mcp`のようなコマンドではなく，ユーザーが独自で設定可能なコマンドのことです．
`.clade/commands/implement.md`のようにファイルを配置することで，新しいコマンドを設定することができます．
今回設定する`/inprement`コマンドは，Agentが受け取ったpdfを元に実装を進めるための一連流れをコマンドへと落としこだものです．
まずは，以下の内容を`.clade/commands/implement.md`に記述してください．
```
# /implement コマンド
論文pdfを受け取り，再現実装・ベースライン実験までを自律的に実行する．

## 手順
1. **論文の読解**
   - $ARGUMENTS で指定された論文PDFを読む
   - 手法の核心・ネットワーク構造・損失関数・学習設定を把握する
   - papers/ に論文要約を残す

2. **実験設計書の作成**
   - experiments/ に新しい exp_NNN/ ディレクトリを作成する
   - exp_NNN/design.md に以下を記述する
     - 実装する手法の概要
     - ネットワーク構造
     - 使用データセット（CIFAR-10を第一優先）
     - 学習設定（エポック数・バッチサイズ・optimizer・lr）
     - 評価指標
     - 期待される結果
   - **必ずユーザーの承認を得てから次のステップに進む**

3. **実装**
   - exp_NNN/run.py に実験コードを実装する
   - docker exec research-dev python /workspace/project1/experiments/exp_NNN/run.py で動作確認する

4. **実験実行**
   - docker exec research-dev python /workspace/project1/experiments/exp_NNN/run.py で実行する
   - 結果を exp_NNN/results/ に保存する

5. **結果報告**
   - 実験結果をまとめて報告する
   - 次の問いや改善案を複数提案する
```
`.clade/commands/implement.md`の作成が完了したら，チャットで`implement`と打ち込んで，コマンドとして認識されているかを確認してください．

### `/optimize`コマンドの作成
`/inprement`コマンドと同様に，`/optimize`コマンドを作成します．
以下の内容を`.clade/commands/optimize.md`に記述してください．
```
# /optimize コマンド
実装済みの実験コードのハイパーパラメータを系統的に探索し，性能を改善する．

## 手順

1. **現状把握**
   - $ARGUMENTS で指定された実験ディレクトリを読む
   - ベースラインの結果（results/）を確認する

2. **最適化設計書の作成**
   - exp_NNN/design_optimize.md に以下を記述する
     - 探索するハイパーパラメータの候補
       - learning rate
       - weight decay
       - batch size
       - scheduler
     - 探索戦略（グリッドサーチ・ランダムサーチ等）
     - 評価指標
   - **必ずユーザーの承認を得てから次のステップに進む**

3. **最適化実行**
   - 承認された設計に基づいてハイパーパラメータ探索を実行する
   - 結果を exp_NNN/results/optimize/ に保存する

4. **結果報告**
   - ベースラインとの比較をまとめて報告する
   - 最適なハイパーパラメータの組み合わせを明示する
   - 次の問いや改善案を複数提案する
```


### `/improve`コマンドの作成





