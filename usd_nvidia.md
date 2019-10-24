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
    set PYTHONPATH=%USD_INSTALL_ROOT%\lib\python
    set PATH=%USD_INSTALL_ROOT%\bin;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\lib;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\deps\usdview-deps;%PATH%;
    set PATH=%USD_INSTALL_ROOT%\deps\embree;%PATH%;

    %windir%\system32\cmd.exe

展開場所が異なる場合は、USD_INSTALL_ROOTの記載を変更するようにしてください。    
Pythonへのパス、USDに必要なパス、usdviewで必要なパスを通して、コマンドプロンプトを起動します。    

    usdview C:\users\xxxx\yyyy.usda

のように指定すると、usdviewで第二引数（絶対パスで指定すること）でUSDファイル(usda/usdc/usdz)を表示するビュワーが起動します。    

### Embreeを使用する場合

CPUパストレーシングを行うEmbreeをusdview内で使用する場合、    
usd-win64_py27_releaseでは1つファイルが足りません。    
別途USDをビルドし、生成された「tbb_preview.dll」を「deps\usdview-deps」内に入れるようにします。    

USDのビルドについては「[USDのビルド (Win)](./usd_build_win.md)」をご参照くださいませ。    
