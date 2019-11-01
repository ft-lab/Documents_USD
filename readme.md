# USDについて

PixarのUSD ( Universal Scene Description ) の覚書きです。    
USD 19.07 段階の記載です。    

## USDとは ?

公式サイト : https://graphics.pixar.com/usd/docs/index.html    

USD ( Universal Scene Description )は、3Dのシーンデータを受け渡しをする際のパイプラインになります。    
USDフォーマットを軸にしていますが、    
大規模シーンを管理する際のDCCツールに依存しない中間的な環境を提供する、というほうが適切かもしれません。    

## USDが使われる個所

* DCCツール間のシーンデータの受け渡しと管理
* Mac環境、iPhoneのスマートフォンやiPadのタブレットのARの形状ファイルとしての使用

ここでは、主に後者について説明することにします。    
ARとしてのUSDの使用は、iOS12で搭載されiOS13/iPadOS13でかなり改善されています。    

## USDのビルド環境/使える機能

USDはオープンソースプロジェクト ( Modified Apache 2.0 License ) のため、誰でも無料で使用できます。    
GitHub : https://github.com/PixarAnimationStudios/USD    

以下の特徴があります。     

* Linux/MacOS/Windowsに対応
* USDファイルを入出力するC++ライブラリが存在
* USDファイルを入出力するPythonラッパーが存在
* コマンドラインの「USDツールセット」 (Python必要)    
以下、一部抜粋です。    
   * [usdcat](./doc/toolset/usd_toolset_usdcat.md) : コマンドラインでUSDファイルをバイナリ/テキスト形式で相互変換    
   * [usdview](./doc/toolset/usd_toolset_usdview.md) : USDファイルの3Dビュワー    
   * [usdzip](./doc/toolset/usd_toolset_usdzip.md) : USDファイルとテクスチャリソースを1つのusdz形式に変換    

デフォルトではPython経由でUSDのツール類にアクセスしたり、独自アプリに組み込めます。     
別途、C++ライブラリのみでの使用でPythonに依存しないようにもできます。   
その場合は、一部のUSDツールは使用できません（ライブラリを使ってアプリに組み込む機能はすべて使えます）。

## USDの特徴

これはUSDフォーマットとしての仕様になります。    
usdviewでのビュワー表示、iOS12/iOS13でのAR、など環境によって対応されている内容が異なります。   

* 3Dシーン/3DモデルをUSDファイルとして管理
* シーンを構成するUSDファイルは、バイナリ形式のusdcと人が理解できるテキスト形式のusdaがあり、相互変換できる
* 複数のUSDファイル、テクスチャファイル、Audioファイルを1つのファイルにパックしたUSDZ形式
* シーン構造を持つことができる
* Audio情報を保持できる (usdzで使用する場合は、M4A, MP3, WAV) *1
* テクスチャイメージを保持できる (usdzで使用する場合は、png/jpeg)
* Meshの場合、三角形でなく多角形を保持できる
* Meshの面の表裏を表示するためのdoubleSidedオプション (Offにする、または指定しないデフォルトだと片面表示)
* Meshの面の法線/UV指定は、頂点ごと(vertex)か面の頂点ごと(faceVarying)かを選べる
* 1Meshに対して複数UVの保持
* PBRマテリアルを指定
* Subdivision(catmullClark)指定でメッシュを動的に滑らかに再分割
* 複数のUSDファイルに3Dモデルを分離し、ファイル参照する構造を作ることができる
* Displacementマップの対応
* 個々の形状ごとのTransform animation、ボーン+スキンを使用したSkeletal animationの対応

*1 : AudioについてはUSDに仕様が存在することは確認しましたが、usdviewやiOS13で再生を確認できてません。    

## ビルド情報

* [USDのビルド (Win)](./doc/usd_build_win.md)    
* USDのビルド (Mac)
* [NVIDIAのビルドされたライブラリを使う](./doc/usd_nvidia.md)    
* [USDのライブラリを使ったC++プログラムを書く (Win)](./doc/usd_write_app_win.md)    
* [USDのライブラリを使ったプログラムを実行 (Win)](./doc/usd_run_lib_win.md)    

## USDツールセット

USDの付属のコマンドラインツールです。    
これはPython経由で実行されます。     
以下に使用できるツールの説明があります。    
https://graphics.pixar.com/usd/docs/USD-Toolset.html    

なお、実行前に [Pythonを使用するパスを通しておく(Win)](./doc/usd_run_lib_win.md) 必要があります。    
ツールの一部を説明します。    

* [usdcat : usdファイルの中身を見る、usdc/usdaの相互変換](./doc/toolset/usd_toolset_usdcat.md)
* [usdview : usdの構造と3D形状を見るビュワー](./doc/toolset/usd_toolset_usdview.md)
* [usdzip : usdc/usdaやテクスチャイメージをusdz形式に変換](./doc/toolset/usd_toolset_usdzip.md)

## USD情報

### 概要

* [USDファイルの構成](./doc/usd_files_desc.md)    
* [シーン単位](./doc/unit.md)    
* [座標系](./doc/scene_axis.md)    
* [USDの内部構成(usda)](./doc/usd_usda.md)    
* [usdaでのコメントの記述](./doc/usd_usda_comment.md)    
* [使用できるジオメトリの種類](./doc/usd_geom.md)    
* [ノード(Prim)の種類](./doc/usd_prim_type.md)    

### 再生環境による比較

* [usdview/iOS12/iOS13での比較](./doc/usd_compare_viewer.md)

### C++での実装とusdaでの記述

* [MeshやXformの移動/回転/スケールを指定](./doc/usd_prim_transform.md)
* [Meshを作成](./doc/usd_create_mesh.md)
* [Materialを作成](./doc/usd_create_material.md)
* テクスチャの繰り返し(wrap)
* すでに存在するパスからUsdPrimを探す
* アニメーション情報を出力
* Subdivision対応
* Displacement対応
* テクスチャUVを移動/回転/スケールする (UsdTransform2d)
* テクスチャのピクセル色の変換 (Shader : UsdUVTextureのbias/scale)
* USDファイルを分離して管理
