# README

このREADMEにはドキュメントで使用している各種表記について書いておきます<br>
ドキュメントの閲覧・編集の参考にしてください

- コードブロックの表記<br>
このドキュメントでは実行するコマンドと出力結果を区別するため，`$`マークをコマンド冒頭につけています<br>
コマンドをコピペして実行する場合，`$`マークの後方部分をコピペしてください

    コマンド
    ```shell
    $ command --dayo
    ```
    出力
    ```shell
    output dayo
    ```

- コードブロックの表記（仮想環境）<br>
ハンズオン形式のドキュメントでは，Condaの仮想環境をターミナルの表記に倣って左端に表記します<br>
コマンドをコピペして実行する場合，`(環境名)$ `の後方部分をコピペしてください

    コマンド（```base```環境の場合）
    ```shell
    (base)$ command --dayo
    ```

- フッターの著者表記<br>
各Markdownファイルのフッター部分に著者を表記します．形式は以下の通り
    ```plane
    © 作成年度_入学年度名字
    ```

    フッター挿入時は以下のコードを流用してください

    ```html
    <br><br>&copy; 作成年_入学年度名字
    ```

    複数の著者がいる場合，半角カンマ`,`で接続して表記してください<br>
    例：&copy; 2024_2020小野，2025_2020大野

<br><br>&copy; 2024_2020小野
