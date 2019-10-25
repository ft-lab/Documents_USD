# usdaでのコメントの記述

usdaファイルでのコメントは、    
「//」または「#」を指定して1行をコメント化する方法と、    
複数行対応の「/* ... */」で囲む方法があります。    

    #usda 1.0
    
    /*
      USD test scene
      date: 10/25/2019
    */

    def Xform "hello"
    {
        def Sphere "sphere"
        {
            color3f[] primvars:displayColor = [(1, 0.2, 0)]
     
            // double radius1 = 1.5   // これはコメント行.
            # double radius2 = 1.2    // これはコメント行.
            double radius = 1.3
        }
    }

