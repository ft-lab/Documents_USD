# マテリアル(UsdPreviewSurface)とShader

USDの標準のマテリアルは「UsdPreviewSurface」( https://graphics.pixar.com/usd/release/wp_usdpreviewsurface.html )というShaderが使用されます。      
これはUSD内で置き換えることもでき、[NVIDIA Omniverse](https://www.nvidia.com/ja-jp/omniverse/) ではMDL( https://www.nvidia.com/ja-jp/design-visualization/technologies/material-definition-language/ )を使ってマテリアルを拡張しています。     
UsdPreviewSurfaceは最低限の機能が提供されていますが、DCCツール側から見ると少し物足りない点があります。     

なお、UsdPreviewSurface以外を使う場合はそれを理解できる実装/環境が必要になります（このあたりが増えてしまうと、方言を生むことになる）。    
USDに付属のビュワー「usdviewer」ではUsdPreviewSurfaceのみ反映されます。     

## Meshにマテリアルを割り当てる



### UsdPreviewSurfaceでテクスチャを指定する場合

Metallic/Roughness/Occlusionなどの他のパラメータでも同じですが、
単色であるBaseColorとBaseColorテクスチャを両立させるには構成を変える必要があります。    
UsdPreviewSurfaceのShaderでは「inputs:diffuseColor」に最終的なPBRマテリアルとしての色を入れます。     

単色のみの場合は以下の記述のようになります。      

```
def Material "material"
{
    def Shader "PBRShader"
    {
        uniform token info:id = "UsdPreviewSurface"
        color3f inputs:diffuseColor = (1, 1, 1)
        float inputs:ior = 1
        float inputs:metallic = 0
        float inputs:opacity = 1
        float inputs:roughness = 0
        token outputs:surface
    }
}
```

テクスチャを指定する場合は以下のような記述になります。     

```
def Material "material"
{
    token inputs:frame:stPrimvarName = "st"
    token outputs:surface.connect = </root/Materials/material/PBRShader.outputs:surface>

    def Shader "PBRShader"
    {
        uniform token info:id = "UsdPreviewSurface"
        color3f inputs:diffuseColor.connect = </root/Materials/material/diffuseTexture.outputs:rgb>
        float inputs:ior = 1
        float inputs:metallic = 0
        float inputs:opacity = 1
        float inputs:roughness = 0
        token outputs:surface
    }

    def Shader "stReader"
    {
        uniform token info:id = "UsdPrimvarReader_float2"
        token inputs:varname.connect = </root/Materials/material.inputs:frame:stPrimvarName>
        float2 outputs:result
    }

    def Shader "diffuseTexture"
    {
        uniform token info:id = "UsdUVTexture"
        asset inputs:file = @basecolor_tex.png@
        float2 inputs:st.connect = </root/Materials/material/stReader.outputs:result>
        token inputs:wrapS = "repeat"
        token inputs:wrapT = "repeat"
        float4 inputs:scale = (1, 0.5, 0.2, 1)
        float4 inputs:bias = (0, 0, 0, 0)
        color3f outputs:rgb
    }
}
```

テクスチャ指定時はUsdPreviewSurfaceの「inputs:diffuseColor」を「inputs:diffuseColor.connect」に置き換えて、
UsdUVTextureでテクスチャに必要な情報を指定する必要があります。     
また、UVの指定は「inputs:st.connect」を参照しています。     
テクスチャに色の乗算を行う場合はUsdUVTextureで「inputs:scale」を指定します。     


