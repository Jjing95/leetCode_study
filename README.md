# leetCode学习记录
## 目录
   * [4月1日](#4%E6%9C%881%E6%97%A5)
   * [4月2日](#4%E6%9C%882%E6%97%A5)
   * [4月3日](#4%E6%9C%883%E6%97%A5)
   * [4月6日](#4%E6%9C%886%E6%97%A5)
   * [4月7日](#4%E6%9C%887%E6%97%A5)
   * [4月8日](#4%E6%9C%888%E6%97%A5)
   * [4月11日](#4%E6%9C%8811%E6%97%A5)
   * [4月12日](#4%E6%9C%8812%E6%97%A5)
   * [4月13日](#4%E6%9C%8813%E6%97%A5)
   * [4月14日](#4%E6%9C%8814%E6%97%A5)
   * [4月15日](#4%E6%9C%8815%E6%97%A5)
## 4月1日
### 1、[139单词拆分](https://leetcode-cn.com/problems/word-break/)
#### 题目要求
给你一个字符串`s`和一个字符串列表`wordDict`作为字典。请你判断是否可以利用字典中出现的单词拼接出`s`。   

> 注意：不要求字典中出现的单词全部都使用，并且字典中的单词可以重复使用。
#### 解题思路
用`dp[j]`来记录`j`长度内的字符串是否匹配 ，准确地说是，能否将`s`的前`j`个字符拆分成`wordDict`中有的元素，再通过下面的公式，先遍历字符串容量后遍历字符串列表，得到结果。
```javascript
// s 的 [j - wordDict[i].length, j] 与 wordDict[i] 是否匹配
// s 的 [0, j - wordDict[i].length] 能否用 wordDict 的元素匹配
if(s.slice(j - wordDict[i].length, j) === wordDict[i] && dp[j - wordDict[i].length]) {
    dp[j] = true;
}
```
#### 正解
先遍历字符串`s`容量后遍历字符串列表`wordDict`
```javascript
var wordBreak = function(s, wordDict) {
    // dp[j] j 长度内字符串是否匹配
    let dp = Array(s.length + 1).fill(false);
    dp[0] = true;
    for(let j = 0; j <= s.length; j++) { // 遍历字符串
        for(let i = 0; i < wordDict.length; i++) { // 遍历字符串列表
            if(j >= wordDict[i].length) {
                // s 的 [j - wordDict[i].length, j] 与 wordDict[i] 是否匹配
                // s 的 [0, j - wordDict[i].length] 能否用 wordDict 的元素匹配
                if(s.slice(j - wordDict[i].length, j) === wordDict[i] && dp[j - wordDict[i].length]) 
                    dp[j] = true;
            }
        }
    }
    return dp[s.length];
};
```
#### 解题过程中遇到的坑
如果先遍历字符串列表`wordDict`后遍历字符串`s`容量
```javascript
// 只改了for循环顺序版本（会出现错误）
var wordBreak = function(s, wordDict) {
    // dp[j] j 长度内字符串是否匹配
    let dp = Array(s.length + 1).fill(false);
    dp[0] = true;
    for(let i = 0; i < wordDict.length; i++) { // 遍历物品
        for(let j = wordDict[i].length; j <= s.length; j++) { // 遍历背包容量
            if(j >= wordDict[i].length) {
                // s 的 [j - wordDict[i].length, j] 与 wordDict[i] 是否匹配
                // s 的 [0, j - wordDict[i].length] 能否用 wordDict 的元素匹配
                if(s.slice(j - wordDict[i].length, j) === wordDict[i] && dp[j - wordDict[i].length])
                    dp[j] = true;
            }
        }
    }
    return dp[s.length];
};

```
> 错后思考：起初一直无法理解怎么代码换一种方式去解题就一直错误，后来查阅了在[leetCode](https://leetcode-cn.com/problems/word-break/comments/)上大家的评论才知道**如果要是外层for循环遍历物品，内层for遍历背包，就需要把所有的子串都预先放在一个容器里。**
<!-- 
#### 题目要求
#### 解题思路
#### 正解
#### 解题过程中遇到的坑 -->

## 4月2日
### 1、[54螺旋矩阵](https://leetcode-cn.com/problems/spiral-matrix/)
#### 题目要求
给你一个`m`行`n`列的矩阵`matrix`，请按照**顺时针螺旋顺序**，返回矩阵中的所有元素。
#### 解题思路
我的方法是按照题意进行遍历，对矩阵进行螺旋式的遍历，然后把遍历到的元素加到`list`中输出。原本的想法是按上下左右四条直线写四个循环，但看到题解中用更改方向的做法，代码整体看上去会更简洁点。以下是我结合大家在评论区的解题思路做出的总结：   
1. 首先我们把给定数组的第一项拿出来，因为它总是排列在最前面，即`head = matrix.shift()`;   
2. 我们再把数组最后一项取出来，即`last = matrix.pop()`，这里需要注意，`last`总是逆序排列;   
3. 一个长方形我们已经定了上下两条边，现在就只剩左右两条边了。简单：   
   因为我们刚刚把数组的第一项跟最后一项都删除了，所以现在我们要把剩下的`item`拿出来遍历   
   把剩下的所有`item`的最后一项拿出来排在`head`的后面，`last`的前面   
   把所有`item`的第一项放在`last`的后面，即可得出第一层外壳的排序，剩下的就是重复的循环了   
4. 处理边界条件
#### 正解
```javascript
var spiralOrder = function (matrix) {
    const len = matrix.length;
    if (len === 0) return [];
    if (len === 1) return matrix[0];
    let res = [];
    while (matrix.length) {
        if (matrix.length >= 2) {
            // 取出第一项
            const head = matrix.shift();
            // 取最后一项
            const last = matrix.pop();
            // 长方形右边框
            const lastArr = [];
            // 长方形左边框
            const headArr = [];
            for (let i = 0; i < matrix.length; i++) {
                const l = matrix[i].length;
                if (l > 1) {
                  lastArr.push(matrix[i].pop());
                  headArr.unshift(matrix[i].shift());
                } else if (l === 1) {
                  lastArr.push(matrix[i].pop());
                }
            }
            res = [].concat(res, head, lastArr, last.reverse(), headArr);
          } else {
              const last = matrix.shift();
              last && (res = [].concat(res, last));
          }
      }
    return res;
};
```
#### 解题过程中遇到的坑
>这里有一个我觉得很容易错的地方，那就是`while`内层的`for`循环要添加垂直方向的边界判断，要不然在遍历非`n*n·矩阵的时候会有重复遍历的情况
### 2、[2两数相加](https://leetcode-cn.com/problems/add-two-numbers/)
>有人相爱，有人夜里开车看海，有人LeetCode第二题也做不出来...
#### 题目要求
给你两个**非空**的链表，表示两个非负的整数。它们每位数字都是按照**逆序**的方式存储的，并且每个节点只能存储**一位**数字。   
请你将两个数相加，并以相同形式返回一个表示和的链表。   
你可以假设除了数字**0**之外，这两个数都不会以**0**开头。
#### 解题思路
这题其实我思考了很久，最后还是结合了[leetCode](https://leetcode-cn.com/problems/word-break/comments/)中大家的评论，才理清了思路。
1. 由于结果也是逆序返回的，所以需要两个变量，一个存储当前节点`item`，一个存储最终返回节点`final`，防止节点丢失被垃圾回收了，由于两数相加可能会出现进`1`的情况，定义变量`a`存储，这实际用开关模拟也行
2. 由于两个链表的位数不确定一致所以`while`两个数据都为`false`才停止
3. 两个节点取值，没有的节点给`0`，跟上一层节点的可能出现进`1`的变量计算当前节点总和`a`
4. 将`l1`和`l2`节点进入下一层级
5. 创建最终求和后的节点，赋值给当前`item`的`next`属性，然后计算是否需要进`1`，重置`a`的值
6. 重置`item`为刚才创建的节点
7. 计算完`l1`和`l2`所有数据，最后`a`仍然可能进`1`，所以判断如果进1的话最后再拼接一个节点上去
8. 需要注意一下，起始给的`final`绑定的数据就是一个空对象，并不是一个真实的数据节点，`l1+l2`的第一个节点的数据存在了第一个`item`，也就是`final`的`next`属性里，这里只是为了方便才这么处理，因此需要返回`final.next`
#### 正解
```javascript
var addTwoNumbers = function(l1, l2) {
    // 1
    let item = {}
    let final = item
    let a = 0
    // 2
    while (l1 || l2) {
        // 3
        let l = l1?.val ?? 0
        let r = l2?.val ?? 0
        a += (l + r)
        // 4
        l1 = l1?.next
        l2 = l2?.next
        // 5
        item.next = {val: a%10, next: null}
        a = ~~(a / 10)
        // 6
        item = item.next
    }
    // 7
    if (a) {
        item.next = { val: a, next: null}
    }
    // 8
    return final.next
};
```
#### 解题过程中遇到的坑
起初我是以下面的解题过程去解决这个问题的，但是调试没出现问题，提交的时候出错了。原因是不能直接用.val获取数值。
```javascript
var addTwoNumbers = function(l1, l2) {
    var res = new ListNode(0)
    // now存放计算值
    var now = res
    // 当任一节点存在时
    while(l1 || l2 || now){
        // a的当前值
        var a = l1.val
        // b的当前值
        var b = l2.val
        // 两个数组相加之和
        var sum = a+b+now.val
        // 当前结果节点的值
        now.val = sum%10
        // a与b寻找下一个节点
        l1 = l1.next
        l2= l2.next
        // 当计算值大于9时
        if(sum > 9){
            // 下一个节点数据带1（进位）
            now.next = new ListNode(1)
        }else if(l1 || l2) {
            // 不进位情况
            now.next = new ListNode(0)
        }
        // 结果节点寻找下一个节点
        now = now.next
    }
    return res
};
```
## 4月3日
### 1、[7整数反转](https://leetcode-cn.com/problems/reverse-integer/)
#### 题目要求
给你一个 32 位的有符号整数 x ，返回将 x 中的数字部分反转后的结果。   
如果反转后整数超过 32 位的有符号整数的范围 `[−231,  231 − 1]` ，就返回 0。   
**假设环境不允许存储 64 位整数（有符号或无符号）。**
#### 解题思路
1. 先将数字转为字符串，然后切割成为数组，再反转数组
2. 判断入参的正负值，根据正负值处理答案正负值
3. 判断限制条件
#### 正解
```javascript
var reverse = function(x) {
    const symbol = String(x).split("").reverse().join("")
    let result;
    if(x>=0){
        result =  Number(symbol) 
    }else{
        result = Number(symbol.slice(-1)+symbol.slice(0,-1)) 
    }
    if(result < (-2)**31 || result > (2**31 -1)){
        result = 0
    }
    return result
};
```
## 4月6日
### 1、[390消除游戏](https://leetcode-cn.com/problems/elimination-game/)
#### 题目描述
列表**arr**由在范围 **[1, n]** 中的所有整数组成，并按严格递增排序。请你对**arr**应用下述算法：   
从左到右，删除第一个数字，然后每隔一个数字删除一个，直到到达列表末尾。   
重复上面的步骤，但这次是从右到左。也就是，删除最右侧的数字，然后剩下的数字每隔一个删除一个。   
不断重复这两步，从左到右和从右到左交替进行，直到只剩下一个数字。   
给你整数**n**，返回**arr**最后剩下的数字。
#### 解题思路
根据题意可知，消除的总轮次为**Math.log2(n)**，每轮消除后剩下的数字都构成等差数列。   
因此可以借助最小值**min**、最大值**max**、步长**step**3个变量来维护等差数列，每轮都更新等差数列的3个变量；最后一轮只剩一个数字，即**min**与**max**相等，任取一个返回即可。   
需要注意的是奇数轮次是从左到右消除，即最小值必定改变，最大值只在等差数列个数为奇数时改变；偶数轮次则相反，最大值必定改变，最小值只在等差数列个数为奇数时改变。
#### 正解
```javascript
var lastRemaining = function (n) {
    let min = 1, max = n, step = 1;
    for (let i = 1; i <= Math.log2(n); i++) {
        if (i % 2) {
            if (((max - min) / step + 1) % 2) max -= step;
            min += step;
        } else {
            if (((max - min) / step + 1) % 2) min += step;
            max -= step;
        }
        step *= 2;
    }
    return max;
};
```
## 4月7日
### 1、[3无重复字符的最长子串](https://leetcode-cn.com/problems/longest-substring-without-repeating-characters/)
#### 题目描述
给定一个字符串**s**，请你找出其中不含有重复字符的**最长子串**的长度。
#### 解题思路
整体思路就是左指针不动，右指针在没遇到重复字符时一直往右移动，如果遇到重复字符了就重新开始记录，即左指针移动到最近出现的一个重复字符处，重新开始记录。   
滑动窗口用来记录最长不出现重复字符的子串的区间   
哈希表用来记录每个字符最后（最新）出现的索引
#### 正解
```javascript
var lengthOfLongestSubstring = function(s) {
    let left = 0;
    let long = 0;
    const map = new Map(), len = s.length;
    for(let right = 0; right < len; right++){
        // 遇到重复字符时还要判定 该重复字符的上一次出现位置是否在 滑动窗口左边界 left 的右边
        if(map.has(s[right]) && map.get(s[right]) >= left){
            left = map.get(s[right]) + 1; // 都满足，则更新，更新到最近出现的那个重复字符，它的上一个索引的右边
        }
        long = Math.max(long, right - left + 1); // 比较滑动窗口大小与 long 的长度
        map.set(s[right], right); // 无论有没有重复，每次遍历都要更新字符的出现位置
    }
    return long;
};
```
## 4月8日
### 1、[36有效的数独](https://leetcode-cn.com/problems/valid-sudoku/submissions/)
#### 题目描述
请你判断一个**9 x 9**的数独是否有效。只需要根据以下规则 ，验证已经填入的数字是否有效即可。   
数字**1-9**在每一行只能出现一次。   
数字**1-9**在每一列只能出现一次。   
数字**1-9**在每一个以粗实线分隔的**3x3**宫内只能出现一次。   

>**注意：**
>一个有效的数独（部分已被填充）不一定是可解的。
>只需要根据以上规则，验证已经填入的数字是否有效即可。
>空白格用 **'.'**表示。
#### 解题思路
给每一行、每一列、每个3x3子数组定义一个键值对 **Map**，之后遍历数组，若单个键值对中出现重复数字返回**false**。
#### 正解
```javascript
var isValidSudoku = function(board) {
let rows = []
let cols = []
let box = []
for (let i = 0; i < 9; i++) {
    rows[i] = new Map()
    cols[i] = new Map()
    box[i] = new Map()
}
for (let i = 0; i < board.length; i++) {
    for (let j = 0; j < board[i].length; j++) {
        if (board[i][j] !== '.') {
            // 获取数字所在子数组的序号
            let s = parseInt(i / 3) * 3 + parseInt(j / 3) 
            if (rows[i].has(board[i][j]) || cols[j].has(board[i][j]) || box[s].has(board[i][j]))
                return false
            else {
                rows[i].set(board[i][j], 1)
                cols[j].set(board[i][j], 1)
                box[s].set(board[i][j], 1)
            }
        }
    }
}
return true
};
```
## 4月11日
### 1、[面试题17.09第k个数](https://leetcode-cn.com/problems/get-kth-magic-number-lcci/)
#### 题目描述
有些数的素因子只有**3，5，7**，请设计一个算法找出第**k**个数。注意，不是必须有这些素因子，而是必须不包含其他的素因子。例如，前几个数按顺序应该是**1，3，5，7，9，15，21**。
#### 解题思路
按照题意这个数必然可以拆解成**3^n * 5^m * 7^l**，  
记录**m ,l ,n**的最大值 , 下一个仍然是可以拆分出新的**m ,l, n**;   
**m, l, n**就代表， 对应因子出现的次数   
所以下一个数，也能表示为之前的数乘**3**或**5**或**7**。   
从**1**开始，下一个数字就在**1 * 3, 1 * 5, 1 * 7**这三个数字里选，   
这样**3**就出现过一次了，如果下一次还出现**3**这个因子，肯定不可能还是与**1**相乘， 只能与下一位**3**相乘， 而**5，7**还是与**1**相乘，因为还没出现过。   
因子出现一次，对应的记号就 **+1**。
#### 正解
```javascript
var getKthMagicNumber = function(k) {
    let [k3,k5,k7]= [0,0,0], fn = [1];

    while(fn.length < k){
        // let next = Math.pow(3,k3)* Math.pow(5, k5)*Math.pow() * 3;
        let next = fn[k3]* 3 ;
        next  = Math.min(next,fn[k5]* 5 )
        next  = Math.min(next,fn[k7]* 7 )
        if(fn[k3] * 3 === next){ k3++}
        if(fn[k5] * 5 === next){ k5++}
        if(fn[k7] * 7 === next){ k7++}
        fn.push(next)
    }
    return fn[k-1]
};
```
>***注意***：**k3, k5, k7**分别代表**3 5 7**因子在之前的数里面出现的最多的次数
>也就是该因子 已经出现了多少次
>当前数如果 是之前的某个数乘以该因子 则该因子的指针进一
## 4月12日
### 1、[1637两点之间不包含任何点的最宽垂直面积](https://leetcode-cn.com/problems/widest-vertical-area-between-two-points-containing-no-points/)
#### 题目描述
给你`n`个二维平面上的点`points`，其中`points[i] = [xi, yi]`，请你返回两点之间内部不包含任何点的 最宽垂直面积的宽度。   
垂直面积的定义是固定宽度，而**y**轴上无限延伸的一块区域（也就是高度为无穷大）。最宽垂直面积为宽度最大的一个垂直面积。   
请注意，垂直区域边上的点不在区域内。
#### 解题思路
不用考虑**y**轴，直接循环把**x**轴拿出来排序，循环拿到最大相邻间隔
#### 正解
```javascript
var maxWidthOfVerticalArea = function(points) {
    let arr =[]
    // 循环拿到x轴数据
    for(let i =0;i<points.length;i++) {
        arr.push(points[i][0])
    }
    // 对x轴数据进行排序
    arr.sort((a,b) => a-b)
    let max = 0
    // 循环拿到最大响铃间隔
    for(let j=1;j<arr.length;j++) {
        max = Math.max(max,arr[j]-arr[j-1])
    }
    return max
}
```
## 4月13日
### 1、[310最小高度树](https://leetcode-cn.com/problems/minimum-height-trees/)
#### 题目描述
树是一个无向图，其中任何两个顶点只通过一条路径连接。换句话说，一个任何没有简单环路的连通图都是一棵树。   
给你一棵包含`n`个节点的树，标记为`0`到`n - 1`。给定数字`n`和一个有`n - 1`条无向边的`edges`列表（每一个边都是一对标签），其中`edges[i] = [ai, bi]`表示树中节点`ai`和`bi`之间存在一条无向边。   
可选择树中任何一个节点作为根。当选择节点**x**作为根节点时，设结果树的高度为**h**。在所有可能的树中，具有最小高度的树（即，min(h)）被称为**最小高度树**。   
请你找到所有的**最小高度树**并按**任意顺序**返回它们的根节点标签列表。
#### 解题思路
>思路来自力扣官方解答[leetcode](https://leetcode-cn.com/problems/minimum-height-trees/solution/zui-xiao-gao-du-shu-by-leetcode-solution-6v6f/)   
>比较理解思路二中的深度优先搜索方法   

   
首先找到距离节点**00**的最远节点**xx**，然后找到距离节点**xx**的最远节点**yy**，然后找到节点**xx**与节点**yy**的路径，然后找到根节点。
#### 正解
```javascript
var findMinHeightTrees = function(n, edges) {
    const ans = [];
    if (n === 1) {
        ans.push(0);
        return ans;
    }
    const adj = new Array(n).fill(0).map(() => new Array());
    for (const edge of edges) {
        adj[edge[0]].push(edge[1]);
        adj[edge[1]].push(edge[0]);
    }

    const parent = new Array(n).fill(-1);
    /* 找到与节点 0 最远的节点 x */
    let x = findLongestNode(0, parent, adj);
    /* 找到与节点 x 最远的节点 y */
    let y = findLongestNode(x, parent, adj);
    /* 求出节点 x 到节点 y 的路径 */
    const path = [];
    parent[x] = -1;
    while (y !== -1) {
        path.push(y);
        y = parent[y];
    }
    const m = path.length;
    if (m % 2 === 0) {
        ans.push(path[Math.floor(m / 2) - 1]);
    }
    ans.push(path[Math.floor(m / 2)]);
    return ans;
}

const findLongestNode = (u, parent, adj) => {
    const n = adj.length;
    const dist = new Array(n).fill(-1);
    dist[u] = 0;

    const dfs = (u, dist, parent, adj) => {
        for (const v of adj[u]) {
            if (dist[v] < 0) {
                dist[v] = dist[u] + 1;
                parent[v] = u;
                dfs(v, dist, parent, adj); 
            }
        }
    }

    dfs(u, dist, parent, adj);
    let maxdist = 0;
    let node = -1;
    for (let i = 0; i < n; i++) {
        if (dist[i] > maxdist) {
            maxdist = dist[i];
            node = i;
        }
    }
    return node;
}
```
## 4月14日
### 1、[689划分为k个相等的子集](https://leetcode-cn.com/problems/partition-to-k-equal-sum-subsets/)
#### 题目描述
给定一个整数数组**nums**和一个正整数**k**，找出是否有可能把这个数组分成**k**个非空子集，其总和都相等。
#### 解题思路
**从n个数字的视角**
1. 以数字的视角，选择**k**个桶
2. 尽可能多的情况命中剪枝的那个**if**分支，就可以减少递归调用的次数，一定程度上减少时间复杂度
3. 如何尽可能多的命中这个**if**分支呢？要知道我们的**index**参数是从**0**开始递增的，也就是递归地从**0**开始遍历**nums**数组
4. 如果我们提前对**nums**数组排序，把大的数字排在前面，那么大的数字会先被分配到**bucket**中，对于之后的数字，`bucket[i] + nums[index]`会更大，更容易触发剪枝的**if**条件
5. 这种解法即便经过了排序优化，也明显比第二种解法慢很多，也就是从数字的角度进行穷举，**n**个数字，每个数字有**k**个桶可供选择，所以组合出的结果个数为`k^n`，时间复杂度也就是`O(k^n)`
**我们应该尽量「少量多次」，就是说宁可多做几次选择，也不要给太大的选择空间；宁可「二选一」选k次，也不要 「k选一」选一次**
#### 正解
```javascript
var canPartitionKSubsets = function (nums, k) {
    // 排除一些基本情况
    if (k > nums.length) return false;
    let sum = nums.reduce((pre, cur) => pre + cur);
    if (sum % k != 0) return false;
    /**
      * 递归穷举 nums 中的每个数字
      * @param {*} nums
      * @param {*} index 开始索引
      * @param {*} bucket 桶
      * @param {*} target 每个桶都需要达成的目标和
    */
    const backtrack = (nums, index, bucket, target) => {
        // 检查所有桶的数字之和是否都是 target
        if (index == nums.length) {
            for (let i = 0; i < bucket.length; i++) {
                if (bucket[i] != target) {
                    return false;
                }
            }
            // nums 成功平分成 k 个子集
            return true;
        }
        // 穷举 nums[index] 可能装入的桶
        for (let i = 0; i < bucket.length; i++) {
            // 剪枝，桶装装满了
            if (bucket[i] + nums[index] > target) {
                continue;
            }
            // 将 nums[index] 装入 bucket[i]
            bucket[i] += nums[index];
            // 递归穷举下一个数字的选择
            if (backtrack(nums, index + 1, bucket, target)) {
                return true;
            }
            // 撤销选择
            bucket[i] -= nums[index];
        }
        // nums[index] 装入哪个桶都不行
        return false;
    };
    // k 个桶（集合），记录每个桶装的数字之和
    let bucket = new Array(k).fill(0);
    // 理论上每个桶（集合）中数字的和
    let target = sum / k;
    /* 降序排序 nums 数组   优化方式，以期尽快的命中剪枝*/
    nums.sort((a, b) => b - a);
    // 穷举，看看 nums 是否能划分成 k 个和为 target 的子集
    return backtrack(nums, 0, bucket, target);
};
```
## 4月15日
### 1、[前序遍历构造二叉搜索树](https://leetcode-cn.com/problems/construct-binary-search-tree-from-preorder-traversal/)
#### 题目描述
给定一个整数数组，它表示**BST**(即**二叉搜索树**)的**先序遍历**，构造树并返回其根。   
保证对于给定的测试用例，总是有可能找到具有给定需求的二叉搜索树。   
**二叉搜索树**是一棵二叉树，其中每个节点，`Node.left`的任何后代的值 严格小于`Node.val`,`Node.right`的任何后代的值**严格大于** `Node.val`。   
二叉树的**前序遍历**首先显示节点的值，然后遍历`Node.left`，最后遍历`Node.right`。
#### 解题思路
1. 生成二叉树的题目，无非就是先生成根节点，然后递归生成左右子树，最后把根节点和左右子树连接起来。具体区别在于你如何找到根节点，如何划分左右子树。
2. 根据前序遍历的特点是，根节点在第一位，后面接着左子树和右子树；
3. **BST**的特点，左子树都比根节点的值小，右子树都比根节点的值大。
4. 所以如何找到根节点？前序遍历的第一个就是根节点。
5. 如何找到左右子树？比根节点小的就是左子树的节点，比根节点大的就是右子树的节点。
6. 最后，确定清楚**build**函数的定义，利用这个函数递归生成**BST**即可。
#### 正解
```javascript
var bstFromPreorder = function (preorder) {
    const build = (preorder, start, end) => {
        if (start > end) return null;
        // 根据前序遍历的特点，根节点在第一位，后面接着左子树和右子树
        let rootVal = preorder[start];
        let root = new TreeNode(rootVal);
        // 根据 BST 的特点，左子树都比根节点的值小，右子树都比根节点的值大
        // rootIndex 就是左右子树的分界点
        let rootIndex = start;
        while (rootIndex <= end) {
            if (preorder[rootIndex] > preorder[start]) break;
            rootIndex++;
        }
        // [start+1, rootIndex-1] 区间内是左子树元素
        root.left = build(preorder, start + 1, rootIndex - 1);
        // [rootIndex, end] 区间内是右子树元素
        root.right = build(preorder, rootIndex, end);
        return root;
    };
    // 前闭后闭区间
    return build(preorder, 0, preorder.length - 1);
};
```
