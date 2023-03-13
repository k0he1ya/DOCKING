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

    `pdb_delhetatm.py PDB-L.pdb | pdb_chain.py | pdb_seg.py | pdb_fixinsert.py | pdb_tidy.py > PDB-L-clean.pdb`
    
    `pdb_delhetatm.py PDB-R.pdb | pdb_chain.py | pdb_seg.py | pdb_fixinsert.py | pdb_tidy.py > PDB-R-clean.pdb`

2. SASAの計算

    `freesasa PDB-L-clean.pdb --format=rsa > PDB-L.rsa`
    
    `freesasa PDB-R-clean.pdb --format=rsa > PDB-R.rsa`

3. active, passive残基の指定
    active残基は1行目、passive残基は2行目に記述する。
    **残基を指定しない場合は空スペースを入れる。そうしないとエラーが発生する**
    
    > PDB-L.list
    ```
    38 40 45 46 69 71 78 80 94 96 141
     
    ```
    
    > PDB-R.list
    ```
    15 16 17 20 48 49 51 52 54 56
    9 10 11 12 21 24 25 34 37 38 40 41 43 45 46 47 53 55 57 58 59 60 84 85
    ```

4. AIRファイルの定義


5. インプットデータの定義


6. Run

## Analyze
