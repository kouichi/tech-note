#初期設定
##環境変数の設定

/sbin, /usr/sbin へ の PATH が通っていないので、 ~/.bash_profile の設定を変更します。

```
PATH=$PATH:$HOME/bin
↓
PATH=$PATH:$HOME/bin:/sbin:/usr/sbin
```

##yumリポジトリの追加

以下のポリシーでyumリポジトリの設定を行うことにします。

- 優先度をつけ、基本パッケージを優先する
- マイナーなyumリポジトリは、--enablerepoオプションを付けてインストールする

yumリポジトリの優先度を付けることができるパッケージ(yum-priorities)を事前にインストールしておきます。

```
$ sudo yum -y install yum-priorities

```

インストール完了後、/etc/yum.repos.d/CentOS-Base.repo 内の基本パッケージ(base, updates, addons, extras, centosplus, contribute)の優先度をそれぞれ設定します。以下の1行をそれぞれについて追記します。(数値が小さいほど優先度が高く、既定値は99になります)

```
priority=1
```

##rpmforgeリポジトリの追加

以下に用意されているrpmパッケージをインストールします。

- http://dag.wieers.com/rpm/packages/rpmforge-release/

現時点で、CentOS 6での最新版(0.5.2-2)をインストールします。

```
$ sudo rpm -ivh http://pkgs.repoforge.org/rpmforge-release/rpmforge-release-0.5.2-2.el6.rf.x86_64.rpm
```

##remiリポジトリの追加

以下に用意されているrpmパッケージをインストールします。

- http://rpms.famillecollet.com/

```
# rpm -ivh http://rpms.famillecollet.com/enterprise/remi-release-6.rpm
```

インストールが終わると /etc/yum.repos.d/rpmforge.repo というファイルが作成されます。

```
$ sudo cat /etc/yum.repos.d/rpmforge.repo
### Name: RPMforge RPM Repository for RHEL 6 - dag
### URL: http://rpmforge.net/
[rpmforge]
name = RHEL $releasever - RPMforge.net - dag
baseurl = http://apt.sw.be/redhat/el6/en/$basearch/rpmforge
mirrorlist = http://apt.sw.be/redhat/el6/en/mirrors-rpmforge
#mirrorlist = file:///etc/yum.repos.d/mirrors-rpmforge
enabled = 1
protect = 0
gpgkey = file:///etc/pki/rpm-gpg/RPM-GPG-KEY-rpmforge-dag
gpgcheck = 1

(以下略)
```

##優先度の設定

優先度もあわせて設定しておきます。以下の1行を追記します。

```
priority=10
```

##注意事項

dag と remi リポジトリでは、以下のように明示的に --enablerepoオプションを付ける必要があります。

```
$ sudo yum --enablerepo=dag,remi install xxxxx
$ sudo yum --enablerepo=dag,remi upgrade
$ sudo yum --enablerepo=dag,remi update
```
