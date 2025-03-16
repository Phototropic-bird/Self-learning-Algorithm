# Self-learning-Algorithm
My highschool self-learning project 1

以下是我對每個演算法的想法、筆記和解題構思

1.遞迴

-基本上就是在函數中呼叫函數本身，有時還會由兩個以上的函數互相呼叫形成鑲嵌遞迴。
最常見的遞迴例子就是費式數列，藉由an=an-1+an-2的遞迴式達成。

2.前綴和和差分

-手寫筆記: https://acrobat.adobe.com/id/urn:aaid:sc:AP:a6096ddc-4ae1-4d46-ae6d-aea8be0509ef
前綴和:
Sn為第n項前綴和，Sn=an+Sn-1，且定義S0=0
差分:
差分Dn=an-an-1，且令a0=D0=0。
特性:

-可以發現差分的前綴和就會等於原序列，因為D1=a1，且Dn=an-an-1。
前綴和和差分就是利用數值的順序及加減法區間操作的效果

-ZeroJuge f638 解題邏輯圖
![image](https://github.com/user-attachments/assets/1b61d812-9949-4b86-9209-903c0d95d581)

3.BFS

-手寫筆記: ![image](https://github.com/user-attachments/assets/59d75b02-9193-48b9-a1fa-1a6ff5953c03)

-ZeroJudge m372 解題構思
這是我第一次參加APCS的第三題，當時我看到連通塊，第一反應是「並查集」。忽略了搜尋遍歷本身。
這題的重點是當找到一塊水管要怎麼繼續尋找其他連通的水管，所以要用到圖的遍歷。事實上，這題不管用BFS和DFS都可以解決(只是Zerojudge上的測資使用DFS會只有95%)，題目並沒有著重於訪問的順序，只要訪問得到即可。
由地圖的左上到右下檢查是否有未訪問過，若遇到沒有訪過的，並對該管子連通的方向使用BFS繼續檢查，在途中紀錄管子塊數，將訪問過的管子標記，以免重複訪問。
另外後來這題其實有類似並查集的解法。遍歷方法為由陣列的左上到右下。每遇到一個新水管檢查其左上是否有連通水管，若由則沿用左上水管的編號(建立並查集陣列)，否則將此水管增加新標號。如此一來就得道連通塊地圖。在遍歷一次，找出出現最多的編號即可。(好快

4.DFS

-另一種遍歷圖的演算法。以下也是使用樹狀圖以及無向圖來做舉例。
實作時，通常會用遞迴。

-手寫筆記: ![image](https://github.com/user-attachments/assets/09541910-e471-4f04-ac14-76d9b34449f5)

5.Dynamic Program (動態規劃)

 一個把大問題分成很多小問題來解決的邏輯方法統稱

  a.	背包DP (Knapsack Problem)
  
  -先想想看一個例題:
  給定n個不同物品，每個物品都有自己的重量以及價值。現在有一個限重為W的背包，要背包可背物品最大總價值為?
  背包DP問題常圍繞在這個狀態轉移方程
  Dp[i][j]=MAX(有時是MIN){Dp[i-1][j-cost[i]]+val[i],Dp[i-1][j]}
  Dp[i][j]表示到第i個物品時，有”扣打(可載重量)”j時的問題答案
  Cost[x]是x物品的重量
  Val[x]是x物品的價值
  所以整句方程式翻成白話文:
  到第i件物品限重為j時的答案為以下兩者取最大值
  .	要放入i物品，背包價值為「到上一件物品，背包負重為j-i物重(以放入物i)的答案+物i的價值」
  .	不放入i物品，背包價值為「到上一件物品為止，背包負重一樣為j時的問題答案」，就是和沒i一樣
  而這個方程式最常的使用方法就是，依序跑過每個物品，跑每個物品時又跑過每個可能重量，這樣可以使該物每個可能重量(cost[i]~W)都有值。
  方便下一個物品取樣。
  事實上，背包問題有很多分支:「0/1(一個物品只有一個)、Bounded(物品有限定數量)、Unbounded(物品沒有限制數量)、Coin Problem、Pearson’Algorithm 等」
  而這裡有個解省資料空間的小技巧:滾動式修正，就是利用取得上層資料的順序的規律來達到縮減陣列維度的方法。
  在大部分的例子:
  無限背包:前到後(可以取得同層值)
  有限背包:後到前(只能取得上層值)
  Coin Changing:前到後(錢幣數量無限的話)
  
  
0/1 

  -ZeroJudge 置物櫃分配 解題構思:
  題目只有問所需還的櫃子總數所以只需要以客人櫃子總數sum當作最大限重，去探討每個重量是否可被達成。最後在從真正需要的櫃子數(扣掉所剩)while 到有可達成櫃子數即可。


和其他問題做結合

  -Zerojudge 投資遊戲 m373 解題構思:
   當K=0時，就是一般的最大區間和問題
  Dp[i]=max{dp[i-1],0}+a[i]，(dp[0]=0)，包含第i個且到第i個值得答案為max{dp[i-1]，0}+此值。最後在跑過整理完的數列，找出最大值。
  而當K>0時，就是給定有限資源，問最好情況的背包問題!
  物品是金牌，限重為金牌個數，價值為金牌的功用下的最大區間和。狀態轉移方程是決定一個金牌該不該放在一個位置，所以將最大區間和的方程稍作修改。
  Dp[i][j]=max{dp[i-1][j-1]+a[i]	//不用金牌,dp[i-1][j-1] //使用金牌}
  Dp[i][j]為金牌數為i時到第j個值的最大區間和大小
  a.	dp[i][j-1]+a[i]金牌個數為i時(沒用金牌)，到上一個值的最大區間和大小+a[i]表示不放金牌的情況(不用金牌到第i且包含第i)
  b.	dp[i-1][j-1]同上，只是不用吃到第a[i]因為使用了金牌。
      
  b.	區間DP
  
一段區間(通常是在陣列上)，的DP。主要解決方法是將其細分為更多小區間。
    
一維區間 
    
  Zerojudge 合併成本---m934 解題構思:
   ![image](https://github.com/user-attachments/assets/7460e419-b06c-4b62-9d12-f7c97b20e2bd)
  所以只要慢慢將區間大小從2增加到n，這樣back-up上來就好。
      
二維區間
    
  TCIRC  刪除邊界 解題構思:
  ![image](https://github.com/user-attachments/assets/9e6dc5d5-9dfb-4149-8187-9e90a6260fe8)
  所以只要從(1*1)的矩陣就行和列套雙層迴圈直到(n*m)
   (1*1)~(1*2)~(1*3)……(1*m)~
  (2*1)~(2*2)~(2*3)……(2*m)~
  …(n*m)

  c. 樹形DP
  
Zerojudge 病毒演化 f.582 解題構思:
![image](https://github.com/user-attachments/assets/a1609f56-66c7-4e3a-b70a-e5d04f9b9a31)

  d. 其他DP
  
這裡舉的例子是最大區間和:
Zerojudge 內積 i40 解題構思:
其實就是在考最大區間和。所以只需要窮舉出所有可能區間就好。至於怎麼窮舉:![image](https://github.com/user-attachments/assets/e37fe74b-d43a-4231-aaa2-c973d4ec90db)

6.  Binary search (二分搜)

-二分搜主要會用在有連續性，或單調性(1或0，像是在000111字串中找分界點)。
   
真假子圖---from Zerojudge 解題構思:
這個牽扯到二分圖的判斷。
![image](https://github.com/user-attachments/assets/fe7c82eb-0a43-4da5-abbc-572d9c881cc3)
(source: https://web.ntnu.edu.tw/~algo/BipartiteGraph.html)
先用原本的資料構建一個正確的二分圖。
要判斷一個調查員Ai的資料是否為正確的則要將A1~A(i-1)的調查員資料全部納入圖中，並檢查此圖是否為二分圖。
可以保證一個錯誤的調查員出現後會使整張圖變得不是二分圖。
也就是說，若不是二分圖，代表Ai之前有錯誤的調查員。
如此一來，使資料有單調性，可以使用二分搜。
另外，用DFS判斷是否為二分圖(判斷是否有環)。
    
7. 位元運算
   
   互補CP---from Zerojudge
   解答:
   可用到二元運算
	  a.「^」因為
		  11111^10111=01000	可以表示出集和(A-B)
   用
	  b.「|」因為取連集
		  10011 | 01100 =11111
		  10011 | 11100 =11111
  不管交集。
  來處理重複的字母。
  所以只需要把每一個字串由字母種類轉成一個數字(ex.ABCE=11101)然後訪進map中，最後用(1<<m)-1代表總集和，用總集和^子集合來找有沒有互補集合。

8. Joseph Algorithm
    
   定時K彈---from Zerojudge 解題構思:
   每次死者死亡，就將全部的人扣除死者編號(取mod)得到新的圖，並依此圖算下一個死者。因為每次重新編號完，都是從編號1(每次的幸運兒)開始數，所以最後的幸運兒是經過這k次淘汰後編號為1的人。
   為了方便，從0反推，每次加M (把死者重新加入)。(記得取mod(加入死者後的總人數))
   最後，因為題目為1-base所以編號再加1。
   
9. Stack

    p&q的邂逅		from Zerojudge 解題構思
  解答:
  省略’.’後就像俄羅斯方塊那樣，由左到右，只要新塞入的q下方為p則消去p。否則就單純塞入就好。而這種只需要知道以及移除最上方元素，以及先進去的資料後看到，剛好很適合使用堆疊。

10. 佇列
    
    佇列的出口和堆疊的相反，是在放入端的對端。也就是說，佇列的資料順序是先進先出。
佇列還有很多多變種:
1.	優先佇列:佇列內的資料會被分級「優先及」，最前端被取出的資料會是優先及最高的資料。
2.	雙向佇列:結合了堆疊的特性，兩端都可以取放新元素。
佇列的應用之一就是用來實作BFS，以下例題:
  傳送點—from Zerojudge 解題構思:
BFS去找路，去過的路要標。
而因為要回傳步數，所以每一層都要計數一次。要判斷層與層，將佇列再複製一次再掏控太慢。直接紀錄，每一層加入的下一層個數t。

11. 串列鏈結(linked list)

    「I know a guy who know a guy who know a guy…」
這句話傳達了linked list的主要精神。
Linked list 是一個可以容易加入，刪除元素的資料結構
Linked list 可以用陣列實作:
a[1]=2,a[2]=3,a[3]=4…陣列內容為這個人(i)認識的人a[i]。
在c++也可以用指標實作，其實會比較傾向於用指標實作。因為:
1.	可以動態配置記憶體，較有效率
2.  可以將已經用不到的記憶體刪除，節省空間
3.  相較於固定的陣列大小，指標很彈性
而不管哪一種方法都可能會用到物件導向，將所有個體資料包裝在一個物件中。物件導向的實作在C++會使用class or struct。

   定時K彈—from Zerojudge 解題構思
事實上這是「APCS使用C++」這本書的例題，做法也大同小異，只是該作法套上Zerojudge 的測資過不了(TLE)。所以這邊只是用來舉例一個串列的應用。
不要用Joseph algorith的話。串列就是這題的暴力解，使用串列，並將每一次的死人乖乖算出。

12. 二元樹和二元平衡樹

    二元樹是一種最簡單的樹(如果一元樹不算樹的話)，有以下這些
    
1.	型別:
   
 ![image](https://github.com/user-attachments/assets/5e555d30-b11e-45f5-b714-76c8f9cbff9a)

完整二元樹:每個節點(除葉節點)都是從左子節點填到右節點。而如果有不滿足的節點，則稱為非完整二元樹。

![image](https://github.com/user-attachments/assets/7f212647-2954-4600-b361-b125f5e14ad5)

完滿二元樹:所有節點(除葉節點)都有左右兩個子節點，而葉節點都在同一層。如果高是h(從根節點到葉節點的最短距離(邊數))則會共有2^(h+1)-1個節點。

 ![image](https://github.com/user-attachments/assets/af2bf62c-bb49-4bd7-a358-3add3063e827)

左(右)歪斜樹:所有節點都只有左(右)節點，而這樣的節點有兩個以上。
(圖片來源: https://ithelp.ithome.com.tw/articles/10242571)

2.	走訪順序:

   ![image](https://github.com/user-attachments/assets/05a2b7ec-65fc-406e-b582-f558cf89e1ec)

(source: https://ithelp.ithome.com.tw/articles/10205571)

a.	前序走訪(preorder traversal)

對於每個子樹由A->B->C順序走訪:
1-2-4-7-8-5-3-6-9-10

b.	中序走訪(inorder traversal)

對於每個子樹由B->A->C順序走訪:
7-4-8-2-5-1-3-9-6-10

c.	後序走訪(postorder traversal)

對於每個子樹由B->C->A順序走訪:
7-8-4-5-2-9-10-6-3-1
其實就是A對BC的順序啦。

排序二元樹(二元搜尋樹):

所有節點的key皆大於左節點的，小於右節點的。
這樣的樹具有資料儲存，查找，刪除價值。
可知照這樣的資料擺放方式，一個節點所有左子樹的key皆會小於此節點的，右子樹則是大於其。

![image](https://github.com/user-attachments/assets/d270266e-c855-4d3d-8b78-90bd48d2ff58)

 (source: https://en.wikipedia.org/wiki/Binary_search_tree)
一個排序二元樹
可以發現若將一串資料，建立二元排序樹，並依inorder訪問，就是將此串資料排序好。
以上面那顆排序樹為例:

Inorder traversal:1-3-4-6-7-8-10-13-14

二元平衡樹:

	在普通的二元樹中，正常來說搜尋一筆資料的時間複雜度為O(logn)。可是當一顆排序樹的資料加入順序不一樣(例如加入的資料順序由大到小)，便會產生一個一維的串列鏈結。查詢一筆資料的時複變為O(n)。即歪斜樹，為了不讓這樣的資料極端狀況發生。
二元平衡樹就派上用場。他可以在資料加入期間，保證沒有任何一顆子樹是歪斜樹。保證左右子樹高差(|hl-hr|)<=1
利用以下情況對應方法:

![image](https://github.com/user-attachments/assets/29a329fe-0eaa-48bb-821a-4536515b2ba6)
(source:「圖說演算法C++」p.9-47)

AVL 實作成果

![image](https://github.com/user-attachments/assets/241a31b0-560f-4845-b56f-64afe66df556)

而我在Debug時，會將每一次加入後都印出整顆樹(以下是加入前10的成果以及所有成果的手繪對照圖):

![image](https://github.com/user-attachments/assets/12c380f6-2a5f-4643-bec3-f9f6853c005c)
![image](https://github.com/user-attachments/assets/690768d7-26cb-4715-9aee-52941520be56)


13. 並查集
    
    手寫筆記:https://acrobat.adobe.com/id/urn:aaid:sc:AP:cb8cc8f0-060b-464c-aad8-98a6e7c8aeb9

    紅白彩帶---from TCIRC 解題構思:
      DSU(啟發式)+multiset紀錄長度
      使用的是陣列DSU

14. 線段樹與 Lazy tag

    手寫筆記:  https://acrobat.adobe.com/id/urn:aaid:sc:AP:e6981adc-d715-469c-b6a3-a6d80f7f2012

    區間求和d799—from Zerojudge
    
    可不可以像「生產線」那樣用差分。不行
因為生產線那題是經過操作後問一次值(使用loop求前綴和，而且原本是空白的)，而這題是原本就有值，而且操作中間有詢問。用差分和前綴和會很慢。但生產線那題確實也可以用線段樹。

節點標號法 : 
![image](https://github.com/user-attachments/assets/c1d447f1-a680-401f-ba60-756f9ebb0768)

16. 樹狀樹組(BIT)

    一維+二維樹狀數組手寫筆記: https://acrobat.adobe.com/id/urn:aaid:sc:AP:52be6503-8971-4e20-bab4-b930ab3038dd

區間求和d799—from Zerojudge

之前的差分不能單純用來支持d799的操作，但是如果用線段樹或樹狀樹組維護(有了上面那個強大且簡單的數學公式)，便可以使用差分的單點修改來滿足。
維護差分樹組，只需要靠單點修改，和前綴查詢(取得單點值)

17. 紅黑樹 (RBT)

    在上面的AVL可以知道這樣的二元樹雖然不會歪掉，可以有穩定的時間複雜度。可是他要翻轉的頻率太高，而且還要維護左右子樹高，這樣反而會拖累時間複雜度。
於是就有紅黑樹啦。
紅黑樹降低翻轉需要的條件(AVL的條件是平衡因子>1)。藉由將節點的塗色規則來判定是否要修理樹。這樣可以省掉維護左右子樹高的麻煩，大大降低時間複雜度。

手寫筆記: https://acrobat.adobe.com/id/urn:aaid:sc:AP:48c82d52-b693-4603-991c-8e9654445167

小心得:
RBT的刪除看似複雜，其實只是在將不好操作的情況藉由翻轉改色，達到我們熟悉的狀況。
而加入節點的操作在影片中舉了一下例子，相對而言就很容易看懂為甚麼要這樣做。就是在不用看另外一顆子樹的路徑黑節點數狀況下，盡量維持自己這棵的路徑黑節點數。(例如將一顆黑母節點變紅，染黑他孩子)

Coding 大綱 : https://acrobat.adobe.com/id/urn:aaid:sc:AP:1ab62c62-eb64-4b05-a4c2-51ea2cf01d0d

18. B tree

    這個資料結構是我在「圖說演算法」上看到的。書上並沒有解的太詳細，所以我上網路查詢有沒有更詳細的規則解說。
查到的資料說，這個資料結構與Disk structure相似。就是在一大堆資料中為每個資料創造一個pointer而pointer的空間占比較小。而當pointer達到一定數量後，會使搜尋變慢，這時就將其分成好幾個blocks再創造一次pointer’，每個pointer指向一個block，所以要找到一個資料，只需要找到pointer所在的pointer’就好。而當pointer’又變很多時，就重複操作，分區，創造新pointer。

手寫筆記 : https://acrobat.adobe.com/id/urn:aaid:sc:AP:2dd1ec21-a20e-4869-a378-848fc6a96beb

Coding 大綱 : https://acrobat.adobe.com/id/urn:aaid:sc:AP:8983bee6-f5cc-4402-9ab6-20582ac02444








   
    




    



      

      




