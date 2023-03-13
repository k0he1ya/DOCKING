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

    ```
    pdb_delhetatm.py PDB-L.pdb | pdb_chain.py | pdb_seg.py | pdb_fixinsert.py | pdb_tidy.py > PDB-L-clean.pdb
    ```
    
    ```
    pdb_delhetatm.py PDB-R.pdb | pdb_chain.py | pdb_seg.py | pdb_fixinsert.py | pdb_tidy.py > PDB-R-clean.pdb
    ```
    
2. active, passive残基の指定

    active残基は1行目、passive残基は2行目に記述する。
    
    **残基を指定しない場合は空スペースを入れる。そうしないとエラーが発生する**
    
    > `PDB-L.list`　（例）
    ```
    38 40 45 46 69 71 78 80 94 96 141
    （空白スペース）
    ```
    
    通常はエピトープに関する実験的情報がない場合、SASAをpassiveと定義する。`freesasa`でSASAを計算し、40%カットオフで残基をフィルタリングする。
    自分で任意にactive,passiveを指定しても良い。
    
    ```
    freesasa PDB-R-clean.pdb --format=rsa > PDB-R.rsa
    echo " " > PDB-R.list
    awk '{if (NF==13 && ($7>40 || $9>40)) printf "%s ",$3; if (NF==14 && ($8>40 || $10>40)) printf "%s ",$4}' PDB-R.rsa >>PDB-R.list
    ```
    > `PDB-R.list`　（例）
    ```
    15 16 17 20 48 49 51 52 54 56
    9 10 11 12 21 24 25 34 37 38 40 41 43 45 46 47 53 55 57 58 59 60 84 85
    ```

3. AIRファイルの定義

    ```
    active-passive-to-ambig.py PDB-L.list PDB-R.list >　antibody-antigen-ambig.tbl
    ```
 
    残基数が多い場合、計算が遅くなる可能性がある。
    この対策として、拘束をCA-CA原子間のみに限定し、拘束距離を3.0Åに微増する方法がある。

    ```
    active-passive-to-ambig.py PDB-L.list PDB-R.list | sed s/segid/name\ CA\ and\ segid/g | sed s/2.0/3.0/g >antibody-antigen-ambig.tbl
    ```


4. インプットファイルの作成


    > run.param
    ```
    AMBIG_TBL=antibody-antigen-ambig.tbl
    HADDOCK_DIR=PATH/TO/HADDOCK/INSTALLATIONDIR/haddock2.4
    N_COMP=2
    PDB_FILE1=PDB-L-clean.pdb
    PDB_FILE2=PDB-R-clean.pdb
    PROJECT_DIR=./
    PROT_SEGID_1=A
    PROT_SEGID_2=B
    RUN_NUMBER=1
    ```


5. Run

    プロジェクトディレクトリ内で以下のコマンドを実行。`run1`ディレクトリが作成される。

    ```
    haddock2.4
    ```
    
    `run1`ディレクトリ内に移動し、必要があれば　`run.cns`ファイルを編集する。（複合体構造の数を変更したい場合など）
    ```
    cd run1
    ```

    `run1`ディレクトリ内で以下のコマンドを再度実行し、ドッキング計算を開始する。
    ```
    haddock2.4
    ```
    
    バックグラウンドで実行したい場合は、以下を代わりに実行する。
    ```
    haddock2.4 >&haddock.out &
    ```

6. Analyse
