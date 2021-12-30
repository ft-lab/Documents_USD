# USDのライブラリを使ったプログラムを実行 (Win)

USDのビルドについては「[USDのビルド (Win)](../doc/usd_build_win.md) 」をご参照くださいませ。    

実行前にOSの環境変数にパスを通す必要があります。    
起動時にバッチ化しておくのがいいかもしれません。    

## Pythonを使用したビルドを行った場合

Pythonを使用したビルドを行った場合、実行前に以下のパス指定が必要になります。    
コマンドラインのUSDツールセット([usdcat](../doc/toolset/usd_toolset_usdcat.md) / [usdview](../doc/toolset/usd_toolset_usdview.md) / [usdzip](../doc/toolset/usd_toolset_usdzip.md) など)の機能を実行する場合は、このビルドを使用する必要があります。    

### Python 2.7の場合

```
set PATH=C:\Python27;C:\Python27\Scripts;%PATH%    
set PYTHONPATH=C:\WinApp\USD\builds\lib\python  
set PATH=C:\WinApp\USD\builds\bin;%PATH%;    
set PATH=C:\WinApp\USD\builds\lib;%PATH%;  
```
Python27を「C:\Python27」をインストール、USDのビルドを「C:\WinApp\USD\builds」に格納したとした記載になります。    

### Python 3.9の場合

```
set PATH=C:\Python39;C:\Python39\Scripts;%PATH%
set PYTHONPATH=C:\WinApp\USD\builds\lib\python;C:\Python39\Lib\site-packages    
set PATH=C:\WinApp\USD\builds\bin;%PATH%;    
set PATH=C:\WinApp\USD\builds\lib;%PATH%;    
```

Python39を「C:\Python39」をインストール、USDのビルドを「C:\WinApp\USD\builds」に格納したとした記載になります。    
Python39の場合は「PYTHONPATH」で「C:\Python39\Lib\site-packages」を追加する必要があります。     

## Pythonを使用しないビルドを行った場合

build_usd.pyで--no-python を指定してビルドした場合は、実行前に以下のパス指定が必要になります。    

    set PATH=C:\WinApp\USD\builds_no_python\bin;%PATH%;    
    set PATH=C:\WinApp\USD\builds_no_python\lib;%PATH%;  

USDのビルドを「C:\WinApp\USD\builds_no_python」に格納したとした記載になります。    
この場合は、Pythonは使用しません。    

