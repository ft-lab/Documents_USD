# usdcat

usdファイルの中身を見たり、usdc/usdaの相互変換を行うコマンドラインツールです。    

## usdの中身を見る

以下を実行すると、第二引数のUSDファイル（usda/usdc/usdz）の中身を表示します。    

    usdcat xxxx.usdc

usdzに複数のusdc/usdaファイルが含まれる場合、先頭のもののみ表示されます。    

## usdcをusdaに変換

バイナリ形式のusdcファイルをASCII形式のusdaファイルに変換します。    

    usdcat xxxx.usdc --out xxxx.usda

これは「usdcat xxxx.usdc」での出力を、--outで指定した「xxxx.usda」に出力する動きになります。

## usdaをusdcに変換

ASCII形式のusdaファイルをバイナリ形式のusdcファイルに変換します。    

    usdcat xxxx.usda --out xxxx.usdc


