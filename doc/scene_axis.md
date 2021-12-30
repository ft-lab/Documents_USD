# 座標系

デフォルトはYが上向きの右手系になります（変更可能）。    
<img src="../images/usd_axis.png" />   

## usdファイルでのUpAxisの指定

USDファイルのヘッダ部の「upAxis」でどの軸を上向きにするか指定できます。    
デフォルトは"Y"が上向きです。     

```
#usda 1.0
(
    defaultPrim = "World"
    endTimeCode = 100
    startTimeCode = 0
    timeCodesPerSecond = 24
    upAxis = "Y"
)
```
