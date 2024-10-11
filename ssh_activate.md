# UbuntuサーバへのSSH接続を有効化

Ubuntuに対するSSH接続はデフォルトで実行できません．
以下の手順で，手元のPCからUbuntuサーバにSSH接続し操作できるようにします．

1. Ubuntuにopenssh-serverをインストールする
    ```shell
    $ sudo apt install openssh-server
    ```
2. 以下のコマンドでSSHのステータスを確認する
    ```shell
    $ sudo systemctl status ssh
    ```

    出力を見てサービスがactiveになっていることを確認
    ```shell
    ● ssh.service - OpenBSD Secure Shell server
    （略）
    Active: active (running)　．．．
    ```

3. activeでない場合は以下のコマンドを実行（未確認）
    ```shell
    $ sudo systemctl enable ssh
    $ sudo systemctl start ssh
    ```

<br><br>&copy; 2024_2020小野