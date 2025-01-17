## Search Algorithm

- 各种排序算法指标对比

|排序方法 | 平均情况 | 最好情况 | 最坏情况 | 辅助空间 | 稳定性|
| :-: | :-: | :-: | :-: | :-: | :-: |
|简单选择排序 | O(n2) | O(n2) | O(n2) | O(1) | 稳定|
|直接插入排序 | O(n2) | O(n) | O(n2) | O(1) | 稳定|
|希尔排序 | O(nlogn)~O(n2) | O(n1.3) | O(n2) | O(1) | 不稳定|
|冒泡排序 | O(n2) | O(n) | O(n2) | O(1) | 稳定|
|**快速排序** | **O(nlogn)** | O(nlogn) | O(n2) | O(logn)~O(n) | 不稳定|
|**归并排序** | **O(nlogn)** | O(nlogn) | O(nlogn) | O(n) | **稳定**|
|**堆排序** | **O(nlogn)** | O(nlogn) | O(nlogn) | **O(1)** | 不稳定|

- 稳定：如果a原本在b前面，而a=b，排序之后a仍然在b的前面
- 不稳定：如果a原本在b的前面，而a=b，排序之后a可能会在b的后面
-   **归并排序与快排**：
    - 1. 都是分治思想，但分解和合并的策略不一样
    - 2. 归并是从中间分两个，合并后两个数列再次排序
    - 3. 快排是比较后，小的在左大的在右，直接合并不再需要排序
    - 所以**快排比归并排序更高效**一些
---

## 📑 index
- mergeSort & quickSort and heapSort
  * <a href="./005_lists_structure.md">3. [003_lists_structure 5&7] merge-sort</a>
  * <a href="#sc">4. [**很棒的数组mergeSort**] sort-colors(#75)</a>
  * <a href="#kleiaa">5. [**数组quickSort**] kth-largest-element-in-an-array(#215)</a>
  * <a href="#tkfe">6. [**数组heapSort**] top-k-frequent-elements(#347)</a>
  * <a href="#mergeSort">1. [代码框架] mergeSort</a>
  * <a href="#quickSort">2. [代码框架] quickSort</a>
  * <a href="#bubbleSort">7. [代码框架] bubbleSort</a>
  * <a href="#selectSort">8. [代码框架] selectSort</a>
  * <a href="#insertSort">9. [代码框架] insertSort</a>
  * <a href="#shellSort">10. [代码框架] shellSort</a>
  * <a href="#countSort">11. [代码思想] countSort</a>
  * <a href="#bucketSort">12. [代码思想] bucketSort</a>
  * <a href="#radixSort">13. [代码思想] radixSort</a>






[//]: # (Image References)
[image1]: .readme/dfs.png "dfs"
[image2]: .readme/bubble_sort.gif "bubbleSort"
[image3]: .readme/select_sort.gif "selectSort"
[image4]: .readme/merge_sort.gif "mergeSort"
[image5]: .readme/quick_sort.gif "quickSort"
[image6]: .readme/heap_sort.gif "heapSort"
[image7]: .readme/insect_sort.gif "insectSort"
[image8]: .readme/shell_sort.gif "shellSort"
[image9]: .readme/count_sort.gif "countSort-计数排序"
[image10]: .readme/bucket_sort.gif "bucketSort-桶排序"
[image11]: .readme/radix_sort.gif "radixSort-基数排序"



---

<div id="sc" onclick="window.location.hash">

#### 4. [数组mergeSort] sort-colors(#75)
linkage: [leetcode](https://leetcode-cn.com/problems/sort-colors/ "颜色分类")
> n个元素的**数组**，原地对它们进行排序
- 思路一：mergeSort
  - 1. 数组的合并方式，开辟临时变量
  - 2. 将临时变量的数组最终合并到nums
    ```cpp
    class Solution {
    public:
        void sortColors(vector<int>& nums)
        {
            if(nums.size() == 0) return;
            int start = 0;
            int end = nums.size() - 1;
            mergeSort(nums, start, end);
        }
        void mergeSort(vector<int>& nums, int start, int end)
        {
            // 二遍注意：(end - start) >> 1 要加括号！
            int mid = start + ((end - start) >> 1);
            if(end - start > 0)
            {
                mergeSort(nums, start, mid);
                mergeSort(nums, mid + 1, end);
                mergeTwoNums(nums, start, end);
            }
        }
        void mergeTwoNums(vector<int>& nums, int start, int end)
        {
            int length = end - start + 1;
            int mid = start + ((end - start)>>1);
            vector<int> tmp(length);
            int index = 0;
            int lhs = start;
            int rhs = mid +1;
            // 判断两组nums是否越界
            while(lhs <= mid && rhs <= end)
            {
                if(nums[lhs] <= nums[rhs])
                {
                    tmp[index++] = nums[lhs++];
                }
                else
                {
                    tmp[index++] = nums[rhs++];
                }
            }
            // 遍历剩余的数组
            while(lhs <= mid)
            {
                tmp[index++] = nums[lhs++];
            }
            while(rhs <= end)
            {
                tmp[index++] = nums[rhs++];
            }
            // 重点：合并tmp数组到nums中
            for(int i = 0; i < length; i++)
            {
                nums[start + i] = tmp[i];
            }
        }
    };
    ```
---

<div id="kleiaa" onclick="window.location.hash">

#### 5. [**数组quickSort**] kth-largest-element-in-an-array(#215)
linkage: [leetcode](https://leetcode-cn.com/problems/kth-largest-element-in-an-array/ "数组中的第K个最大元素")
> 在未排序的数组中找到第k个最大的元素
- 思路一： quickSort(未优化)
    ```cpp
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k)
        {
            int low = 0;
            int high = nums.size()-1;
            quickSort(nums, low, high);
            return nums[nums.size()-k];
        }

        void quickSort(vector<int>& nums, int low, int high)
        {
            // 2. 尾递归用while，普通情况if判断
            if(high-low > 0)
            {
                int pivot = partition(nums, low, high);
                quickSort(nums, low, pivot - 1);
                quickSort(nums, pivot + 1, high);
            }
        }

        int partition(vector<int>& nums, int low, int high)
        {
            int pivotKey = nums[low];
            while(high-low > 0)
            {
                //从两边向中间逼近
                while(low < high && nums[high] >= pivotKey)
                {
                    high --;
                }
                // 优化不必要的交换
                nums[low] = nums[high];
                while(low < high && nums[low] <= pivotKey)
                {
                    low ++;
                }
                nums[high] = nums[low];
            }
            // 重点：跳出循环时low==high 此时low位置就是pivotKey元素的正确位置
            nums[low] = pivotKey;
            return low;
        }
    };
    ```

- 思路二： quickSort(优化版)
    ```cpp
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k)
        {
            int low = 0;
            int high = nums.size()-1;
            quickSort(nums, low, high);
            return nums[nums.size()-k];
        }
        // 采用最优的quickSort方式
        // 1. 两者中取最短，为了减少递归层数
        // 2. 尾递归方式
        // 3. partition中利用三数取中:选取三个数排序，然后选取中间的数
        // 4. 利用值交换方式代替swap
        void quickSort(vector<int>& nums, int low, int high)
        {
            // 2. 尾递归用while，普通情况if判断
            while(high-low > 0)
            {
                int mid = low + ((high - low)>>1);
                int pivot = partition(nums, low, high);
                // 1. 两者取短 + 尾递归优化，最坏空间复杂度优化至logn
                if(pivot < mid)
                {
                    quickSort(nums, low, pivot - 1);
                    low = pivot + 1;
                }
                else
                {
                    quickSort(nums, pivot + 1, high);
                    high = pivot - 1;
                }
            }
        }

        int partition(vector<int>& nums, int low, int high)
        {
            // 3. 省略三种取中
            int pivotKey = nums[low];
            while(high-low > 0)
            {
                //从两边向中间逼近，注意顺序一定不要写反
                while(low < high && nums[high] >= pivotKey)
                {
                    high -- ;
                }
                nums[low] = nums[high];
                while(low < high && nums[low] <= pivotKey)
                {
                    low ++;
                }
                // 4. 减少不必要的交换
                nums[high] = nums[low];
            }
            // 重点：减少不必要的交换后要还原nums[low]
            nums[low] = pivotKey;
            return low;
        }
    };
    ```
- 思路三： stl::sort库
  - std::sort()内部实现处理形式不同
    - 1. 数据量小时，利用insertSort()
    - 2. 数据量大时，利用quickSort()
    - 3. 栈深度过大时，采用heapSort()
    ```cpp
    sort(nums.begin(),nums.end());
    ```
---

- 思路四： heapSort(**很棒的堆排序思路**)
  - 二叉堆
  - 根节点索引 root / 2;
  - 左子树索引 root * 2 + 1
  - 右子树索引 root * 2 + 2;
- 思路：
  - 以大顶堆为例，分为「**建堆**」,「**调整**」和「**删除**」，如下图
![][image6]
  - 一. 构建堆
    - 找到根节点进行遍历
  - 二. 调整堆
    - 1. 找到根节点和其左右子树比较，找到最大的索引
    - 2. 如果子树小于根节点，那么进行值交换
    - 3. 进行向上迭代重复操作
  - 三. 删除堆(大顶堆的根节点一定是最大值)
    - 1. 进行循环删除的过程
    - 2. 在删除将最后一个值赋值到第一个
    - 3. 将heapSize减一
    - 4. 将最大堆进行调整,调用步骤二
    ```cpp
    class Solution {
    public:
        int findKthLargest(vector<int>& nums, int k)
        {
            int heap_size = nums.size();
            // 1. 建立堆
            buildHeap(nums,heap_size);
            // 3. 删除堆
            // 注意：1. index的>=，而非<= 2. for循环注意使用nums.size()，而非heap_size
            for(int index = nums.size()-1; index >= nums.size()-k+1;　index--)
            {
                swap(nums[0],nums[index]);
                heap_size--;
                // 重点：因为交换了nums[0],对顶堆0进行调整
                adjustMaxHeap(nums,0,heap_size);
            }
            return nums[0];
        }

        void buildHeap(vector<int>& nums, int heap_size)
        {
            for(int root_index = heap_size/2;root_index >= 0;root_index--)
            {
                // 2. 调整堆
                adjustMaxHeap(nums, root_index,heap_size);
            }
        }

        void adjustMaxHeap(vector<int>& nums, int root_index,int heap_size)
        {
            int largest_index = root_index;
            int left_index = root_index*2+1;
            int right_index = root_index*2+2;
            // 注意：nums[largest_index]中不能写root_index
            if(left_index<heap_size && nums[largest_index]<nums[left_index])
            {
                largest_index = left_index;
            }
            if(right_index<heap_size && nums[largest_index]<nums[right_index])
            {
                largest_index = right_index;
            }
            if(largest_index != root_index)
            {
                swap(nums[root_index],nums[largest_index]);
                // 交换后largest_index为root的索引，进行largest_index递归
                adjustMaxHeap(nums,largest_index,heap_size);
            }
        }
    };
    ```
---

<div id="tkfe" onclick="window.location.hash">

#### 6. [**数组heapSort**] top-k-frequent-elements(#347)
linkage: [leetcode](https://leetcode-cn.com/problems/top-k-frequent-elements/ "前K个高频元素")
> 非空整数数组，返回其中出现频率前k高的元素
- 时间复杂度必须优于O(nlogn)
- 思路：hashmap
    ```cpp
    class Solution {
    public:
        vector<int> topKFrequent(vector<int>& nums, int k) 
        {
            // priority_queue<Type, Container, Functional>
            std::unordered_map<int,int> counter;
            std::vector<int> res;
            // 添加元素并计数
            for(auto x: nums)
            {
                counter[x]++;
            }
            vector<pair<int, int>> tmp;
            for (auto pair : counter)
            {
                tmp.push_back(make_pair(pair.second, pair.first));
            }
            sort(tmp.begin(),tmp.end());
            for(int i = 0; i < k; i++)
            {
                res.push_back(tmp[tmp.size()-1-i].second);
            }
            return res;
        }
    };
    ```
---

<div id="bubbleSort" onclick="window.location.hash">

#### 7. [代码框架] bubbleSort
- 排序思想：
  - 传统思路
    - 两两比较相邻的关键字，反序进行交换，如下图
![][image2]
    - 但这样排序效率较为低效
        ```cpp
        void bubbleSort(vector<int>& nums)
            {
                for(int i = 0; i <= nums.size()-2;i++)
                {
                    for(int j =i+1;j<=nums.size()-1;j++)
                    {
                        if(nums[i]>nums[j])
                            swap(nums[i],nums[j]);
                    }
                }
            }
        ```
    
  - 改进一：
    - 内层遍历从后向前
    - 比较前者大于后者进行值交换
    ```cpp
    void bubbleSort(vector<int>& nums)
    {
        for(int i = 0; i <= nums.size()-2;i++)
        {
            for(int j =nums.size()-1;j>i;j--)
            {
                // 比较前者大于后者进行值交换
                if(nums[j-1]>nums[j])
                    swap(nums[j-1],nums[j]);
            }
        }
    }
    ```
  - 改进二：
    - 利用flag标记，有数据交换进行排序
    - 针对部分有序
    ```cpp
    void bubbleSort(vector<int>& nums)
    {
        bool flag = true;
        // 注意flag的放置条件
        for(int i = 0; i <= nums.size()-2 && flag;i++)
        {
            flag = false;
            for(int j = nums.size()-1;j>i;j--)
            {
                if(nums[j-1] > nums[j])
                {
                    swap(nums[j-1], nums[j]);
                    // 如果后面每一个元素都大于前面，则部分有序，提高效率
                    flag = true;
                }
            }
        }
    }
    ```
---

<div id="selectSort" onclick="window.location.hash">

#### 8. [代码框架] selectSort
- 排序思想：
  - 通过当前值与其他值比较
  - 找到最小值的索引
  - 如果索引非当前索引，进行值交换，如下图
![][image3]
    ```cpp
    void selectSort(vector<int>& nums)
        {
            for(int i = 0; i <= nums.size()-2;i++)
            {
                int min_index = i;
                for(int j =nums.size()-1;j>i;j--)
                {
                    if(nums[min_index]>nums[j])
                    {
                        min_index = j;
                    }
                }   
                if(i != min_index)
                {
                    swap(nums[i],nums[min_index]);
                }
            }
        }
    ```
---

<div id="insertSort" onclick="window.location.hash">

#### 9. [代码框架] insertSort
- 排序思想：
  - 将数字直接插入到有序的数组中，如下图
![][image7]
    ```cpp
    void insectSort(vector<int>& nums)
        {
            for(int i = 0; i <= nums.size()-2;i++)
            {
                // 定义j为后一个值，j的索引为1
                for (int j = i+1; j >= 1 && nums[j] < nums[j-1]; j--)
                {
                    swap(nums[j-1],nums[j]);
                }
            }
        }
    ```
---

<div id="shellSort" onclick="window.location.hash">

#### 10. [代码框架] shellSort
- 排序思想：
  - 分割成为若干子序列分别进行直接插入排序，见下图
![][image8]
  - 1. 分组，每个一定间隔的数为一组
  - 2. 组内排序
  - 3. 不断循环1和2，知道分组间隔变为1
  - 4. 运用插入排序
  - 增量的选取，i=i/3+1 时的复杂度为O(n^1.5),好于i=i/2 其复杂度为O(n^2)
    ```cpp
    void shellSort(vector<int>& nums)
    {
        int increment = nums.size();
        while (increment > 1)
        {
            // 增量的选取
            increment = increment / 3 + 1;
            for (int i = increment; i < nums.size(); i++)
            {
                int temp = nums[i];
                if (nums[i] < nums[i - increment])
                {
                    int j;
                    for (j = i - increment; j >= 0 && nums[j] > temp; j = j - increment)
                        nums[j + increment] = nums[j];
                    nums[j + increment] = temp;
                }
            }
        }
    }
    ```
---

<div id="countSort" onclick="window.location.hash">

#### 11. [代码思想] countSort(计数排序)
- 排序思想：
  - 不是基于比较排序算法
  - 存储数据需要**额外开辟空间**
  - 输入的数据必须是**有确定范围**的整数
  - 当输入的元素是n个[0,k]，时间/空间复杂度均是O(n+k)，如下图
![][image9]
---

<div id="bucketSort" onclick="window.location.hash">

#### 11. [代码思想] bucketSort(桶排序)
- 排序思想：
  - 基于计数的排序算法
  - 数据分到**有限数量桶**里，每个桶再分别排序
  - 非空桶排好序的数据拼接，
  - 桶划分的越小，各个桶之间的数据越少，排序所用的时间也会越少
  - 如下图
![][image10]
---

<div id="radixSort" onclick="window.location.hash">

#### 11. [代码思想] radixSort(基数排序)
- 排序思想：
  - 按照低位先排序，然后收集
  - 再按照高位排序，然后再收集
  - 依次类推，直到最高位
  - 基数排序的性能比桶排序要略差，如下图
![][image11]

<div id="mergeSort" onclick="window.location.hash">

#### 1. [代码框架] mergeSort
- 排序思想：
  1. 将序列一分为二，对两边进行递归分割操作，得到独立子序列
  2. 对独立子序列递归的执行合并操作，得到有序的序列,如下图
![][image4]
- python版本
    ```python
    def merge(left, right):
        l = 0
        r = 0
        m = []
        while l < len(left) and r < len(right):
            if left[l] <= right[r]:
                m.append(left[l])
                l += 1
            else:
                m.append(right[r])
                r += 1
        remains = right[r:] if l == len(left) else left[l:]
        m.extend(remains)
        return m

    def merge_sort(arr):
        if len(arr) <= 1:
            return arr
        middle = len(arr) // 2
        left = merge_sort(arr[:middle])
        right = merge_sort(arr[middle:])
        return merge(left, right)
    ```
---

<div id="quickSort" onclick="window.location.hash">

#### 2. [代码框架] quickSort
- 排序思想：
  - 基于分治思想的不稳定排序(基准前后放在一边)
  - 快速排序缺点：**对小规模数据性能不是很好**
  - 快排是原地交换，无合并过程传入的索引是存在的索引（如：0、length-1等），**越界可能导致崩溃**,如下图
![][image5]
- python版本
    ```python
    def quick_sort(b):
        """快速排序"""
        if len(b) < 2:
            return arr
        # 选取基准，随便选哪个都可以，选中间的便于理解
        pivot = arr[len(b) // 2]
        # 定义基准值左右两个数列
        left, right = [], []
        # 从原始数组中移除基准值
        b.remove(pivot)
        for item in b:
            # 大于基准值放右边
            if item >= pivot:
                right.append(item)
            else:
                # 小于基准值放左边
                left.append(item)
        # 使用迭代进行比较
        return quick_sort(left) + [pivot] + quick_sort(right)
    ```
---
