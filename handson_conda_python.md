# ハンズオンConda仮想環境×Python×CUDA×JupyterLab

UbuntuサーバでGPUを利用した機械学習環境を整える作業の流れをまとめまし<br>
これやっておけば基礎的な機械学習のPython環境構築は完璧かも(?!)

## 前提

以下の作業が完了しているところから始めます
- [Ubuntuのインストール，セットアップ](ubuntu_install.md)
- [Ubuntuユーザの作成](ubuntu_adduser.md)
- [手元PCからUbuntuサーバへのSSH接続設定の有効化](ssh_activate.md)
- [Condaのインストール](conda_install.md)

また，接続先Ubuntuサーバの[**ユーザ名**，**ログインパスワード**](ubuntu_adduser.md)，[**IPアドレス**](check_ipaddress.md)を把握しておいてください

## 1. SSHでUbuntuサーバに接続
以下のコマンドでサーバに接続する
```shell
$ ssh ユーザ名@IPアドレス
```
ターミナルの表示に従いログインパスワードを入力する

## 2. Conda仮想環境を作成，有効化
ターミナルの左端の表記が```(base)```となっていることを確認する<br>
なっていなければ[Condaのインストール](conda_install.md)を再確認してね

1. 仮想環境作成

    以下のコマンドで仮想環境を作成する．ここでは環境名を```test_env```，Pythonバージョンを3.10系とする
    ```shell
    (base)$ conda create -n test_env python=3.10
    ```
    ライブラリのインストール確認表示が出た場合は```y```Enterで続行

2. 仮想環境に切り替え

    以下のコマンドで仮想環境に移動する
    ```shell
    (base)$ conda activate test_env
    ```
    ターミナルの左側の表記が```(環境名)```となっていることを確認<br>
    以下のコマンドでPythonが正しくインストールされていることを確認する
    ```shell
    (test_env)$ python -V
    ```
    Python3.10系が表示されれば成功


## 3. CUDAをインストール
AnacondaのCUDAのページ[https://anaconda.org/nvidia/cuda](https://anaconda.org/nvidia/cuda)を参照し，対象のCUDAバージョンのインストールコマンドをコピペして実行<br>今回は12.4をインストールする
```shell
(test_env)$ conda install nvidia/label/cuda-12.4.0::cuda
```
インストール終了後，以下の2点を確認
- 仮想環境内で```$ nvcc -V```を実行し，対象バージョンのCUDAがインストールされていることを確認
- 別の仮想環境内で```$ nvcc -V```を実行し，対象バージョンのCUDAがインストールされていないことを確認**（仮想環境を戻すのを忘れずに！！）**

## 4. JupyterLabをインストール，セットアップ

1. JupyterLabのインストール

    AnacondaのCUDAのページ[https://anaconda.org/conda-forge/jupyterlab](https://anaconda.org/conda-forge/jupyterlab)でコマンドを確認し，JupyterLabをインストールする
    ```shell
    (test_env)$ conda install conda-forge::jupyterlab
    ```

2. JupyterLabリモートアクセス設定

    **この設定は仮想環境間共通で適用されるため，初回のみ実行**<br>
    既に設定が済んでいる場合は***3. JupyterLabパスワード設定***に進む

    インストール完了後，リモートアクセスの設定を行う<br>
    以下のコマンドでデフォルトの設定ファイルを作成する
    ```shell
    (test_env)$ jupyter lab --generate-config
    ```
    ```.jupyter/jupyter_lab_config.py```を編集する<br>
    ```shell
    (test_env)$ nano ./jupyter/jupyter_lab_config.py
    ```

    アクセス設定を変更する．以下の2行を探し，編集する．以下ではnanoエディタを利用

    編集前
    ```shell
    # c.ServerApp.allow_origin = ''
    # c.ServerApp.ip = 'localhost'
    ```

    編集後（コメントアウトを外すのを忘れずに！！）
    ```shell
    c.ServerApp.allow_origin = '*'
    c.ServerApp.ip = '0.0.0.0'
    ```

3. JupyterLabパスワード設定

    以下のコマンドでパスワードを設定
    ```shell
    (test_env)$ jupyter lab password
    ```

## PyTorchのインストール
このハンズオンではCUDAセットアップの確認に機械学習ライブラリのPyTorchを使用する<br>
ただし**インストールサイズが大きく時間がかかる**ので，環境に不要な場合は[PyTorchのインストール](#PyTorchのインストール)と[Python，CUDAの動作確認](#Python，CUDAの動作確認)の項目はスキップして構わない

PyTorchのサイト[https://pytorch.org/get-started/locally/](https://pytorch.org/get-started/locally/)にアクセスし，環境を指定してコマンドを取得する．ハンズオンでは以下の設定でコマンドを取得する
- PyTorch Build： ```Stable(2.4.1)```
- Your OS: ```Linux```
- Package: ```Conda```
- Language: ```Python```
- Compute Platform: ```CUDA 12.4```

これらを設定すると，Run this Commandにコマンドが表示される．コピペして実行
```shell
(test_env)$ conda install pytorch torchvision torchaudio pytorch-cuda=12.4 -c pytorch -c nvidia
```

## JupyterLabを起動

1. 以下のコマンドを実行し，JupyterLabを起動する
    ```shell
    (test_env)$ jupyter lab
    ```

2. ターミナルに表示されるポート番号（数字4桁）を確認し，手元PCのブラウザで以下にアクセスする
    ```url
    http://サーバIPアドレス:ポート番号
    ```
    例：UbuntuサーバIPアドレスが```10.36.0.5```，ポート番号が```8888```の場合，```http://10.36.0.5:8888```にアクセス

3. [設定した](#JupyterLabパスワード設定)パスワードを手元PCのブラウザで入力し，JupyterLabに接続

## Python，CUDAの動作確認
1. JupyterLabでnotebookファイル（.ipynbファイル）を作成する
    1. ```File``` → ```New``` → ```Notebook```でファイルを作成
    2. Select Kernelでは```Python```または```Python3```を選択

2. PyTorchを使ってGPUの利用可否を表示する．以下のプログラムをセルに入力して実行
    ```python
    import torch

    print(torch.cuda.is_available())
    print(torch.cuda.get_device_name())
    ```
    実行ボタン▶️を押す

    3行目ではGPUの利用可否，4行目では対象となるGPUの名称を表示している<br>
    ```True```と正しいGPU名が出力された場合は成功