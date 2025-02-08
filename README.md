# streamlit-01
streamlit の検証

## aws ec2 で　サービスとして起動させる方法
https://qiita.com/take314/items/51c58859dde262916b47

### 自動復旧させる

万が一アプリケーションのプロセスが落ちても自動復旧できるよう、systemdを利用した自動復旧の仕組みを構築します。
まずアプリケーション実行用のスクリプトrun.shをmy-appディレクトリ配下に作成し、chmod 755 run.shしておきます。

```bash:run.sh
#!/bin/sh
~/.local/bin/streamlit run main.py  # systemdから実行するため、streamlitの実行ファイルはフルパスで指定
```

次に、run.shをシステムのデーモンから実行できるようにするため、  
/etc/systemd/system/配下にmy-app.serviceファイルを作成します。

```ini
[Unit]
Description=Run my-app
After=network.target

[Service]
WorkingDirectory=/home/ec2-user/my-app
ExecStart=/home/ec2-user/my-app/run.sh
Restart=always
User=ec2-user

[Install]
WantedBy=multi-user.target
```
