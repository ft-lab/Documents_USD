# USDについて

ここでは、PixarのUSD ( Universal Scene Description ) の覚書きです。    

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
   * usdcat : コマンドラインでUSDファイルをバイナリ/テキスト形式で相互変換    
   * usdview : USDファイルの3Dビュワー    
   * usdzip : USDファイルとテクスチャリソースを1つのusdz形式に変換    

デフォルトではPython経由でUSDのツール類にアクセスしたり、独自アプリに組み込めます。     
別途、C++ライブラリのみでの使用でPythonに依存しないようにもできます。   
その場合は、一部のUSDツールは使用できません（ライブラリを使ってアプリに組み込む機能はすべて使えます）。

## USDの特徴

これはUSDフォーマットとしての仕様になります。    
usdviewでのビュワー表示、iOS12/iOS13でのAR、など環境によって対応されている内容が異なります。   

* 3Dシーン/3DモデルをUSDファイルとして管理
* 複数のUSDファイル、テクスチャファイル、Audioファイルを1つのファイルにパックしたUSDZ形式
* シーン構造を持つことができる
* Audio情報を保持できる (usdzで使用する場合は、M4A, MP3, WAV) *1
* テクスチャイメージを保持できる (usdzで使用する場合は、png/jpeg)
* Meshの場合、三角形でなく多角形を保持できる
* 1Meshに対して複数UVの保持
* PBRマテリアルを指定
* Subdivision(catmullClark)指定でメッシュを滑らかに再分割
* 複数のUSDファイルに3Dモデルを分離し、ファイル参照する構造を作ることができる
* Displacementマップの対応
* 個々の形状ごとのTransform animation、ボーン+スキンを使用したSkeletal animationの対応

*1 : AudioについてはUSDに仕様が存在することは確認しましたが、usdviewやiOS13で再生を確認できてません。    

## ビルド情報

* [USDのビルド (Win)](./usd_build_win.md)    
* USDのビルド (Mac)
* [NVIDIAのビルドされたライブラリを使う](./usd_nvidia.md)    
* USDのライブラリを使ったプログラムを実行 (Win)

## USDツールセット

USDの付属のコマンドラインツールです。    

* usdcat : usdファイルの中身を見る、usdc/usdaの相互変換
* usdview : usdの構造と3D形状を見るビュワー
* usdzip : usdc/usdaやテクスチャイメージをusdz形式に変換

## USD情報

* [USDファイルの構成](./usd_files_desc.md)    
* [シーン単位](./unit.md)    
* [座標系](./scene_axis.md)    
* [USDの内部構成(usda)](./usd_usda.md)    
