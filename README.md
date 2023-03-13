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

## Run
1. PDBの前処理
    `pdb_delhetatm.py PDB.pdb | pdb_chain.py | pdb_seg.py | pdb_fixinsert.py | pdb_tidy.py > PDB-clean.pdb`
3. SASAの計算
    `freesasa PDB-clean.pdb --format=rsa > PDB.rsa`
5. active, passive残基の指定
    
7. AIRファイルの定義
8. インプットデータの定義
9. Run

## Analyze
