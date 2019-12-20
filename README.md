# Module-3-Image-Local-Descriptors


1.Key Point : Fast

  -運算流程 : 
    
    -把某一個pixel當作中心pixel稱A，假設其值為50
    
    -定義出r(pixel向外延伸的半徑)以及n(數量閾值)以及t(用來製作A_value+t或A_value-t的閾值)
    
    -沿著A往外擴r=3半徑畫圓可得16連續路徑的pixel
    
    -當16個Pixel中有連續大於n個PIXEL的值是大於50+t 或小於50+t的我們則可視為該點為一個關鍵點










