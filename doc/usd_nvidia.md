# NVIDIAのビルドされたライブラリを使う

NVIDIAのサイトにて、USDのビルド済みのライブラリ(USD Pre-built Libraries and Tools)をダウンロードできます。     

https://developer.nvidia.com/usd     

このビルドは、Pythonを使用するビルドになります。    
C++のみのライブラリとして使用する場合には向きません。    
Python一式とUSDのコマンドラインツールも含まれています。    

## 使い方 (Win)

zipファイルをダウンロードし、"C:\WinApp\usd-win64_py27_release"に展開したとします。    
これにはPython27も含まれています。    

以下のようなバッチファイル(*.bat)を作成します。    
これで、必要なPATHを通します。    

    set USD_INSTALL_ROOT=C:\WinApp\usd-win64_py27_release
    set PATH=%USD_INSTALL_ROOT%\deps\python;%USD_INSTALL_ROOT%\deps\python\Scripts;%PATH%
    set PYTHONPATH=%USD_INSTALL_ROOT%\lib\python;%USD_INSTALL_ROOT%\deps\usdview-deps-python
    set PATH=%USD_INSTALL_ROOT%\bin;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\lib;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\deps\usdview-deps;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\deps\embree;%PATH%;

    %windir%\system32\cmd.exe

展開場所が異なる場合は、USD_INSTALL_ROOTの記載を変更するようにしてください。    
Pythonへのパス、USDに必要なパス、usdviewで必要なパスを通して、コマンドプロンプトを起動します。    

    usdview C:\users\xxxx\yyyy.usda

のように指定すると、usdviewで第二引数（絶対パスで指定すること。相対パス指定は後述）でUSDファイル(usda/usdc/usdz)を表示するビュワーが起動します。    

### usdview/usdcat/usdzip使用時に、相対パスで指定したい

"C:\WinApp\usd-win64_py27_release"に展開している場合、
"bin\usdview.cmd"、"bin\usdcat.cmd"、"bin\usdzip.cmd"、などを開き、最後の行以外をコメントにします。     
remでその行がコメントとなります。    
もしくは最後の行以外を削除します。    

    rem @echo off
    rem setlocal
    rem pushd %~dp0
    
    rem set DEPS=%~dp0..\deps
    rem set LIBP=%~dp0..\lib
    rem set PYP=%~dp0..\lib\python
    
    rem set EMBREE_DEPS=%DEPS%\embree
    rem set PYTHON_DEPS=%DEPS%\python
    rem set USDVIEW_DEPS=%DEPS%\usdview-deps
    rem set USDVIEW_PYTHON_DEPS=%DEPS%\usdview-deps-python
    
    rem set PYTHONPATH=%PYP%;%USDVIEW_PYTHON_DEPS%
    
    rem set PATH=%PYTHON_DEPS%;%LIBP%;%USDVIEW_DEPS%;%EMBREE_DEPS%
    
    @python "%~dp0usdview" %*

これで、1つ前のバッチ(PATH/PYTHONPATHの指定があるもの)を実行後、    
USDファイルのあるフォルダにコマンドラインで移動し、    

    usdview yyyy.usda

のように指定してUSDのコマンドを実行できます。    


### Embreeを使用する場合

CPUパストレーシングを行うEmbreeをusdview内で使用する場合、    
usd-win64_py27_releaseでは1つファイルが足りません。    
別途USDをビルドし、生成された「tbb_preview.dll」を「deps\usdview-deps」内に入れるようにします。    

USDのビルドについては「[USDのビルド (Win)](../doc/usd_build_win.md)」をご参照くださいませ。    
