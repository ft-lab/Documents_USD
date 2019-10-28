# ノード(Prim)の種類

usdaではXformでNULLノード相当、Materialがマテリアル、Meshがメッシュ構造、など「Prim」の種類が存在します。    

usdaファイルでは以下のように記述されます。    

    def Xform "root"
    {
        def Scope "Materials"
        {
            def Material "black"
            {
            }
        }

        def Mesh "mesh_1"
        {
        }
    }

このうち「def Xform "root"」の2つめがPrimの種類になります。    


ジオメトリの場合は「 [ジオメトリの種類](./usd_geom.md) 」をご参照くださいませ。    
その他、以下のようなものがあります。    

|Primの種類|説明|
|---|---|
|Xform|変換行列/アニメーション要素を持つことができる|
|Scope|単なる入れ物。グループ分けする場合などで使用|
|Material|マテリアル|
|Shader|マテリアル内のShader要素|
|SkelRoot|スケルトン（ボーン構造）のルート|
|Skeleton|スケルトン情報|
|SkelAnimation|スケルトンでのアニメーション情報|
|SphereLight|点光源|
|RectLight|面光源|
|DomeLight|全天球の光源（背景ファイルを指定して、IBLとする）|


