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
「setup a user ?」は、rootアカウント  
  


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
  











Windowsだとデフォルトではgitは入ってないので  
https://github.com/emscripten-core/emsdk  
githubの <>code のプルダウンメニューから Download.zipでダウンロードして、フォルダに解凍。後は手順通りにインストール。  
  
Windowsだとデフォルトではpythonは入ってないが、コマンドラインでpythonと打てば、microsoft storeのpythonまでガイドされ、インストール出来る。  
```
>emsdk.bat update
```
***
Updating the SDKのemsdk update部分で SSL: CERTIFICATE_VERIFY_FAILED で失敗する場合、  
（参考： https://www.webcyou.com/?p=10401 ）  
```
>pip install --upgrade certifi
>python
>>> import certifi
>>> certifi.where()　ここでcertifi/cacert.pem のパスが表示される（ただしバックスラッシュは二重表示）
>>> quit()
```
一旦コマンドラインに戻り、環境変数を設定する。
```
set SSL_CERT_FILE=表示されたcertifi/cacert.pemのパス（バックスラッシュ一重表記）
```
(どちらかと言えば、Windowsのシステムから恒久的に環境変数として登録した方が良さそう)  
***
```
>emsdk.bat install latest
>emsdk.bat activate latest
```
以上でインストール終わり。  

## 開発環境の起動
emsdkインストールフォルダにて
```
>emsdk_env.bat
```
を実行すると、環境変数などが設定される。  

## ローカルwebサーバのインストール
Freenginx( https://freenginx.org/en/ )  
ダウンロードページ( https://freenginx.org/en/download.html )  
Windows版をダウンロード、解凍して、適当な場所に置く。  
freenginx/nginx.exeを起動すると  
freenginx/html/ の中を  
http://localhost/ として見る事が出来るようになる。  
  
フルパス指定するのが面倒な場合は、index ofを表示させる。  
(参考： https://qiita.com/onokatio/items/4669b37644fe07d3aa80 )  
freenginx/conf/nginx.conf の server{} のあたりに次のように記述を追加。  
```
    server {
        autoindex on;
        ... 元の記述 ...
    }
```
freenginx/html/index.html を_index.htmlといったリネームなりで殺しておかないと、index of表示はされないので注意。  
  
作業しているフォルダのキャッシュを殺しておく  
（ただしこれでもブラウザ側で勝手にキャッシュする）  
```
    location / {
        ... 元の記述 ...
		proxy_no_cache 1;
		proxy_cache_bypass 1; 
    }
```
freenginxの再起動は以下で行う（要：管理者権限）
```
nginx -s reload
```
## SDL
基本的にSDL2を使用する事になると思うが、使用する場合  
```
emcc --show-ports
```
にて、使用できるportを確認して、必要なほど、以下のように追加する。  
```
emcc -o main.html main.cpp aaa.cpp bbb.cpp --use-port=sdl2 --use-port=sdl2_gfx --use-port=sdl2_ttf  
```
（これらのportを使う時にアップデートがかかるので、上記のSSL関連の作業が必須となる）  

パッド入力  
ブラウザ側の制約で自己証明書ではないhttpsサーバが必要  





## 課題
ファイル（リソース）読み込み  
サウンド  


