# ドッキング結果（ディレクトリ名：DOCKING）

## ファイル構成の説明

    DOCKING
    └── UNBOUND-BOUND
                各クラスタリングごとアンサンブル構造が置いてあるディレクトリ
            ＊＊　5E7B-5E7Fはたしか single はエラーで生成できず
        ├── ensemble_average,centroid, ...
         
        ├──　その他ドッキング計算に必要な諸々のファイル
        
            3つのシナリオ
        ├── CDR-Surf
        ├── CDR-Epi9
        └── Real-interface
            ├── BOUND
            ├── UNBOUND
            ├── ENSEMBLE (ウォード法)
            └── ENSEMBLE_average, centroid, ...
                ├── ...
                └── run1
                    ├── ...
                    └── structures
                        ├── it0
                            └── ...
                        └── it1
                            ├── ...
                            └── water
                                ├── ...
                                └── analysis (ここに最終的なドッキング結果の構造が入っている。ドッキングスコアで昇順に並んでいる)
                                    └── ...
        
# ドッキング計算

## Requirements
- haddock2.4-2021-05
- CNS
- haddock-tools
- pdb-tools
- freesasa
- DockQ

## 前処理
## RUN
## Analyze
