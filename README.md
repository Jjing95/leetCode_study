# leetCode学习记录
## 目录
   * [4月1日](#4%E6%9C%881%E6%97%A5)
   * [4月2日](#4%E6%9C%882%E6%97%A5)
   * [4月3日](#4%E6%9C%883%E6%97%A5)
   * [4月6日]()
## 4月1日
### 1、139单词拆分
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
### 1、54螺旋矩阵
#### 题目要求
给你一个`m`行`n`列的矩阵`matrix`，请按照**顺时针螺旋顺序**，返回矩阵中的所有元素。
#### 解题思路
我的方法是按照题意进行遍历，对矩阵进行螺旋式的遍历，然后把遍历到的元素加到`list`中输出。原本的想法是按上下左右四条直线写四个循环，但看到题解中用更改方向的做法，代码整体看上去会更简洁点。以下是我结合大家在评论区的解题思路做出的总结：   
1.首先我们把给定数组的第一项拿出来，因为它总是排列在最前面，即`head = matrix.shift()`;   
2.我们再把数组最后一项取出来，即`last = matrix.pop()`，这里需要注意，`last`总是逆序排列;   
3.一个长方形我们已经定了上下两条边，现在就只剩左右两条边了。简单：   
   因为我们刚刚把数组的第一项跟最后一项都删除了，所以现在我们要把剩下的`item`拿出来遍历   
   把剩下的所有`item`的最后一项拿出来排在`head`的后面，`last`的前面   
   把所有`item`的第一项放在`last`的后面，即可得出第一层外壳的排序，剩下的就是重复的循环了   
4.处理边界条件
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
### 2、2两数相加
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
### 1、7整数反转
