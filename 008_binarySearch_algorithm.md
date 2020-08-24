## binarySearch Algorithm

##### 二分搜索模板
- 给一个**有序数组**和目标值，找第一次/最后一次/任何一次出现的索引，如果没有出现返回-1
- **模板四要素**
    - 1、初始化：start=0、end=len-1
    - 2、循环退出条件：start + 1 < end
    - 3、比较中点和目标值：A[mid] ==、 <、> target
    - 4、判断最后两个元素是否符合：A[start]、A[end] ? target
- 时间复杂度 O(logn)，使用场景一般是有序数组的查找
---

## 📑 index
* <a href="#bs">1. binary-search(#704)[典型示例]</a>





---
<div id="bs" onclick="window.location.hash">

#### 1. binary-search(#704)
linkage: [leetcode](https://leetcode-cn.com/problems/binary-search/ "二分查找")
- 给一个有序数组和目标值，如果存在，返回目标索引，否则返回-1


