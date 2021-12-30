# usdzip

USDファイル(usda/usdc)、テクスチャファイル(png/jpg)、Audioファイル(M4A, MP3, WAV)をまとめて    
1つのusdz形式にします。    
このusdz形式をiOS13/iPadOS13以降のAR Quick Lookに持って行くと    
ARで形状を見ることができるようになります。    

usdzは実態はzip形式ですが、無圧縮でファイルはAlignmentされています。    

## 参考サイト

* https://graphics.pixar.com/usd/release/toolset.html#USDToolset-usdzip    
* https://graphics.pixar.com/usd/release/wp_usdz.html

## 指定のシーンをusdzに変換

xxxx.usdc、image1.png、image2.jpg、の3つのファイルでUSDのシーンが構成されているとします。    

    usdzip xxxx.usdz xxxx.usdc image1.png image2.jpg

これで、「xxxx.usdz」が出力されます。    
なお、ASCII形式のusdaファイルもusdzに入れることができます。    
ファイルサイズを抑える場合は、あらかじめ [usdcat](./usd_toolset_usdcat.md) でusdcに変換してからusdzに変換するほうがいいかもしれません。    

## 複数のUSDファイルが参照される構成の場合

scene.usdc、sphere.usdcの2つのUSDファイルが存在し、    
scene.usdc（シーン全体）からsphere.usdc（1つの形状）を参照する構造の場合、    
以下のようにusdzipを実行します。    

    usdzip xxxx.usdz scene.usdc sphere.usdc

これで、「xxxx.usdz」が出力されます。    
usdzipに指定する第二引数以降で、先にシーン全体である「scene.usdc」を指定するようにします。    

