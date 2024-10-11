# SSHでUbuntuサーバへ接続

手元のPCからUbuntuサーバへ接続し，遠隔での操作を可能とします．

0. 事前準備１：UbuntuサーバのSSH設定を有効化しておく．[参考：UbuntuサーバへのSSH接続を有効化](ssh_activate.md)<br>事前準備２：手元のPCとUbuntuサーバが同一のネットワークに接続されていることを確認しておく<br>事前準備３：接続先UbuntuサーバのIPアドレスを確認しておく<br>
コマンド```$ ifconfig```

1. 接続先Ubuntuサーバのユーザ名とログインパスワードを確認し，以下のコマンドを実行する

    ```shell
    $ ssh ユーザ名@接続先UbuntuのIPアドレス
    ```

    例：ユーザ名```ono```，IPアドレス```10.36.0.5```である場合

    ```shell
    $ ssh ono@10.36.0.5
    ```
2. 手元PCでの接続が初回である場合，```Are you sure you want to continue connecting (yes/no)?```と訊かれる．```yes```で続行

2. ログインパスワードを入力
3. ```ユーザ名@サーバマシン名:~$```と表示されたら接続成功