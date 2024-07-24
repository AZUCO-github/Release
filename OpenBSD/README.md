# OpenBSD を とりあえず使ってみる

記載時：2024/07/24  
  
https://www.openbsd.org/  

## VMwareにインストール
isoのダウンロード  
https://www.openbsd.org/faq/faq4.html#Download  
install??.isoをダウンロードする。  
  
VMwareで仮想マシンをセットアップする。  
構成のタイプは標準で良い。  
インストーラディスクイメージはダウンロードしたものを指定。  
ゲストOSの選択は、「その他」の「他の64bit」か「その他」を選択。  
（ダウンロードしたisoに合わせる）  
仮想マシン名は適当に「OpenBSD」とかで良い。  
ディスクの最大サイズは、どの程度が良いかわからないのだが40GB程度だと問題無さそうだった。  
（pkgでソフトを入れる場合は、/usr/の容量が求められるため）  
  
メモリやCPUもホスト側が苦しくない範囲で適当に割り当てれば良い。  
（8GB、4coreくらい？）  
  
仮想マシンをパワーオンする  

## OpenBSDのインストール
以下手順は記載時のOpenBSD 7.5のもの  
  
「Install / Upgrade / Autoinstall / Shell」は、Iを入力。  
「choose your keyboard layout ?」は、jpを入力。  
「system hostname ?」は、適当にopenbsdあたりを入力。  
「network interface to configure ?」は、デフォルトで良いのでそのままenter。  
「IPv4 address for ...」は、デフォルトで良いのでそのままenter。  
「IPv6 address for ...」は、最近はデフォルトっぽいので、autoconfを入力  
「network interface to configure ?」は、デフォルトで良いのでそのままenter。  
  
「password for root account ?」は、administoratorとなるユーザrootのパスワードなので、適当に決めてメモしておく。  
（パスワードは確認のために2回訊かれる）  
「start sshd by default ?」は、、デフォルトで良いのでそのままenter。  
「do you expect to run the x window system ?」は、とりあえず使ってみる勢には必要なので、そのままenter。  
「do you want the x window system to be started by xenodm ?」は、GUIでログインするならyを入力。  
  
「setup a user ?」は、作成するユーザIDを入力する。  
（ユーザアカウントが無いと、この後の色々セットアップで支障が出るので）  
「full name for user ?」は、適当に入れてenter。  
「password for user ?」は、適当に決めてメモしておく。  
（パスワードは確認のために2回訊かれる）  
  
「allow root ssh login ?」は、特に必要無ければデフォルトのnoで良いので、そのままenter。  
「what timezone are you in ?」は、恐らくデフォルトでasia/tokyoになってると思うので、そのままenter。  
「which disk is the root disk ?」は、恐らくディスクは一つだと思うので、そのままenter。  
「encrypt the root disk with a passphrase or keydisk ?」は、デフォルトのnoで良いので、そのままenter。  
「use whole disk MBR. whole disk GPT or Edit ?」は、デフォルトで良いので、そのままenter。  
「use auto layout. edit auto layout. or create custom layout ?」は、前述の「/usr/の容量が欲しい」があるので、eを入力。  
  
パーティションレイアウトのエディットプロンプトが出てくるので、とりあえずヘルプの?を入力。  
小文字のpが「print partition」でパーティションのリストが表示されるので、pを入力。  
「/usr/」の容量が欲しいので、「/usr/*」以下のフォルダとして指定されているパーティションを削除する必要がある。  
（この例では、g-jが該当する）  
  
小文字のdが「delete partition」でパーティションを削除できるので、dを入力して削除する。  
d g  
d h  
d i  
d j  
  
削除し終わったので、再度pを入力してパーティションのリストを確認する。  
容量を増やす対象のパーティション「/usr」は、この例ではfが該当。  
  
小文字のcが「change partition size」でパーティションのサイズを変更できるので、c fを入力して変更する。  
「partition f is ...（中略）... maximum size of mmmmm sectors」と出てくるので、  
「size: [ccccc] 」のプロンプトに対して、maximum sizeとして表示されたmmmmmを入力する。  
  
小文字のqが「quit & save changes」なので、qを入力して、パーティションエディットを終了する。  
終了の際に「write new label ?」と訊かれるので、デフォルトで良いので、そのままenter。  
  
「location of sets ?」は、デフォルトで良いと思われるので、そのままenter。  
「pathname to the sets ?」も、デフォルトで良いと思われるので、そのままenter。  
「set name ? or abort or done」も、デフォルトで良いと思われるので、そのままenter。  
「directry ...（中略）... verification ?」は、進行したいのでyを入力。  
（この後、色々書き込みだすので、しばらく待つ）  
  
「location of sets ?」は、デフォルトで良いので、そのままenter。  
「time appears wrong. set to ...（後略）」は、デフォルトで良いので、そのままenter。  
（この後、色々動き出すので、しばらく待つ）  
  
「exit to shell,halt or reboot ?」は、再起動なので、そのままenter。  
（この後、再起動するので、しばらく待つ）  
  
## OpenBSDの色々セットアップ
xenodmを有効にしたので、GUIログインが起動していると思うので、  
rootアカウントではなく、ユーザアカウントでログインする。  
以下、いずれもコンソールでコマンドなどを入力していく。  

### super userに切り替える
```
su -
```
（rootのパスワードを訊かれるので入れる）  

### pkgのアップデート
```
pkg_add -u
```
### doasをインストール
```
pkg_add doas
```
### /etc/doas.confを設定
```
vi /etc/doas.conf
```
viは初期状態から入ってる模様。  
viで/etc/doas.confに書く内容。  
```
permit user as root
```
viで編集した内容を保存して抜ける時は、[esc]キーを押して、:qwを入力する。  
### super userから抜ける（userに戻る）
```
exit
```
### unzipのインストール
```
doas pkg_add unzip
```
（userのパスワードを訊かれるので入れる）  
「choose package for unzip」は、とりあえず1を選択。  
### ブラウザのchromiumのインストール
```
doas pkg_add chromium
```
（userのパスワードを訊かれるので入れる）  
### ブラウザのchromiumの起動
```
chrome
```
（chromium & ではないので注意）  
### 日本語フォント IPAフォントのインストール
起動したchromeで以下にアクセス  
https://moji.or.jp/ipafont/ipa00303/  
下の方の「IPAfont?????.zip」をダウンロードする。  
（?????は最新のバージョンに合わせて読み替え）  
ダウンロードが終わったらフォントをインストールする。  
```
unzip -x ~/Downloads/IPAfont?????.zip
mkdir ~/.fonts
mv IPAfont?????/* ~/.fonts/
(cd ~/.fonts; mkfontdir; mkfontscale)
rm -rf IPAfont?????
```
### ファイルマネージャのThunarのインストール
```
doas pkg_add thunar
```

















