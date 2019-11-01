# Meshを作成

## Mesh情報を指定

### C++での記述

    #include "pxr/usd/usd/stage.h"
    #include "pxr/usd/usdGeom/xform.h"
    #include "pxr/usd/usdGeom/mesh.h"
    #include <vector>
    
    using namespace PXR_INTERNAL_NS;
    
    //---------------------------------------------------.
    UsdStageRefPtr stage = UsdStage::CreateNew("xxxx.usda");
    
    UsdPrim node1 = stage->DefinePrim(SdfPath("/hello"), TfToken("Xform"));
    
    // Meshを作成.
    UsdPrim prim = stage->DefinePrim(SdfPath("/hello/mesh"), TfToken("Mesh"));
    UsdGeomMesh geomMesh(prim);
    
    // Meshの頂点を格納.
    {
        std::vector vList;
        vList.push_back(GfVec3f(-1, 0, -1));
        vList.push_back(GfVec3f(-1, 0, 1));
        vList.push_back(GfVec3f(1, 0, 1));
        vList.push_back(GfVec3f(1, 0, -1));
   
        UsdAttribute attr = geomMesh.CreatePointsAttr();
        VtVec3fArray ar = VtVec3fArray(vList.begin(), vList.end());
        attr.Set(ar);
    }
    
    // 頂点の法線を格納.
    {
        std::vector<GfVec3f> vList;
        vList.push_back(GfVec3f(0, 1, 0));
        vList.push_back(GfVec3f(0, 1, 0));
        vList.push_back(GfVec3f(0, 1, 0));
        vList.push_back(GfVec3f(0, 1, 0));
    
        UsdAttribute attr = geomMesh.CreateNormalsAttr();
        VtVec3fArray ar = VtVec3fArray(vList.begin(), vList.end());
         attr.Set(ar);
    }
    
    // Meshの面情報を格納.
    {
        std::vector<int> iList;
        iList.push_back(4);
    
        UsdAttribute attr = geomMesh.CreateFaceVertexCountsAttr();
        VtIntArray ar = VtIntArray(iList.begin(), iList.end());
        attr.Set(ar);
    }
    {
        std::vector<int> iList;
        iList.push_back(0);
        iList.push_back(1);
        iList.push_back(2);
        iList.push_back(3);
    
        UsdAttribute attr = geomMesh.CreateFaceVertexIndicesAttr();
        VtIntArray ar = VtIntArray(iList.begin(), iList.end());
        attr.Set(ar);
    }
    
    // UVを格納.
    {
        std::vector<GfVec2f> uvList;
        uvList.push_back(GfVec2f(0, 0));
        uvList.push_back(GfVec2f(1, 0));
        uvList.push_back(GfVec2f(1, 1));
        uvList.push_back(GfVec2f(0, 1));
    
        UsdGeomPrimvar primV = geomMesh.CreatePrimvar(TfToken("st"), SdfValueTypeNames->TexCoord2fArray, UsdGeomTokens->varying);
        UsdAttribute attr = primV.GetAttr();
        VtVec2fArray ar = VtVec2fArray(uvList.begin(), uvList.end());
        attr.Set(ar);
    }
    
    // doubleSided指定.
    geomMesh.CreateDoubleSidedAttr(VtValue(false));
    
    // Subdivision情報（なし）を格納.
    {
        UsdAttribute attr = geomMesh.CreateSubdivisionSchemeAttr();
        attr.Set(TfToken("none"));
    }

    stage->Save();

### usdaでの出力

実行すると「xxxx.usda」は以下のようになります。    

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
        }
    }

## Meshを構成する要素

Meshは、頂点座標、頂点法線、面ごとの頂点数、面の頂点インデックス、UV、subdivisionの指定、などで構成されます。    
また、複数のUVレイヤを持つことができ、頂点カラーも保持することができる構造になっています。    

「int[] faceVertexCounts」で面ごとの頂点数を指定します。    
「int[] faceVertexIndices」は面の頂点インデックスを指定します。    
「point3f[] points」は頂点座標です。    
「texCoord2f[] primvars:st」は頂点ごとのUVを指定します。上記の指定(interpolation = "varying")の場合はpointsと同じ数である必要があります。    
別途、UV指定を「面ごとの頂点」で与える指定も可能です。これは後述します。    

なお、UVは、V値が逆になっています。    
参考 : https://graphics.pixar.com/usd/docs/UsdPreviewSurface-Proposal.html#UsdPreviewSurfaceProposal-TextureCoordinateOrientationinUSD    

「bool doubleSided」がTrueの場合は両面表示(1)。デフォルトはFalse(0)です。    
doubleSidedの指定がない場合は、リアルタイム環境では裏面を向くと面が見えなくなります。    

「subdivisionScheme」で"none"を指定しない場合、デフォルトではサブディビジョンの再分割がかかることになります。    
ビュワー環境によっては意図した結果にならないことになるため、これは明示的に"none"を入れるようにします。    

### UV指定を面の頂点に与える

リアルタイムでは、頂点ごとの法線とUVを与える場合が多いですが（そのため重複頂点が増える）、
USDでは「UVは面の頂点ごとに別途割り当て」ることができます。    

usdaでは以下のように記述します。    

    texCoord2f[] primvars:st = [(0.75,0.43),(0.5,0.87),(0.25,0.43),
    (1,0.87),(0,0.87),(0.5,0)] (
        interpolation = "faceVarying"
    )
    int[] primvars:st:indices = [0,1,2, 3,1,0, 4,2,1, 5,0,2]

「primvars:st」にUVを格納しますが、このときにinterpolationで"faceVarying"を指定します。    
C++では「geomMesh.CreatePrimvar」の第三引数に「UsdGeomTokens->faceVarying」を渡すことになります。    

そのあと、「primvars:st:indices」に面ごとのUV頂点インデックスを指定していきます。     
これは「faceVertexIndices」と同じ個数分の指定です。    

C++では以下のような指定を行うことになります。    

    {
        // uvListにUV値を格納.
        std::vector<GfVec2f> uvList;
        uvList.push_back(GfVec2f(0.75f, 0.43f));
        uvList.push_back(GfVec2f(0.5f, 0.87f));
        uvList.push_back(GfVec2f(0.25f, 0.43f));
        uvList.push_back(GfVec2f(1.0f, 0.87f));
        uvList.push_back(GfVec2f(0.0f, 0.87f));
        uvList.push_back(GfVec2f(0.5f, 0.0f));

        UsdGeomPrimvar primV = geomMesh.CreatePrimvar(TfToken("st"), SdfValueTypeNames->TexCoord2fArray, UsdGeomTokens->faceVarying);
        UsdAttribute attr = primV.GetAttr();
        attr.Set(VtVec2fArray(uvList.begin(), uvList.end()));
    }
    {
        std::vector<int> uvFaceIndexList;
        ... // uvFaceIndexListに値を入れる.

        UsdGeomPrimvar primV = geomMesh.GetPrimvar(TfToken("st"));
        if (primV) {
            UsdAttribute attr = primV.CreateIndicesAttr();
            attr.Set(VtIntArray(uvFaceIndexList.begin(), uvFaceIndexList.end()));
        }
    }


