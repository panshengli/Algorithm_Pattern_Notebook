## Stack and Queue

- Stack可以临时保存一些数据，之后用到依次再弹出来，常用于 DFS 深度搜索
- Queue一般常用于 BFS 广度搜索，类似一层一层的搜索
- 各种数据结构操作元素方法
    数据结构 | 向后面压入<br>一个元素 | 删除最后<br>一个元素 | 返回头元素 | 返回尾元素 | 入栈(队) | 出栈(队) | 判断空
    :-: | :-: | :-: | :-: | :-: | :-: | :-: | :-:
    vector | push_back() | pop_back()<br>emplace_back() | at(0) | at(size(i)-1) | -- | -- | empty()
    stack | -- | -- | top()<br>peek() | -- | push()<br>emplace() | pop() [删除尾] | empty()
    queue | -- | -- | front() | back() | push()<br>emplace() | pop() [删除头] | empty()
    deque | -- | -- | front() | back() | push_front()<br>push_back() | pop_front()<br>pop_back() | empty()
---

## 📑 index
- Stack 栈
  * <a href="#minStack">1. min-stack</a>
  * <a href="#erpn">2. evaluate-reverse-polish-notation</a>
  * <a href="#ds">3. decode-string</a>



[//]: # (Image References)
[image1]: .readme/stack.gif "stack"


<div id="minStack" onclick="window.location.hash">

#### 1. min-stack
linkage: [leetcode](https://leetcode-cn.com/problems/min-stack/ "最小栈")
- 设计一个支持 push ，pop ，top 操作，并能在常数时间内检索到最小元素的栈
  - push(x) —— 将元素 x 推入栈中
  - pop() —— 删除栈顶的元素
  - top() —— 获取栈顶元素
  - getMin() —— 检索栈中的最小元素
- 思路一：利用vector写出
- 注意：
  - vector删除最后一个元素可用pop_back();
  - stack删除最后一个元素可用pop(),stack返回栈顶元素用top();
  ```cpp
  class MinStack {
  public:
      /** initialize your data structure here. */
      MinStack() {
          num_.clear();
      }
      void push(int x) {
          num_.push_back(x);
      }
      void pop() {
          // vector删除最后一个元素可用pop_back()
          num_.pop_back();
      }
      int top() {
          return num_.at(num_.size()-1);
      }
      int getMin() {
          int count = num_.size();
          int get_min = INT_MAX;
          for(int i=0;i<count;i++)
          {
              if(get_min>num_.at(i))
              {
                  get_min = num_.at(i);
              }
          }
          return get_min;
      }
  private:
      std::vector<int> num_;
  };
  ```

- 思路二：利用stack写出
- 注意：
  - 注意stack出栈和入栈的方式，见图解
  - 思路：用两个栈实现，一个最小栈始终保证最小值在顶部
![][image1]
  ```cpp
  class MinStack {
  public:
      /** initialize your data structure here. */
      MinStack(){
      }

      void push(int x) {
          num_.push(x);
          // 注意：需要添加判断空的条件
          if(get_min_.empty() || x<=get_min_.top())
          {
              get_min_.push(x);
          }
      }

      void pop() {
          int x = num_.top();
          num_.pop();
          // 若 x 是当前的最小值，则也需要删除辅助栈的栈顶元素
          if (x == get_min_.top())
          {
              get_min_.pop();
          }
      }

      int top() {
          return num_.top();
      }

      int getMin() {
          return get_min_.top();
      }

  private:
      std::stack<int> num_;
      std::stack<int> get_min_;
  };
  ```
---

<div id="erpn" onclick="window.location.hash">

#### 2. evaluate-reverse-polish-notation
linkage: [leetcode](https://leetcode-cn.com/problems/evaluate-reverse-polish-notation/ "逆波兰表达式求值")
- 根据 逆波兰表示法，求表达式的值
- 说明：
  - 整数除法只保留整数部分。
  - 给定逆波兰表达式总是有效的。换句话说，表达式总会得出有效数值且不存在除数为 0 的情况。
- 思路：
  - 注意：c++如何将字符转为数字atoi或stoi，思路题
  - 开始判断是否存在数字符号
  - 不存在，向栈中push数据
  - 然后进行符号赋值操作
  - 最后将表达式计算的结果push栈中
  - 返回栈顶元素
    ```cpp
    class Solution {
    public:
        int evalRPN(vector<string>& tokens) 
        {
            std::stack<int> numbers;
            for(int i=0;i<tokens.size();i++)
            {
                if(tokens.at(i) == "+" || tokens.at(i) == "-" 
                    || tokens.at(i) == "*" || tokens.at(i) == "/")
                {
                    int res;
                    int n2 = numbers.top();
                    std::cout<<"n2: "<<n2<<std::endl;
                    numbers.pop();
                    int n1 = numbers.top();
                    std::cout<<"n1: "<<n1<<std::endl;
                    numbers.pop();

                    if(tokens.at(i) == "+")
                        res = n1+n2;
                    if(tokens.at(i) == "-")
                        res = n1-n2;
                    if(tokens.at(i) == "*")
                        res = n1*n2;
                    if(tokens.at(i) == "/")
                        res = n1/n2;
                    // 不要忘记将计算的结果push到栈中
                    numbers.push(res);
                }
                else
                {
                    // 重点：将字符串转换为数字
                    numbers.push(stoi(tokens.at(i)));
                }
            }
        return numbers.top();
        }
    };
    ```
---

<div id="ds" onclick="window.location.hash">

#### 3. decode-string
linkage: [leetcode](https://leetcode-cn.com/problems/decode-string/ "字符串解码")
- 给定一个经过编码的字符串，返回它解码后的字符串
