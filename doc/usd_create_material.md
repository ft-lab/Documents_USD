# Materialの作成

マテリアルの仕様は以下が参考になります。    
「UsdPreviewSurface Proposal」    
https://graphics.pixar.com/usd/docs/UsdPreviewSurface-Proposal.html    

## Material情報を指定

### C++での記述

ヘッダ部は以下の指定を行っています。    

    #include "pxr/usd/usd/stage.h"
    #include "pxr/usd/usdGeom/xform.h"
    #include "pxr/usd/usdGeom/mesh.h"
    
    #include "pxr/usd/usdShade/material.h"
    #include "pxr/usd/usdShade/shader.h"
    
    #include <vector>
    
    using namespace PXR_INTERNAL_NS;

「[Meshを作成](./usd_create_mesh.md)」のコードの「stage->Save()」の前に以下を記載するとします。    
「prim(型はUsdPrim)」がMesh情報とします。    

    // "material_0"のマテリアルを作成.
    UsdPrim primMat = stage->DefinePrim(SdfPath("/hello/material_0"), TfToken("Material"));
    UsdShadeMaterial mat(primMat);
    
    // PBR Shaderの作成.
    UsdPrim primShader = stage->DefinePrim(SdfPath("/hello/material_0/PBRShader"), TfToken("Shader"));
    UsdShadeShader shader(primShader);
    
    shader.CreateIdAttr().Set(TfToken("UsdPreviewSurface"));
    
    shader.CreateInput(TfToken("diffuseColor"), SdfValueTypeNames->Color3f).Set(GfVec3f(0.5f, 0.9f, 0.2f));
    shader.CreateInput(TfToken("roughness"), SdfValueTypeNames->Float).Set(0.3f);
    shader.CreateInput(TfToken("metallic"), SdfValueTypeNames->Float).Set(0.3f);
    
    // UVの接続.
    mat.CreateInput(TfToken("frame:stPrimvarName"), SdfValueTypeNames->Token).Set(TfToken("st"));
    
    // MaterialからShaderをつなぐ.
    mat.CreateSurfaceOutput().ConnectToSource(shader, TfToken("surface"));
    
    // マテリアルをprimの形状にバインド.
    mat.Bind(prim);

### usdaでの出力

実行すると、以下の”xxxx.usda”が出力されます。    

    #usda 1.0
    
    def Xform "hello"
    {
        def Mesh "mesh"
        {
            uniform bool doubleSided = 0
            int[] faceVertexCounts = [4]
            int[] faceVertexIndices = [0, 1, 2, 3]
            normal3f[] normals = [(0, 1, 0), (0, 1, 0), (0, 1, 0), (0, 1, 0)]
            point3f[] points = [(-1, 0, -1), (-1, 0, 1), (1, 0, 1), (1, 0, -1)]
            texCoord2f[] primvars:st = [(0, 0), (1, 0), (1, 1), (0, 1)] (
                interpolation = "varying"
            )
            uniform token subdivisionScheme = "none"
            rel material:binding = </hello/material_0>
        }

        def Material "material_0"
        {
            token inputs:frame:stPrimvarName = "st"
            token outputs:surface.connect = </hello/material_0/PBRShader.outputs:surface>
    
            def Shader "PBRShader"
            {
                uniform token info:id = "UsdPreviewSurface"
                color3f inputs:diffuseColor = (0.5, 0.9, 0.2)
                float inputs:metallic = 0.3
                float inputs:roughness = 0.3
                token outputs:surface
            }
        }
    }

usdviewで見ると以下のようになります。     
<img src="../images/usd_cpp_material_01.jpg" />    

形状（ジオメトリ）からマテリアルの参照は「rel material:binding = </hello/material_0>」のようにパス指定になります。    
「def Material "material_0"」にマテリアル情報を記載しています。    
「def Shader “PBRShader”」でマテリアルのパラメータを指定。    
「uniform token info:id = “UsdPreviewSurface”」としてつなげることで、
PBRマテリアルに沿ったパラメータ指定を行うことになります。    

## Materialの構成とパラメータ

まだ途中、、、     
