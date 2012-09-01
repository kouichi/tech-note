
#さくらインターネット VPS セットアップ


##事前準備

- OSの情報確認

```
# cat /etc/redhat-release
CentOS release 6.2 (Final)
```

- ネットワークの情報確認

##作業一覧

セットアップ手順を以下にまとめる。

###初期設定

1. アカウント作成
2. sudo設定
3. sshd設定
4. Mac(Client) のssh設定
5. タイムゾーンの設定
6. 環境変数の設定
7. yumリポジトリの追加
	- rpmforge
	- DAG
	- EPEL
	- remi

###iptables

1. iptables設定
2. hosts.allow設定


###Postfix

1. Postfix設定

###Ruby

1. rvmのインストールと設定
2. Rubyインストール
3. gemインストール


###nginx

1. nginxのインストールと設定
2. php-fpmのインストールと設定

###PHP

###MySQL 

###その他

1. cron
2. logrotate
3. 
