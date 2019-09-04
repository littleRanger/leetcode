# 每日温度 

根据每日 气温 列表，请重新生成一个列表，对应位置的输入是你需要再等待多久温度才会升高超过该日的天数。如果之后都不会升高，请在该位置用 0 来代替。

## examples
| input:                           | output                   |
| -------------------------------- | ------------------------ |
| [73, 74, 75, 71, 69, 72, 76, 73] | [1, 1, 4, 2, 1, 1, 0, 0] |

## Note

The length of temperatures will be in the range [1, 30000]. Each temperature will be an integer in the range [30, 100].

## Soulution1 
using stack

思路：将数据从后往前放入栈中。

1. 每次放入前，在栈中寻找比自己大的节点，把自己小的都pop出去。
2. 循环结束条件：栈中无节点，即不存在比自己大的节点，该位置输出0；找到第一个比自己大的节点，则计算距离为该位置的输出。
3. 最后放入该节点。

| time | space |
| ---- | ----- |
| O(n) | O(n)  |

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        stack<int> s;
        int n = T.size();
        vector<int> out(n);
        for (int i=n-1; i>=0; i--) {
            while (!s.empty() && T[i]>=T[s.top()]) {
                s.pop();
            }
            out[i] = s.empty() ? 0 : s.top()-i;
            s.push(i);
        }
        return out;
    }
};
```

## Soulution2

| time | space |
| ---- | ----- |
| O(n) | O(1)  |

思路：从后往前处理元素，对每个元素i，反复与之前的元素j比较。

循环结束条件：
1. 找到比i大的j
2. j已经是后面最大的了，即out[j] 是0.

```cpp
class Solution {
public:
    vector<int> dailyTemperatures(vector<int>& T) {
        int i, j, n = T.size();
        vector<int> out(n);
        out[n-1] = 0;
        for (i=n-2; i>=0; i--) {
            j=i+1;
            while (T[i] >= T[j] && out[j] != 0) {
                    j += out[j];
            }
            out[i] = (T[j]<=T[i]) ? 0: j-i; 
        }
        return out;
    }
};
```
