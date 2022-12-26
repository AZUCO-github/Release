# celadonをビルドする

記載時：2022/12/26  
結論：ビルドは出来たが恐らくTPMの関係でVMwareではboot出来ない  

## 何故android x86ではないのか

microsoft intune ポータルを使って、保護されたプロファイルを使用しようとする場合、現時点でのandroid x86では暗号化がサポートされておらず使う事が出来ない。  
このためandroid x86のintelによるブランチであるceladon（ https://projectceladon.github.io/celadon-documentation/index.html ）にチャレンジしてみた。  

## ビルドする

当時の「Build Celadon From Source」の内容は以下  
https://web.archive.org/web/20221226124253/https://projectceladon.github.io/celadon-documentation/getting-started/build-source.html  
  
（以下、内容の要約と注釈を書くので、原文を参照しながら見ること）  
コンパイルする環境はUbuntu 18.04との事なので、VMwareにインストール。  
ディスクサイズは400GB程度あれば足りるっぽい。  
  
「Set up the development environment」にて以降、手順が書かれているが、この手順を行う前に、ビルド時にスワップファイルが足りなくなるので（デフォルトで2GB程度しか確保されていないため）以下のコマンドを実行する。  

```
sudo fallocate -l 16G /swapfile2
sudo chmod 600 /swapfile2
sudo mkswap /swapfile2
sudo swapon /swapfile2
```

１を行う前に足りないものがあるので、以下のコマンドを実行する。  

```
sudo apt-get install curl
```

２を行った後に実際のビルドで足りないものがあるので、以下のコマンドを実行する。  

```
sudo apt install python-pip
pip install six
pip install pystache==0.5.4 --no-cache-dir
```

ビルドにはかなり時間がかかるので、気長に待とう(´・ω・`)  

## 動かす

ここでは32GB以下のフラッシュメモリ/SDカードを使って起動させる方法とする。  
UEFIにファイルを見せるために、FAT32でフォーマットする必要があるので、フラッシュメモリなどのサイズが32GB以上ある場合は、パーティションを32GBにして、フォーマットを選択するとFAT32の選択肢が選べる。  
「\<build directory\>/out/target/product/\<lunch target\>/」に「\<lunch target\>-flashfiles-eng.\<user name\>.zip」というファイルがあるので、これを解凍したものをフラッシュメモリなどにコピーする。  
  
VMwareでceladonのVMを作成する場合は、Windows11 64bitの環境で作る。  
必要なデバイスは、メモリ、プロセッサ、ハードディスク、USBコントローラ、ディスプレイ、Trusted Platform Module  
オプションではアクセスコントロールで暗号化を行い、VMとしてTPMを有効にする必要がある。  
  
この状態で起動して、フラッシュメモリを接続する。  
UEFIのshellに入れば、startup.nspが実行されて、ハードディスクにインストールが行われる。  
  
このあたりの参考になるのが  
https://www.youtube.com/watch?v=GbB7ktzdov0  
ただしこの動画の内容も、１年前で既に中身が変わっているので、本当に参考程度にしかならない。  
  
このようにしてHDDにインストールされ、一応起動しようとするのだが、TPMの関係なのが、最後まで起動する事は出来ない。  
Windows11でTPMが必須になった事も含め、androidとしてもセキュア化を進めようとしているのかもなので、今回は起動できなかったが、今後のチャレンジの時のメモとしてドキュメント化しておく。  
  
  
  
