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
    

局部影像特徵描述 : 組建關鍵點周圍的影像特徵

1.Local invariant description : SIFT

  -計算流程
  
    -輸入一組關鍵點

    -針對每個關鍵點抓取其周圍16*16個影像區間

    -將16*16 畫分成 4*4 cells

    -針對每一個cell計算其梯度大小與梯度角度矩陣 ，並且替每個cell計算其定向梯度直方圖(需參考該像素離中心遠近判定貢獻直方圖計算的多寡)

    -統籌16個cells上的直方圖分布然後合併再一起(16*8(direct) dim)

    -對128陣列的值做L2標準化

    -因此一張影像上，會產出對應N個關鍵點的N組128維特徵向量

2.Local invariant description : RootSIFT
  
  -為SIFT的優化
  
  -計算流程
    
    -輸入一組關鍵點

    -針對每個關鍵點抓取其周圍16*16個影像區間

    -將16*16 畫分成 4*4 cells

    -針對每一個cell計算其梯度大小與梯度角度矩陣 ，並且替每個cell計算其定向梯度直方圖(需參考該像素離中心遠近判定貢獻直方圖計算的多寡)

    -統籌16個cells上的直方圖分布然後合併再一起(16*8(direct) dim)

    -Hellinger kernel
    
      -對128陣列的值做L1標準化
    
      -取SIFT向量中每個元素的平方根

    
3.Local invariant description : SURF
  
  -SURF與SIFT的演算邏輯相似，但計算速度較SIFT快上許多(DoG -> Hessian)
  
  -SIFT返回的特徵向量為128dim ， SURF則為64dim
  
  -計算流程
    
    -輸入一組關鍵點

    -針對每個關鍵點抓取其周圍16*16個影像區間

    -將16*16 畫分成 4*4 cells
    
    -從每個cells中計算梯度的大小，並計算出四個值sum(dx),sum(dy),sum(|dx|),sum(|dy|)
    
    -每個cell都返回4個值組成的特徵向量，一張影像就會返回16*4 = 64dim特徵向量

4.Local invariant description : Feature extraction and matching

  -整合上述進行兩張影像之比對
  
  -關鍵點探測器
  
    -DOG
    
    -SIFT
    
    -FAST
    
    -FASTHESSIAN
    
    -SURF
    
    -HARRIS
    
    -ORB(Binary)
    
    -STAR(Binary)
    
  -關鍵點萃取局部特徵的特徵點描述器
  
    -SIFT
    
    -SURF
    
    -FastSIFT
    
  -比對器(KNN) 目前只使用BruteForce，其餘方法尚未理解
  
    -BruteForce
    
    -BruteForce-SL2
    
    -BruteForce-L1
    
    -FlannBased
  
  三種工具整合計算進行影像比對

局部影像特徵描述 : 組建關鍵點周圍的影像特徵(不同於SIFT,SURF等使用梯度萃取特徵，使用二進制的描述方式來萃取)

  -相較梯度描述速度快上許多
  
  -儲存空間的佔有也相對縮減
  
  -相對的二進制描述的方式僅僅善用了像素強度的落差來輸出0與1
  
  -使用 Hamming Distance距離演算法(XOR運算和) 速度剩過 Euclidean Distance 在128 dim & 64 dim的運算速度
  
  -二進制描述有三大步驟
    
    -抽樣的模式
    
      從每個關鍵點中，採收其周圍Pixel的方式
    
    -採樣的成對
      
      將蒐集出來的Pixel集，進行關聯式的建立，好比A跟Z比較,B跟X比較，經過比較後就可以產出我們的一組特徵向量
    
    -方向的補償
      
      兩張影像上同一個物體的某個位置，可能因轉動角度差異造成雖為同個物體但成像旋轉角度不同，因此我們亦必須同時確保關鍵點描出出來的
      二進制描述特徵向量，不管轉動角度都可以同樣辨識出為同一個關鍵點位置
  
  -使用範例 : 假設初始採樣出的PIXEL可以分成512個成對，512個成對經過相互比較就會產出512個0,1的組合，512/UNSIGN-8BIT = 64
  
              因此就可以輸出64個0~255的數字組合
   
      

1.Binary description : BRIEF

2.Binary description : ORB

3.Binary description : BRISK

4.Binary description : Feature extraction and matching



  
