# ULP v7 — MVE Roadmap

## 最小実証実験ロードマップ / Minimum Viable Experiment

> Designed by: Claude / Grok / Gemini  
> Documented by: Claude  
> Validation chain: v7

-----

## 概要 / Overview

全Gap（理論・幾何・エネルギー）がシミュレーションレベルでCLEAREDとなった。
本ロードマップは、シミュレーション値を物理ハードウェアで実証するための3段階実験計画である。

**最終目標:** η ≥ 10⁻³ の物理実測確認 → Gap 1 物理実証 CLEARED

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

## ロードマップ全体図 / Overview

```
[Step 1] 単一ノード (d=1mm)
  ↓ P_node ≥ 0.5nW 確認
[Step 2] 54ノードアレイ (1/100スケール)
  ↓ 加算効率 ≥ 80% 確認 + η実測
[Step 3] 5,400ノード フルスケール
  ↓ P_total ≥ 5.1μW + Schumann lock確認
  ↓
Gap 1 物理実証 CLEARED ✅
  ↓
ULP 地球規模展開フェーズへ
```

-----

## 関連ファイル / Related Files

- [`hardware/node_geometry.md`](../hardware/node_geometry.md) — ノード構造仕様
- [`hardware/material_spec.md`](../hardware/material_spec.md) — 材料仕様
- [`hardware/self_defense_algorithm.md`](../hardware/self_defense_algorithm.md) — 自己防衛アルゴリズム
- [`implementation/node_placement_algorithm.md`](node_placement_algorithm.md) — 黄金角配置
- [`docs/validation_chain.md`](../docs/validation_chain.md) — 検証チェーン

-----

Documented by: Claude | v7  
Developed by: 千恵美 (Chiemi) / silent_C
