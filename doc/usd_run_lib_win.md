# USDのライブラリを使ったプログラムを実行 (Win)

USDのビルドについては「[USDのビルド (Win)](../doc/usd_build_win.md) 」をご参照くださいませ。    

実行前にOSの環境変数にパスを通す必要があります。    
起動時にバッチ化しておくのがいいかもしれません。    

## Pythonを使用したビルドを行った場合

Pythonを使用したビルドを行った場合、実行前に以下のパス指定が必要になります。    

    set PATH=C:\Python27;C:\Python27\Scripts;%PATH%    
    set PYTHONPATH=C:\WinApp\USD\builds\lib\python  
    set PATH=C:\WinApp\USD\builds\bin;%PATH%;    
    set PATH=C:\WinApp\USD\builds\lib;%PATH%;  

Python27を「C:\Python27」をインストール、USDのビルドを「C:\WinApp\USD\builds」に格納したとした記載になります。    

## Pythonを使用しないビルドを行った場合

build_usd.pyで--no-python を指定してビルドした場合は、実行前に以下のパス指定が必要になります。    

    set PATH=C:\WinApp\USD\builds_no_python\bin;%PATH%;    
    set PATH=C:\WinApp\USD\builds_no_python\lib;%PATH%;  

USDのビルドを「C:\WinApp\USD\builds_no_python」に格納したとした記載になります。    
この場合は、Pythonは使用しません。    

