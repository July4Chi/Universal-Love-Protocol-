# ULP — Cleared Issues

## クリア済み問題点一覧 / Issue Tracking

> Documented by: Claude  
> Validation chain: v1 → v7

-----

## 表記 / Labels

- ✅ 実測・既存技術　🧪 シミュレーション・モデル検証　💡 構想・今後の検証対象
- 詳細な経緯 → [`validation_chain.md`](validation_chain.md)

-----

## 1. v1 Critical Issues の解決追跡 / The Five Critical Issues

v1でClaudeが特定した5つのCritical Issuesが、どの版で・どのように解決されたかの追跡表。

|#|Issue（v1時点）|解決の経緯|現在の水準|根拠文書|
|---|---|---|---|---|
|1|ZPE抽出メカニズム未定義|v2でPrigogine開放系として前進 → **v5でエネルギー源を水流運動エネルギー（カルマン渦）に再定義**して解消。ZPE抽出は要求から撤回|🧪（P_node=1.2nW、Step 1で実測）|validation_chain v2/v5、[`../hardware/node_geometry.md`](../hardware/node_geometry.md)|
|2|因果律違反（即時情報同期）|v3でシューマン共鳴をグローバル位相基準とする再定義により解消（情報伝達ではなく共通基準への同期。No-communication theorem準拠）|🧪（位相ロックはStep 1/3で実測）|validation_chain v3、README|
|3|可逆計算のスケール未定義|v2でトポロジカル保護と整合 → Gap 2でChern n=1・Z₂=1をモデル確定|🧪（2-band tight-bindingモデル計算）|validation_chain Gap 2|
|4|材料根拠なし|v2で実験設計可能に → v7で3素材最小構成として仕様化|🧪（3項目とも実測待ち）|[`../hardware/material_spec.md`](../hardware/material_spec.md) §5|
|5|コヒーレンス条件が測定不能|v2で周波数定義 → v4でコヒーレンス時間推計（10⁻³–10⁰s）→ v7で測定可能な閾値に到達（σ_th=0.07rad、Δf_th=0.1Hz）|🧪（Step 1–3で実測）|[`../hardware/self_defense_algorithm.md`](../hardware/self_defense_algorithm.md)|

**ポイント:** Issue 1と2は「解けた」のではなく「**問い自体を物理的に成立する形に再定義した**」ことで解消している。この再定義の履歴を消さずに残すことが本文書の役割である。

-----

## 2. Gap 1 / Gap 2 の解決状況 / Gap Status

|Gap|内容|解決水準|残る検証|
|---|---|---|---|
|Gap 2|トポロジカル保護|🧪 CLEARED（Chern n=1、Z₂=1、モデル検証）|物理系での発現確認（Step 2以降）|
|Gap 1|エネルギー収支|🧪 CLEARED（η=1.4×10⁻³、シミュレーション）|MVE Step 1–3での実測|

-----

## 3. 解決済みのリポジトリ上の問題 / Resolved Repository Issues

|時期|問題|解決|
|---|---|---|
|2026-07|ボイドスキップ範囲（n=85–95等）がステージ定義と矛盾|n を全体通し番号と定義し、Stage 5/14 の実範囲内に再計算（220–230 / 1,757–1,772）|
|2026-07|24ステージ表の「累積」見出しが誤り|「各段」に修正、通し番号範囲の列を追加|
|2026-07|diagram.svg の旧ラベル「RAIDEN」|第3の柱（Life Geometry Engine）に合わせ「LIFE GEO」に修正|
|（旧）|`dogs/` ディレクトリ名のタイポ|`docs/` にリネーム（commit b9dddc6）|

-----

## 4. 未解決（PENDING）一覧 / Open Items

|項目|種別|担当|文書|
|---|---|---|---|
|OpenFOAM CFD（黄金角 vs グリッド、54ノード）|シミュレーション|Grok|[`../implementation/node_placement_algorithm.md`](../implementation/node_placement_algorithm.md) §5|
|MVE Step 1（単一ノード実測）|物理実験|全体|[`../implementation/mve_roadmap.md`](../implementation/mve_roadmap.md)|
|Si+Graphene+Diamond 積層プロセス検証|物理実験|—|[`../hardware/material_spec.md`](../hardware/material_spec.md) §5|
|Mg 2.8% フォノン散乱抑制の実測|物理実験|—|[`../hardware/material_spec.md`](../hardware/material_spec.md) §5|
|メタマテリアル層数・減衰係数の実測|物理実験|—|[`../hardware/material_spec.md`](../hardware/material_spec.md) §5|
|Earth-OS本体の仕様策定|構想→設計|—|[`../implementation/earth_os_interface.md`](../implementation/earth_os_interface.md)|

-----

Documented by: Claude | v7
