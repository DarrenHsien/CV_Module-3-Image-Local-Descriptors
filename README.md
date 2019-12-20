# Module-3-Image-Local-Descriptors

關鍵點探測

1.Key Point : Fast
  
  -目的 : 找 "角" 點
  
  -運算流程 : 
    
    -把某一個pixel當作中心pixel稱A，假設其值為50
    
    -定義出r(pixel向外延伸的半徑)以及n(數量閾值)以及t(用來製作A_value+t或A_value-t的閾值)
    
    -沿著A往外擴r=3半徑畫圓可得16連續路徑的pixel
    
    -當16個Pixel中有連續大於n個PIXEL的值是大於50+t 或小於50+t的我們則可視為該點為一個關鍵點


2.Key Point : Harris
  
  -目的 : 找 "角" 點
  
  -運算流程 : 
    
    -計算整張影像的水平與垂直梯度GX,GY
    
    -找一個像素區間(3*3 etc)
    
    -因此我們可以再每一個Pixel對應的像素區間上產出一個矩陣M = [[sum(Gx^2) , sum(Gx*Gy)],[sum(Gx*Gy), sum(Gy^2)]]
    
    -計算出M矩陣的特徵值與行列式值
    
      -det(M) < 0 : 代表該像素所在區域應該是"邊緣"並非"角"
      
      -det(M) 很小 : 代表該像素所在區域應該平坦無角區域"
      
      -det(M) 較大 : 代表該像素為關鍵點"
      
3.Key Point : DoG(Different of Guassian) 等同 SIFT

  -目的 : 找 "斑" 點(可能為角點或邊緣或綜合) ， 可於不同影像尺度空間查找相同的關鍵點
  
  -運作流程 : 
    
    -等比例縮小(一半)影像，因此會產出好幾個不同尺度影像
    
    -每個比例的影像都生出連續五張影像但搭配不同的高斯模糊強度，並將其兩兩相減，輸出四張
      
      -依此類推 : 原始影像產出4張DOG ,0.5倍影像也產出四張 ....
      
      -最終同個比例要產出三張DOG影像，以中間影像為主，某個PIXEL的像素值與26個PIXEL(其周圍8個PIXEL + 上下層各9個PIXEL )都大於或都小於則判定為關鍵點
      
    - 由於不同的比例皆會判定出關鍵點，因此DOG的關鍵點描述也會有大小範圍差


4.Key Point : Fast Hessian 等同 SURF
  
  -目的 : 找 "斑" 點(可能為角點或邊緣或綜合) ，用以取代DoG計算速度較慢的詬病， 可於不同影像尺度空間查找相同的關鍵點
  
  -運作流程 : 
    
    -等比例縮小(一半)影像，因此會產出好幾個不同尺度影像
    
    -最終同個比例要產出三張經過"箱體濾波的"影像，以中間影像為主，某個PIXEL的像素值與26個PIXEL(其周圍8個PIXEL + 上下層各9個PIXEL )都大於則判定為關鍵點
    

局部影像特徵描述

1.Local invariant description : SIFT


2.Local invariant description : RootSIFT


3.Local invariant description : SURF


4.Local invariant description : Feature extraction and matching

局部影像特徵描述

1.Binary description : BRIEF

2.ORB

3.BRISK

4.Binary description : Feature extraction and matching



  
