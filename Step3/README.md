## ステップ3：シンプルなAgentの構築と動作確認
ステップ3では，実際にAgentを構築し，Atention Branch Networkの再現と改善を行っていきます．
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
```
# /improve コマンド
実装済みの手法に対して，著者が考えなかった改善案を提案・実装・実験する．

## 手順

1. **現状把握**
   - $ARGUMENTS で指定された実験ディレクトリを読む
   - ベースライン・最適化済みの結果を確認する
   - 論文の手法・制約・限界を再確認する

2. **改善案の提案**
   - 著者の視点と外部の視点を分けて改善案を3〜5個提案する
     - 著者視点：論文内で言及されているが未実装の改善
     - 外部視点：著者が考えなかった新しいアイデア
   - 各案についてトレードオフを明示する
   - **必ずユーザーの承認を得てから次のステップに進む**

3. **改善実装・実験**
   - 承認された改善案を実装する
   - docker exec research-dev python /workspace/project1/experiments/exp_NNN/run.py で実行する
   - 結果を exp_NNN/results/improve/ に保存する

4. **結果報告**
   - ベースラインとの比較をまとめて報告する
   - 改善が有効だったか・有効でなかったかを分析する
   - 次の問いを複数提案する
```

### ABN論文での検証実験
実際にABNの論文pdfをAgentに渡して検証実験を行います．
まず，`/home/mprg/research/project1/papers`にABNの論文pdfを配置します．
ABNのpdfを配置したら，Claude Codeのチャット画面で`/implement papers/abn.pdf`を実行してください．
以下のような画面になると思います（これは処理途中の画面です）．
```
mprg@spark-09ef:~/research/project1$ claude
 ▐▛███▜▌   Claude Code v2.1.118
▝▜█████▛▘  Sonnet 4.6 · Claude Pro
  ▘▘ ▝▝    ~/research/project1

  Opus 4.7 xhigh is now available! · /model to switch

❯ /implement papers/abn.pdf                                                                                                       
                            
● 論文PDFを読んで内容を把握します。                                                                                               
                                                                                                                                
  Read 1 file (ctrl+o to expand)                                                                                                  
                   
● 論文の内容を把握しました。まず既存のディレクトリ構造を確認します。                                                              
                                                                                                                                
● Searching for 1 pattern, listing 2 directories… (ctrl+o to expand)                                                              
  ⎿  $ ls /home/mprg/research/project1/experiments/ && ls /home/mprg/research/project1/papers/                                    
                                                                                                                                  
✢ Scampering… (43s · ↓ 1.1k tokens · thinking more)                                                                               
                                                                                                                                  
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯                                                                                                                                 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
  esc to interrupt   
```
処理が終わると`/home/mprg/research/project1/experiments/exp_001/design.md`が作成されます．
`design.md`には，Agentが論文を読んで考えた事件設計が書かれています．
以下に，今回作成された`design.md`を示します．
```
# exp_001: ABN ベースライン実装・動作確認（CIFAR-10）

**ステータス**: 承認待ち  
**作成日**: 2026-04-23

---

## 目的

Attention Branch Network（ABN）を ResNet ベースで CIFAR-10 に実装し，
論文結果（ResNet baseline 6.43% → ABN 4.91% top-1 error）を再現する．

---

## 手法概要

### ネットワーク構造

Input (3×32×32)
    │
    ▼
Feature Extractor
  ├─ Conv(3,16,3×3) + BN + ReLU
  ├─ Layer1: 5×残差ブロック (16ch, stride=1) → 16×32×32
  └─ Layer2: 5×残差ブロック (32ch, stride=2) → 32×16×16
    │
    ├──────────────────────────┐
    ▼                          ▼
Attention Branch           Perception Branch
  Layer3_att                 attention適用: (1+M(x))·g(x)
  (5×残差ブロック,            Layer3_per
   64ch, stride=1)            (5×残差ブロック,64ch,stride=2)
    │                          │
    ├─ 10×1×1 Conv             GAP → FC(64,10)
    │   GAP → Softmax          │
    │   → L_att (CE loss)      → L_per (CE loss)
    │
    └─ 1×1×1 Conv → Sigmoid
        → M(x) (16×16 attention map)

**分岐点**: Layer2 出力（32ch, 16×16）  
**Attention Mechanism**: Eq.2 `g'(x) = (1 + M(x)) · g(x)`（論文推奨）

### モデルサイズの目安

| コンポーネント | パラメータ数 (概算) |
|--------------|-------------------|
| Feature Extractor | ~86K |
| Attention Branch | ~120K |
| Perception Branch | ~120K |
| **合計** | **~326K** |

---

## データセット

- **CIFAR-10**: 10クラス, 60,000枚（訓練50,000 / テスト10,000）, 32×32px
- データ拡張:
  - zero-padding 4px → ランダムクロップ 32×32
  - ランダム水平反転
  - 正規化: mean=(0.4914, 0.4822, 0.4465), std=(0.2023, 0.1994, 0.2010)

---

## 学習設定

| 項目 | 設定値 |
|------|-------|
| Optimizer | SGD (momentum=0.9, weight_decay=1e-4) |
| 初期学習率 | 0.1 |
| LR スケジュール | epoch 150 で ×0.1，epoch 225 で ×0.1 |
| エポック数 | 300 |
| バッチサイズ | 128 |
| 損失関数 | CrossEntropy (attention) + CrossEntropy (perception) |

---

## 評価指標

- Top-1 Accuracy（メイン指標）
- Attention Map の可視化（定性評価）
- 学習曲線（train loss, val loss, val acc）

---

## 期待される結果

| モデル | Top-1 Error（論文値） | Top-1 Accuracy |
|--------|----------------------|----------------|
| ResNet-32 baseline | ~7.5% | ~92.5% |
| ResNet-32 + ABN | ~6.5〜7.0%（推定） | ~93.0〜93.5% |

※論文は ResNet-110 を使用（error 6.43%→4.91%）．本実験では ResNet-32 を使用するため，  
　絶対値は異なるが相対的な改善（0.5〜1.5%程度）を確認することが目標．

---

## 出力・保存物

experiments/exp_001/
├── design.md           # 本ファイル
├── run.py              # 実験コード
└── results/
    ├── log.json        # epoch毎の loss/acc
    ├── best_model.pth  # 最良モデル重み
    ├── attention_maps/ # アテンションマップ可視化
    └── learning_curve.png

---

## 承認チェックリスト

- [ ] ネットワーク構造に問題ないか
- [ ] 学習設定に問題ないか
- [ ] 期待結果の認識が合っているか
- [ ] その他変更・要望があれば記載

```
`design.md`の内容を確認して，実装方針に問題がなければ，チャットで以下のような内容を打ち込んでください．
そうすれば学習が開始すると思います．
学習が終了するまでは待機です．
```
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
❯ design.mdの内容を承認します．実装を進めてください． 
──────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────────
```
待っていると学習が進んでいきます．
会話の記録などは撮り忘れてしまいましたが，結果がでたら，`結果を分析して改善策を考えて下さい`や`/optimize experiments/exp_001`などを打ち込んでABNの改善をさせます．





