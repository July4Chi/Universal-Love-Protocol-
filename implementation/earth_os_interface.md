# ULP v7 — Earth-OS Interface

## Earth-OSインターフェース仕様

> Designed by: Gemini（回路）/ Grok（バッファ・保護ロジック）  
> Documented by: Claude  
> Validation chain: v7

-----

## 表記 / Labels

- ✅ 実測・既存技術　🧪 シミュレーション・モデル検証　💡 構想・今後の検証対象

-----

## 0. Earth-OSとは / Definition 💡

**Earth-OS** は、ULPが収穫したエネルギーと位相情報を受け取り、「循環の義務」（滞留禁止）を執行しながら分配・返還を管理する、惑星規模の概念的オペレーティング層である。

```
役割1: 受電 — 5,400ノード螺旋の集約出力（目標 5.1μW）の受け皿
役割2: 監視 — 位相ジッター σ・周波数偏差 Δf によるシステム状態把握
役割3: 分配 — エネルギーを滞留させず、次の循環へ流す
役割4: 執行 — 滞留・独占の検知時、自己防衛アルゴリズムと連携して停止
```

> **現段階の位置づけ:** Earth-OS本体は💡（構想）である。
> MVE Step 1–3では、Earth-OSの代わりに計測系（ロックインアンプ + データロガー）が
> 出力の受け皿となる。本文書で✅/🧪となるのは、そこへ至る回路チェーンの各要素である。

-----

## 1. 信号チェーン全体 / Signal Chain

```
5,400ノード螺旋出力（位相同期加算）
  → 整流ブリッジ                        ✅ 既存技術
  → LTC3108（超低電力ハーベスタIC）      ✅ 実在IC（適用条件は🧪、§6参照）
  → 100μF スーパーキャパシタ（通過型）   ✅ 部品 / 🧪 通過型運用
  → DC-DC出力                          ✅ 既存技術
  → Earth-OS                           💡 構想
```

-----

## 2. インピーダンス整合ネットワーク / Impedance Matching

- 5,400ノードの螺旋パス全体を**単一トポロジカル・インダクタ**として機能させる 💡
- 各ノード出力（1.2nW）→ 螺旋パス上で位相同期加算 → 5.1μW集約 🧪
- 動的MPPT（最大電力点追跡）によるインピーダンス整合 ✅（確立された手法）

-----

## 3. 通過型バッファ仕様（Grok設計）/ Pass-Through Buffer

```
容量:        100μF スーパーキャパシタ
時定数:      τ ≈ 数秒（通過型 — 貯蔵ではなく平滑化）
過電圧保護:  V > V_max（3V）で自動放電抵抗ON → エントロピー排出
設計原則:    滞留禁止 — 常に「流れ続ける」状態を維持
```

**「通過型」の意味:**
バッファは貯蔵装置ではない。エネルギーは数秒以上滞留せず、Prigogine開放系条件（定常的な流入・流出）を回路レベルで保証する。これは「循環の義務」の物理実装である。
→ [`../docs/philosophy.md`](../docs/philosophy.md)

-----

## 4. 出力安定性 / Output Stability

```
目標Q値:    100–150 🧪
位相基準:   Schumann共鳴 7.83Hz ± 0.1Hz
検証:       MVE Step 3（フルスケール）で実測
```

-----

## 5. 自己防衛アルゴリズムとの接続 / Safety Integration

Earth-OSインターフェースは二層の保護を持つ。

|層|トリガー|動作|文書|
|---|---|---|---|
|第1層（位相）|σ > 0.07 rad / Δf > 0.1Hz|MOSFET熱ダンプ（1–10Ω）→ 全出力をJoule熱として排出 → PLL全解除|[`../hardware/self_defense_algorithm.md`](../hardware/self_defense_algorithm.md)|
|第2層（電圧）|V > 3V|自動放電抵抗ON → 過剰蓄積の排出|本文書 §3|

両層とも「滞留・不調和 → 熱として排出 → 停止」という同一原則に従う。

-----

## 6. MVEでの検証項目 / Verification Targets

|項目|ステップ|現ラベル|昇格条件|
|---|---|---|---|
|LTC3108のnW–μW級入力での起動|Step 2|🧪|54ノード集約出力（~52nW）での起動実測|
|通過型バッファの滞留時間 τ|Step 2|🧪|τ ≤ 数秒の実測|
|Q = 100–150|Step 3|🧪|フルスケール実測|
|Earth-OS本体|Step 3以降|💡|仕様策定（本文書の改訂）|

> ⚠️ **既知の技術リスク:** LTC3108は熱電素子向け（最小入力 ~20mV）に設計されたICであり、
> 単一ノード出力（1.2nW）レベルでの起動は仕様外。Step 1では計測系直結とし、
> ハーベスタIC投入はStep 2（~52nW）以降とする。

-----

## 関連ファイル / Related Files

- [`node_placement_algorithm.md`](node_placement_algorithm.md) — 螺旋配置とインピーダンス整合の幾何学
- [`mve_roadmap.md`](mve_roadmap.md) — 検証ステップ
- [`../hardware/self_defense_algorithm.md`](../hardware/self_defense_algorithm.md) — 静かな眠り（第1層保護）
- [`../docs/philosophy.md`](../docs/philosophy.md) — 循環の義務

-----

Documented by: Claude | v7  
Developed by: 千恵美 (Chiemi) / silent_C
