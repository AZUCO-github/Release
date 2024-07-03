# EMScripten

記載時：2024/07/02  
結論：作業中  

https://emscripten.org/  

## Windows環境でのインストール手順の注釈
公式のダウンロード＆インストールガイド  
https://emscripten.org/docs/getting_started/downloads.html  
  
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
      ... 以下、元の記述 ...
```
freenginxの再起動は以下で行う。
```
nginx -s reload
```



## 課題
マルチスレッド  
SDL描画  
リアルタイムフレーム管理（タイマ？）  
ファイル（リソース）読み込み  
パッド入力  
サウンド  
https://qiita.com/ytanto/items/258d567314cb4ad07457  
https://www.slideshare.net/slideshow/cmu29/49627641  



