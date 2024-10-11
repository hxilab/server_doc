# Condaをインストールする

仮想環境を作成しPythonをバージョンごとに管理できるCondaをインストールします．ここではデフォルトで数値計算系ライブラリを内包しないMinicondaをインストールします．

1. インストーラファイルをダウンロードする<br>[https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh](https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh)
    ```shell
    $ wget https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
    ```
2. インストールを実行
    ```shell
    $ bash Miniconda3-latest-Linux-x86_64.sh
    ```
3. コンソールの表示に従い，利用規約に同意しインストール先を指定
4. Condaの自動起動の質問に対して```yes```と入力してEnter
5. ターミナルを再度開き，左端にデフォルトの仮想環境```(base)```が有効になっていることを確認
6. Condaの自動起動は以下のコマンドで設定できる（true/false）
    ```shell
    conda config --set auto_activate_base true
    ```