#アカウント作成

```
通常の作業を行うアカウントとして以下のユーザを作成します。

- ユーザ: kouichi

また、作業を行うサーバは以下のホスト名とします。

- ホスト名: sakura-vps

上記のユーザ名およびホスト名は適宜読み替えて下さい。

初期状態では、ユーザは、`root`しか存在しないので、`root`でログインします。
```

まずはサーバへログインします。

```
$ ssh root@sakura-vps
```

ログイン後、普段利用するためのユーザ(`kouichi`)を作成しておきます。もちろん忘れずにパスワードも設定しておきます。

```
# useradd -u 500 -G wheel kouichi
# passwd kouichi
```

ユーザの属する補助グループに`wheel`を指定していますが、これは特定のグループに所属するユーザのみ`sudo`コマンドが実行できるようにするためです。

- 専用グループを作成し、そのグループに属させる方がよい。

##sudoの設定

`wheel`グループに所属するユーザのみ、`sudo`コマンドを許可するように設定しておきます。

```
# visudo
# コメントアウトされているのを外す
%wheel  ALL=(ALL)       ALL
```

- 本来は利用できるコマンドも制限をかけた方がよい。

##公開鍵でのsshログイン設定

VPSサーバへsshログインするマシン(Mac)にて、公開鍵/秘密鍵のペアを作成し、公開鍵をVPSサーバへscpにて転送しておきます。

```
$ ssh-keygen -t rsa -f ~/.ssh/id_rsa.sakura-vps
$ scp ~/.ssh/id_rsa.sakura-vps.pub kouichi@sakura-vps:~
```

VPSサーバへsshログインします。

```
$ ssh kouichi@sakura-vps
```

さきほど転送した公開鍵を`authorized_keys`に追記しておきます。(`authorized_keys`ファイルのパーミッションに注意して下さい)

```
$ mkdir ~/.ssh
$ chmod 700 ~/.ssh
$ cat ~/id_rsa.sakura-vps.pub >> ~/.ssh/authorized_keys
$ chmod 600 ~/.ssh/authorized_keys 
```

最後にMacにて、問題なくsshログインできること確認します。

```
$ ssh-add ~/.ssh/id_rsa.sakura-vps
$ ssh sakura-vps
```

問題なくsshログインできることが確認できたら、VPSサーバ上に一時的に置いた公開鍵を忘れずに削除しておきます。

```
$ rm ~/id_rsa.sakura-vps.pub
```

##sshdの設定変更

VPSサーバへのsshログインに制限をかけておきます。

まず、変更作業を行う前にバックアップを取っておきます。

```
$ cd /etc/ssh/
$ sudo cp sshd_config sshd_config.orig
```

引き続いて、`sshd_config`ファイルを編集します。(編集した部分のみ抜粋)

```
# パスワード認証を無効化し、公開鍵認証を有効化する
PasswordAuthentication no
PubkeyAuthentication yes
ChallengeResponseAuthentication no

# rootでの直接ログインは認めない
PermitRootLogin no
 
# PAM認証は使わない
UsePAM no
# リモートホスト名の確認は行わない
UseDNS no

# X11 Forwardingは使わない
X11Forwarding no

# 許可ユーザを制限
AllowUsers kouichi
```

最後にsshdを再起動します。

```
$ sudo service sshd restart
```

最後に忘れずにsshログインできることを確認して下さい。