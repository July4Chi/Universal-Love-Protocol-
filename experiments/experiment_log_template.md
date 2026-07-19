# ULP 実験記録テンプレート / Experiment Log Template

> 記録様式 v1 — MVE Step 1 / 2 / 2.5 / 3 共通  
> 1実験 = 1ファイル。命名規則: `YYYY-MM-DD_stepX_<短い説明>.md`  
> このファイルをコピーして `experiments/` 直下に保存する。

-----

## 基本情報 / Basic Info

```
日付:
実施者:
MVEステップ:     Step 1 / 2 / 2.5 / 3
対照条件:        C0 / C1 / C2 / C3（Step 1）、黄金角 / グリッド（Step 2）
```

## 実験条件 / Conditions

```
ノード径 d:       [mm]
流速 v:           [mm/s]
水温:             [°C]
Reynolds数 Re:
ノード数:
配置パラメータ:    c₀ =        α =        N_total =
```

## 機材 / Equipment

```
トランスデューサ:  （圧電 / 電磁、型番）
LNA:              （NF、ゲイン）
ロックインアンプ:  （型番、時定数、参照周波数）
PLL:              （基準周波数、capture range）
環境:             （シールド、防振、冷却温度）
```

## 結果 / Results

```
P実測:            [nW]
S/N比:            [dB]
位相ロック:        成功 / 失敗（7.83Hz ± ___ Hz）
位相ジッター σ:    [rad]（σ_th = 0.07 rad との比較）
サンプル数:
生データ参照:      （ファイルパス / 外部リンク）
```

## 判定 / Verdict

```
成功判定基準:      （mve_roadmap.md の該当Stepの基準を転記）
判定:             PASS / FAIL / 部分的PASS
ラベル昇格:        🧪 → ✅ に昇格する値（あれば）:
```

## 考察・次アクション / Notes & Next Actions

```
（FAIL時: mve_roadmap.md「失敗時の分岐」のどの枝に該当するか、次に何を変えるか）
```

-----

関連: [`../implementation/mve_roadmap.md`](../implementation/mve_roadmap.md)
