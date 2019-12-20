# Module-3-Image-Local-Descriptors


1.Key Point : Fast

  -運算流程 : 
    
    -把某一個pixel當作中心pixel稱A，假設其值為50
    
    -定義出r(pixel向外延伸的半徑)以及n(數量閾值)以及t(用來製作A_value+t或A_value-t的閾值)
    
    -沿著A往外擴r=3半徑畫圓可得16連續路徑的pixel
    
    -當16個Pixel中有連續大於n個PIXEL的值是大於50+t 或小於50+t的我們則可視為該點為一個關鍵點


2.Key Point : Harris
  
  -運算流程 : 
    
    -計算整張影像的水平與垂直梯度GX,GY
    
    -找一個像素區間(3*3 etc)
    
    -因此我們可以再每一個Pixel對應的像素區間上產出一個矩陣M = [[sum(Gx^2) , sum(Gx*Gy)],[sum(Gx*Gy), sum(Gy^2)]]
    
    -計算出M矩陣的特徵值與行列式值
    
      -det(M) < 0 : 代表該像素所在區域應該是"邊緣"並非"角"
      
      -det(M) 很小 : 代表該像素所在區域應該平坦無角區域"
      
      -det(M) 較大 : 代表該像素為關鍵點"
      






