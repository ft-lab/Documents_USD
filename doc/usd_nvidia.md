# NVIDIAのビルドされたライブラリを使う

USDをソースからビルドしてインストールする場合は「[USDのビルド (Win)](./usd_build_win.md)」をご参照くださいませ。     

NVIDIAのサイトにて、USDのビルド済みのライブラリ(USD Pre-built Libraries and Tools)をダウンロードできます。     

https://developer.nvidia.com/usd     

このビルドは、Pythonを使用するビルドになります。    
C++のみのライブラリとして使用する場合には向きません。    
Python一式とUSDのコマンドラインツールも含まれています。    

2021年12月段階では「USD 21.05, Python 3.6」がダウンロードできます。     

## インストール手順 (Win)

「usd-21-05-usd-win64_py36_release.7z」をダウンロードし、"C:\WinApp\usd-21-05-usd-win64_py36_release"に展開したとします。    
これにはPython36も含まれていないため、別途Pythonをインストールしておく必要があります。    
まだインストールしていない場合は https://pythonlinks.python.jp/ja/index.html より、Python 3.6の最新版をダウンロードしてインストールするようにしてください。       
「C:\Python36」にPython36をインストール済みとします。      
また、Pythonには「pyside2」「PyOpenGL」をpipでインストールしておく必要があります。     

### コマンドプロンプトでPATHを通す

コマンドプロンプトで以下を実行します。     
```
set PATH=C:\Python36;C:\Python36\Scripts;%PATH%
```
これで、Pythonへのパスが通りました。     

「python --version」と入力して、「Python 3.6.8」のように表示されるのを確認します。     

### pipでpyside2をインストール

コマンドプロンプトで以下を実行します。     
```
pip install pyside2
```

### PyOpenGLをインストール

コマンドプロンプトで以下を実行します。     
```
pip install PyOpenGL
pip install PyOpenGL_accelerate
```

これで実行環境のインストールが完了しました。      

## 使い方 (Win)

### 起動用のバッチを作成

以下のようなバッチファイル(*.bat)を作成します。    
これで、必要なPATHを通します。    

```
set PATH=C:\Python36;C:\Python36\Scripts;%PATH%
set USD_INSTALL_ROOT=C:\WinApp\usd-21-05-usd-win64_py36_release
set PYTHONPATH=%USD_INSTALL_ROOT%\lib\python;C:\Python36\Lib\site-packages
set PATH=%USD_INSTALL_ROOT%\bin;%PATH%;
set PATH=%USD_INSTALL_ROOT%\lib;%PATH%;

%windir%\system32\cmd.exe
```

展開場所が異なる場合は、USD_INSTALL_ROOTとPythonのインストールパス「C:\Python36」の記載を変更するようにしてください。    
Pythonへのパス、USDに必要なパス、usdviewで必要なパスを通して、コマンドプロンプトを起動します。    

### usdviewを実行

上記のバッチでコマンドプロンプトを起動後、以下のようにusdviewを起動しました。     

```
usdview yyyy.usda
```

のように指定すると、usdviewで第二引数で指定したUSDファイル(usda/usdc/usdz)を表示するビュワーが起動します。    

