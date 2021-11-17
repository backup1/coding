# Editorials

IOI 2011 : [http://blog.sina.com.cn/s/blog\_4ee63ce90102drjn.html](http://blog.sina.com.cn/s/blog\_4ee63ce90102drjn.html)

> **热带花园：**把每个点拆成两个点使得每个点有唯一的出度。找出P和P'（P拆出的点）所在的仙人掌。从P和P'向回找。计算每个点到P和P'的距离并按对环长的模分类。各种扫描。（来自周而进的思路）。另：O(QN)的暴力是可以过的（我是这么干的）。&#x20;
>
> **长跑比赛：**树分治，不多说了。其实可以用启发式合并的平衡树来搞（来自周而进的思路）&#x20;
>
> **米仓：**求前缀和。线性扫描。米仓一定建在选出的稻田的中位数上。
>
> **鳄鱼：**Dijkstra，用多说吗？&#x20;
>
> **大象：**我的做法：动态树。把每个大象看作一个黑点P，大象往后L+1的地方建立一个白点P'。P的父亲P',P’的父亲是它靠右的最近的一个点（不管颜色）。答案就是最左边的黑点到根的路径上黑点的个数。用动态树、multiset来维护。&#x20;
>
> **鹦鹉：**这是一个很bug的题。官方题解上没有能拿满分的算法。Tourist竟然满了！详细内容我以后贴官方题解吧。
>
> 一个小小的提示：接受者获得的信息实际上是：发送者传送了多少个0、多少个1、。。。、多少个255.

IOI 2011 : [http://gagguy.blogspot.com/2011/07/ioi-2011.html](http://gagguy.blogspot.com/2011/07/ioi-2011.html)

> #### IOI 2011
>
> 今年 IOI 的 problem set 非常好, 難度適中又可以 split 開 top 的 contestant\
> 在此打下題目的 brief solution, 其中 Elephant 是參巧 FHQ 的做法的\
> \
> **Updated - Elephant official solution**\
> 先把 N 隻大象分成 K 個區間, 每個區間儲著 N/K 隻大象\
> 假設某區間有 p 隻大象 (0, 1, 2, .. p-1), 要儲存:\
> pos\[i]: 大象 i 的位置\
> opt\[i]: 對於這區間, cover 大象 {i, i+1, .., p-1} 最少用多少 camera\
> max\[i]: 在 cover {i, i+1, .., p-1} 時, 最右的 camera cover 到最右的位置\
> \
> 有了這些 information, 可以在 O(K lg N) 搵到最少所需 camera, 做法是由最左的大象開始, 根據 max\[i] 移動, 並 binary search 在下一個 (可以跳過幾個區間) 區間的位置, 再移動\
> 每次 query 都要做, 總時間 O(MK lg N)\
> \
> 每次 update 最多只會影響 2 個區間, 可以用 binary search 找出是哪個區間\
> 只要把受影響區間 O(N/K) recompute 就可以\
> 方法是由右至左 construct, 總時間 O(M \* (lg N + N/K))\
> \
> 但是, 區間的大象數目可能會變得不平均, 所以當一區間的大象數目**跌至 0** 或 **升至 2N/K** 就要把所有大象重新分區間\
> 在整個 process 中最多只會重組 MK/N 次, 所以整個複雜度是 O(MK/N \* N) = O(MK)\
> \
> 可以解得當 K = sqrt(N) 時達至最小值, 所以整個算法複雜度為 O(M \* sqrt(N) \* lg N)\
> \
> **Garden**\
> 用 edge 作為狀態, 建立一新 Graph. 由於行完每條 edge 後行下一條 edge 是固定的, 所以新 Graph 中的每一 vertex 都有一條 outgoing edge, 形成**頂環樹**\
> 對於每一個 query, 計算每一個 node 對應的 start edge 走 G\[i] 步後會到達哪條 edge, 一共是 O(M)\
> 整個算法為 O(MQ)\
> \
> **Race**\
> 和 [POJ 1741](http://poj.org/problem?id=1741) 頗相似, 做法是 Divide and Conquer on Tree\
> 每次選 Center of Tree 作為 root, 用 O(N) 計算它所有 children 的距離和高度, 然後 O(N lg N) 考慮所有經過 root 的 path\
> Recursive 地計算它的 subtree, O(N lg N lg N)\
> \
> **Ricehub**\
> 首先注意到其中一個 center 一定是 hub 選址的其中一個 optimal solution\
> 對於每一個 center, binary search 它可以 cover 的距離 (左右相同) 或 binary search 可以 cover 的 center 的數目\
> \
> 注意: 如果由左到右 sweep line 好像會有問題, 因為 left bound 並不是 monotonic increasing 的\
> \
> **Crocodile**\
> 由 exits 作為起點做 Dijkstra, 當一個 vertex 被第 2 條 edge reach 時才算是 visit 該 vertex\
> \
> **Parrots**\
> 81分做法: 原本的 array 可以看成一條長度為 8N 的 01 string, 只要傳送 1 的位置就可以\
> \>81分: k 最小可以是多少? 其實傳送 kN 個 \[0, 255] 的整數可以產生的可能性為 H(256, kN) = C(255+kN, 255)\
> Solve C(255+kN, 255) ≥ 255N 可以解 k 約是 4.09 (N = 64)\
> \
> 這題的重點就是怎樣可以在有限時間, 空間內做 mapping.\
> 我目前想到最好的方法是, 2 個數字為一組, 用 8 個數字, 傳送 14 次 (k = 7), 可以預先把所有可能性打表 ( H(8, 14) 個可能性 ), 就可以 efficient 做 mapping\
> \
> **Elephant**\
> 首先把問題變成一棵 Tree, 每一個 node 代表一個 position, 定義:\
> 黑點: 每隻大象的位置都是一黑點, 它的 parent 為 position + L 的 node\
> 白點: 它的 parent 為右面或同一位置中最近的 node\
> Root: 在 +INF 的地方\
> \
> 答案就是最左的 node 到 root 的 path 上黑點的數目\
> 每一個 move 都可以拆分為 insert 和 delete\
> \
> Delete(黑點): 把該 node 變成白點 (需要改 parent)\
> Insert(黑點): 在 position + L 建立一點白點, 作為它的 parent\
> Insert(白點): 把 parent 連到在它右面或同一位置最近的點, 黑點優先\
> \
> 注意: 一個 position 可以多於一點, 要避免白點形成 cycle\
> \
> Implement 用到 2 個 data structure: 動態樹 (dynamic tree / link-cut tree) 和 Binary Search Tree
