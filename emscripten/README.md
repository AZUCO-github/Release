# EMScripten

記載時：2024/07/02  
結論：作業中  

https://emscripten.org/  

## Windows環境でのインストール手順の注釈

公式のダウンロード＆インストールガイド  
https://emscripten.org/docs/getting_started/downloads.html  
  
Windowsだとデフォルトではgitは入ってないので  
https://github.com/emscripten-core/emsdk  
githubの <>code のプルダウンメニューから Download.zipでダウンロードして、フォルダに解凍  
後は手順通りにインストール  
  
Windowsだとデフォルトではpythonは入ってないが  
コマンドラインでpythonと打てば、microsoft storeのpythonまでガイドされ、インストール出来る  

```
>python
>emsdk install latest
>emsdk activate latest
>emsdk update
```
Updating the SDKのemsdk update部分で  
SSL: CERTIFICATE_VERIFY_FAILEDで失敗する場合、  
（参考： https://www.webcyou.com/?p=10401 ）  
```
>pip install --upgrade certifi
>python
>>> import certifi
>>> certifi.where()　ここでcertifi/cacert.pem のパスが表示される（ただしバックスラッシュは二重表示）
>>> quit()
```
一旦コマンドラインに戻り、環境変数を設定する  
```
set SSL_CERT_FILE=表示されたcertifi/cacert.pemのパス（バックスラッシュ一重表記）
>emsdk update
```
以上でインストール終わり  

## 開発環境の起動
emsdkインストールフォルダにて
```
>emsdk_env
```
を実行すると、環境変数などが設定される  
（ただし永続ではない）  



## 課題
マルチスレッド  
SDL描画  
リアルタイムフレーム管理（タイマ？）  
ファイル（リソース）読み込み  
パッド入力  
サウンド  



