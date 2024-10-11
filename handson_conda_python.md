# ハンズオンConda仮想環境×Python×CUDA×JupyterLab

UbuntuサーバでGPUを利用した機械学習環境を整える作業の流れをまとめました．<br>これやっておけば基礎的な環境構築は完璧かも(?!)

## 前提

以下の作業が完了しているところから始めます
- Ubuntuのインストール，セットアップ
- Ubuntuユーザの作成
- [手元PCからUbuntuサーバへのSSH接続設定の有効化](ssh_activate.md)
- [Condaのインストール](conda_install.md)

また，接続先Ubuntuサーバの**ユーザ名**，**ログインパスワード**，**IPアドレス**を把握しておいてください

## 1. SSHでUbuntuサーバに接続
以下のコマンドでサーバに接続する．
```shell
$ ssh ユーザ名@IPアドレス
```
ターミナルの表示に従いパスワードを入力する．

## 2. Conda仮想環境を作成，有効化
ターミナルの左端の表記が```(base)```となっていることを確認する<br>
なっていなければ[Condaのインストール](conda_install.md)を再確認してね

以下のコマンドで仮想環境を作成する．ここでは環境名を```test_env```，Pythonバージョンを3.10系とする
```shell
$ conda create -n test_env python=3.10
```
ライブラリのインストール確認表示が出た場合は```y```Enterで続行

以下のコマンドで仮想環境に切り替え
```shell
$ conda activate test_env
```

## 3. CUDAをインストール
AnacondaのCUDAのページ[https://anaconda.org/nvidia/cuda](https://anaconda.org/nvidia/cuda)を参照し，対象のCUDAバージョンのインストールコマンドをコピペして実行する．今回は12.4をインストールする
```shell
$ conda install nvidia/label/cuda-12.4.0::cuda
```
インストール終了後，以下の2点を確認
- 仮想環境内で```$ nvcc -V```を実行し，対象バージョンのCUDAがインストールされていることを確認
- 別の仮想環境内で```$ nvcc -V```を実行し，対象バージョンのCUDAがインストールされていないことを確認**（仮想環境を戻すのを忘れずに！！）**

## 4. JupyterLabをインストール，セットアップ
AnacondaのCUDAのページ[https://anaconda.org/conda-forge/jupyterlab](https://anaconda.org/conda-forge/jupyterlab)でコマンドを確認し，JupyterLabをインストールする
```shell
$ conda install conda-forge::jupyterlab
```

### JupyterLabリモートアクセス設定
**この設定は仮想環境間共通で適用されるため，初回のみ実行**<br>
既に設定が済んでいる場合は[パスワード設定](#JupyterLabパスワード設定)に進む

インストール完了後，リモートアクセスの設定を行う<br>
以下のコマンドでデフォルトの設定ファイルを作成する
```shell
$ jupyter lab --generate-config
```
```.jupyter/jupyter_lab_config.py```を編集する<br>
```shell
$nano ./jupyter/jupyter_lab_config.py
```
以下の2行を探し，編集する

編集前
```shell
# c.ServerApp.allow_origin = ''
# c.ServerApp.ip = 'localhost'
```

編集後
```shell
c.ServerApp.allow_origin = '*'
c.ServerApp.ip = '0.0.0.0'
```

## JupyterLabパスワード設定
以下のコマンド

