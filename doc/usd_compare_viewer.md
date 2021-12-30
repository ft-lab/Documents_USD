# old : usdview/iOS12/iOS13での比較

これは古いusdviewは USD 19.07/iOS 12/iOS 13時の情報になります。    


USD付属の「usdview」、iOS12/iOS13でのAR Quick Lookで、usd(z)の表示できる機能とできない機能を一覧します。    

なお、usdviewは USD 19.07付属のものをWindows 10環境で試しました。    
iOS 12.4.1、iOS 13.2で試しました。    

## 各機能の比較

USDの仕様範囲で、    
それぞれの環境で表現できるものを「OK」、表現できないものを「NG」としています。    

|チェック項目|usdview (19.07)|iOS12|iOS13|
|---|---|---|---|
|usdzにusdcファイルを入れる|OK|OK|OK|
|usdzにusdaファイルを入れる|OK|NG|OK|
|usdzに複数のusda/usdcファイルを入れて、usdファイル間で参照|OK|NG|OK|
|頂点法線の反映|NG|OK|OK|
|法線マップの反映|NG|OK|OK|
|MeshのDoubleSided|OK|NG|NG|
|Meshでの複数UV(UV2の使用)|OK|NG|NG|
|MaterialのDisplacement対応|NG (*1)|NG|NG|
|MaterialのSubdivision対応|OK|NG|NG|
|MaterialのOpacityを使った半透明表示 (Z Sort)|NG (*2)|NG (*3)|OK|
|MaterialのOpacityを使った透過ピクセル表示 (Cutout)|NG (*4)|NG (*5)|NG (*6)|
|MeshのOpacityを使ったときの影|-|NG (*7)|NG (*7)|
|MaterialのUsdTransform2d|NG|NG|OK (*8)|
|Shader : UsdUVTextureのbias|NG|NG|NG|
|Shader : UsdUVTextureのscale|NG|NG|OK|
|テクスチャのRGBAチャンネルを個別指定|NG (*9)|NG (*9)|OK (*10)|

usdviewでの法線マップの反映について。    
https://github.com/PixarAnimationStudios/USD/issues/701    

usdviewでのDisplacementの反映について。    
https://github.com/PixarAnimationStudios/USD/issues/922    

*1 ... Displacementの反映はされるが、面が分断されてしまう。     

*2 ... 意図した半透明にならない。常に加算合成される ?    

*3 ... Zソートされていない    

*4 ... 半透明と同じで加算合成されていて、かつ、Cutoutは効いていない ?    

*5 ... DiffuseColorにRGBAテクスチャを指定した場合、A要素でトリミングされているように見える。    
ただし、Zソートされていないため前後関係が正しくならない。    

*6 ... ZソートはされているがCutoutはされていないように見える。また、透過ピクセル部分にうっすらと白(Specular?)が乗る。    

*7 ... Opacityで半透明にしても影が濃い    

*8 ... 有効だが、1つのMaterial内ですべてのテクスチャで同じUsdTransform2dの変換がかかる ?    

*9 ... UsdPreviewSurface/UsdUVTextureで、テクスチャのRGBAにroughness/metallic/occlusion/opacityなどパックし、G/B/Aを参照しても正しく取り出せない。    
常にRを見る模様。    
そのため、グレイスケールで別テクスチャにする必要あり。    

*10 ... roughness/metallic/occlusion/opacity使用時に、テクスチャのRGBAを個別参照は有効。    
ただし、emissiveColorテクスチャを使用している場合でopacityテクスチャも使用する場合、opacityテクスチャは常にRチャンネルを参照している ?    
そのため、emissiveColorテクスチャ + opacityテクスチャ使用時は、opacityテクスチャはグレイスケールで与えるほうが安全。    

## それ以外の特記事項

以下のうち [ iOS12 ]と書いているものは、iOS12で起きる問題で、iOS13では修正されているのを確認済みです。    

* [ iOS12 ] スキンアニメーション時、MeshからAnimへの「rel skel:animationSource」を指定しないと、ボーン＋スキンアニメーションが行われない。    
USDの仕様上ではこれはいらない。     
* [ iOS12 ] iCloudで日本語フォルダがある場合、その中のusdzはAR Quick Lookで表示しようとすると失敗する。
* [ iOS12 ] transform animation(Xformでのノードごとのアニメーション)で、xformOp:orientのクォータニオンの回転が機能しない。    
xformOp:rotateXYZだとOK。    
* [ iOS12 ] MaterialのDiffuseColorのAlpha要素を使用すると自動で透過になる（Opacity=1.0時）。    
透過ピクセルの表現を行う場合、opacityのテクスチャに加えて透過ピクセルにroughness 1.0/ios 1.0を与える必要あり。    
→ iOS13では、明示的にAlphaを見るという仕様はなくなり、USDの仕様に合わせて「Opacity」で透過ピクセルを表現できる。    

