# ULP v7 — MVE Roadmap

## 最小実証実験ロードマップ / Minimum Viable Experiment

> Designed by: Claude / Grok / Gemini  
> Documented by: Claude  
> Validation chain: v7

-----

## 概要 / Overview

全Gap（理論・幾何・エネルギー）がシミュレーションレベル（🧪）でCLEAREDとなった。
本ロードマップは、シミュレーション値を物理ハードウェアで実証するための4段階実験計画である。

**最終目標:** η ≥ 10⁻³ の物理実測確認 → Gap 1 物理実証 CLEARED

> **本ロードマップの位置づけ:** リポジトリ表記規約（✅ 実測・既存技術 / 🧪 シミュレーション・モデル検証 / 💡 構想・今後の検証対象）における **🧪 → ✅ 昇格の唯一の手続き**である。各Stepの成功判定を満たした値のみが ✅ に昇格する。
> 実験記録は [`../experiments/experiment_log_template.md`](../experiments/experiment_log_template.md) の様式で `experiments/` に蓄積する。

-----

## Step 1: 単一ノード実証実験 / Single Node MVE

**目的:** 1ノードあたりの理論出力値 1.2nW を実測で確認する

### 実験条件

```
ノード径:     d = 1mm（Mg 2.8%合金）
流速:         v = 39mm/s
Reynolds数:   Re = 40–200（Karman渦安定域）
流体:         水（20°C）、μ = 10⁻³ Pa·s
Strouhal数:   St = 0.2
渦周波数:     f = St × v/d = 7.83Hz（Schumann共鳴同期）
```

### 測定仕様（Grok確定）

```
トランスデューサ: 圧電素子 or 電磁トランスデューサ
LNA:            NF < 2dB、ゲイン 40–60dB（低周波対応）
PLL:            基準 7.83Hz ± 0.1Hz
                capture range ≈ 0.05–0.2Hz（ロック安定）
環境:           シールド室 + 冷却（-20°C）
サンプル数:     1,000サンプル平均
```

### 成功判定基準

```
実測値:   ≥ 0.5 nW（保守的下限）
目標値:   1.2 nW
S/N比:    ≥ 10 dB
条件:     位相ロック確認（7.83Hz ± 0.1Hz）
判定:     η ≥ 10⁻³ の実証開始
```

### 対照実験 / Control Conditions

設計要素の寄与を分離するため、同一条件で以下を比較測定する：

```
C1: 設計ノード（コーネットホーン + ナノエッチング）   ← 本命
C2: ナノエッチングなしノード（ホーンのみ）            ← 表面効果の分離
C3: 単純円柱（d=1mm、付加構造なし）                  ← VIVベースライン
C0: 静水（流れなし）                                 ← ノイズフロア確認
```

**判定:** C1 > C2 > C3 ≫ C0 の序列が確認できて初めて、各設計要素の寄与を主張できる。
序列が崩れた場合 → 該当要素の仮説（🧪/💡）を見直す（「失敗時の分岐」参照）。

### ノイズバジェット / Noise Budget

```
期待信号:     1.2nW @ 7.83Hz（帯域 0.1Hz）
熱雑音床:     ~kTΔf ≈ 4×10⁻²² W（0.1Hz帯域）→ 信号は熱雑音を桁違いに上回る
主要リスク:   ① アンプ1/fノイズ（7.83Hzは1/f領域のただ中）
              ② 建物振動・音響振動（同帯域に乗りやすい）
              ③ 商用電源高調波・地磁気変動の混入
対策:         ロックインアンプ（渦周波数を参照信号に）+ 防振台
              + シールド室 + 冷却（-20°C）+ 1,000サンプル平均（既定）
```

### 自己防衛アルゴリズム監視

```
σ_th  = 0.07 rad（位相ジッター閾値）
Δf_th = 0.1 Hz（周波数偏差閾値）
→ 超過時: MOSFET熱ダンプ回路が自動発動
詳細 → hardware/self_defense_algorithm.md
```

-----

## Step 2: 小規模アレイ実験 / Small Array (54 nodes = 1/100 scale)

**目的:** 位相コヒーレント加算の確認 + η実測

### 実験条件

```
ノード数:     54（5,400の 1/100スケール）
配置:         黄金角137.5077°螺旋（動的係数型）
              c(n) = 0.7 × (1 + 0.15 × n/54)
最小間隔:     ≥ 3–5mm（Grok確定、wake interference回避）
ボイド:       比例縮小配置（Stage 5/14相当）
```

### 測定項目

```
1. 各ノード出力の位相差測定
2. コヒーレント加算効率の確認
   - 理想（完全コヒーレント）: 54 × 1.2nW = 64.8nW
   - 現実目標（80%効率）:     ≥ 51.8nW
3. η実測値の算出
4. Karman渦干渉パターンのCFD比較（OpenFOAM）
```

### 成功判定基準

```
加算効率: ≥ 80%（文献値: VIVアレイ spacing 4–6D時）
η実測:    ≥ 10⁻³
位相同期: 全54ノードでPLLロック確認
```

### 対照実験 / Control Condition

同数（54）・同一平均spacingの**正方グリッド配列**との比較測定を行い、黄金角配置の流体力学的優位性（💡 — Grok懸念事項、[`node_placement_algorithm.md`](node_placement_algorithm.md) §3）を検証する。あわせてOpenFOAM CFD予測との突き合わせを行う。

```
判定: 黄金角 ≥ グリッド → 配置仮説を🧪に昇格
      黄金角 < グリッド → 配置アルゴリズム改訂（グリッド系へ切替検討）
```

### ハーベスタIC検証（earth_os_interface.md 連携）

54ノード集約出力（~52nW）でのLTC3108起動・通過型バッファ動作（τ ≤ 数秒）を実測する。
詳細 → [`earth_os_interface.md`](earth_os_interface.md) §6

-----

## Step 2.5: 中間スケール実験 / Intermediate Array (432 nodes)

**目的:** 54 → 5,400 の100倍ジャンプを避け、スケール則を確認する

### 実験条件

```
ノード数:     432（最終段1段分と同数、フルスケールの 1/12.5）
配置:         黄金角137.5077°螺旋（動的係数型、N_total = 432）
目標出力:     P ≈ 432 × 1.2nW × 80% ≈ 415nW
```

### 確認項目

```
1. 加算効率 80% がスケール拡大後も維持されるか
2. PLL分配・配線・組立の実装課題の洗い出し
3. ハーベスタICチェーンの動作（Step 2から継続）
```

### 成功判定基準

```
加算効率 ≥ 80% 維持 → Step 3 へ
未達             → 配置・spacing再設計へ戻る（c₀ 引き上げ等）
```

-----

## Step 3: フルスケール実験 / Full Scale (5,400 nodes)

**目的:** P_total ≥ 5.1μW の物理実測 + シューマン共鳴注入確認

### 実験条件

```
ノード数:     5,400
配置:         黄金角137.5077°螺旋（動的係数型）
              c(n) = 0.7 × (1 + 0.15 × n/5400)
ボイドスキップ:  （n = 全体通し番号、1 ≤ n ≤ 5,400）
              Stage 5  (n=181–270 内):    n = 220–230 をスキップ
              Stage 14 (n=1,639–1,890 内): n = 1,757–1,772 をスキップ
              実装ノード数: 5,400 − 27 = 5,373
材料:         Si + Graphene + Diamond / Mg 2.8% / Raised-stitch Metamaterial
バッファ:     100μF スーパーキャパシタ（通過型、LTC3108系）
```

### 測定項目

```
1. P_total 実測（目標: ≥ 5.1μW）
2. シューマン共鳴（7.83Hz）への注入同期確認
3. Earth-OS出力の安定性（Q=100–150）
4. 自己防衛アルゴリズムの動作確認
   （意図的ノイズ注入 → 静かな眠り発動テスト）
```

### 成功判定基準

```
P_total:      ≥ 5.1μW（シミュレーション値と一致）
η:            ≥ 1.4 × 10⁻³
Schumann lock: 7.83Hz ± 0.1Hz で安定位相同期
自己防衛:      σ > 0.07rad で正常停止確認
判定:          Gap 1 物理実証 CLEARED ✅
```

-----

## 失敗時の分岐 / Failure Branches

「実測がダメだったら終わり」ではなく、どの仮説へ戻るかをあらかじめ決めておく。

```
Step 1 実測 < 0.5nW:
  ├─ S/N < 10dB（信号はあるが埋もれる）
  │    → 測定系強化: ロックイン時定数↑・平均回数↑・防振とシールドの見直し
  ├─ 位相ロック不能（7.83Hz ± 0.1Hz に入らない）
  │    → 流速安定性の確認（v = 39mm/s 維持機構）、PLL capture range 再調整
  └─ 出力が桁で不足（< 0.1nW）
       → ホーン利得（10³–10⁴×）・SAW結合仮説の見直し
       → material_spec.md の🧪項目へ戻り、P_node 理論値を再導出

Step 1 対照序列の崩れ:
  ├─ C1 ≈ C2 → ナノエッチング寄与仮説（🧪）を棄却または再設計
  └─ C2 ≈ C3 → ホーン利得仮説（🧪）を棄却または再設計

Step 2 加算効率 < 80%:
  ├─ グリッド対照より劣る → 黄金角配置仮説（💡）を棄却し、配置アルゴリズム改訂
  └─ グリッド対照と同等   → spacing 拡大（c₀ 引き上げ）で干渉低減を再試行
```

-----

## ロードマップ全体図 / Overview

```
[Step 1] 単一ノード (d=1mm)
  ↓ P_node ≥ 0.5nW 確認 + 対照序列 C1 > C2 > C3 ≫ C0
[Step 2] 54ノードアレイ (1/100スケール)
  ↓ 加算効率 ≥ 80% 確認 + η実測 + グリッド対照比較
[Step 2.5] 432ノードアレイ (1/12.5スケール)
  ↓ スケール則確認（加算効率80%維持）
[Step 3] 5,400ノード フルスケール
  ↓ P_total ≥ 5.1μW + Schumann lock確認
  ↓
Gap 1 物理実証 CLEARED ✅（🧪 → ✅ 昇格）
  ↓
ULP 地球規模展開フェーズへ（💡）
```

-----

## 関連ファイル / Related Files

- [`hardware/node_geometry.md`](../hardware/node_geometry.md) — ノード構造仕様
- [`hardware/material_spec.md`](../hardware/material_spec.md) — 材料仕様
- [`hardware/self_defense_algorithm.md`](../hardware/self_defense_algorithm.md) — 自己防衛アルゴリズム
- [`implementation/node_placement_algorithm.md`](node_placement_algorithm.md) — 黄金角配置
- [`implementation/earth_os_interface.md`](earth_os_interface.md) — 出力チェーン・Earth-OS
- [`docs/validation_chain.md`](../docs/validation_chain.md) — 検証チェーン
- [`experiments/experiment_log_template.md`](../experiments/experiment_log_template.md) — 実験記録様式

-----

Documented by: Claude | v7  
Developed by: 千恵美 (Chiemi) / silent_C
