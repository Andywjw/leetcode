# [1438. Longest Continuous Subarray With Absolute Diff Less Than or Equal to Limit](https://leetcode.com/problems/longest-continuous-subarray-with-absolute-diff-less-than-or-equal-to-limit)

[中文文档](/solution/1400-1499/1438.Longest%20Continuous%20Subarray%20With%20Absolute%20Diff%20Less%20Than%20or%20Equal%20to%20Limit/README.md)

## Description

<p>Given an array of integers <code>nums</code> and an integer <code>limit</code>, return the size of the longest <strong>non-empty</strong> subarray such that the absolute difference between any two elements of this subarray is less than or equal to <code>limit</code><em>.</em></p>

<p>&nbsp;</p>
<p><strong class="example">Example 1:</strong></p>

<pre>
<strong>Input:</strong> nums = [8,2,4,7], limit = 4
<strong>Output:</strong> 2 
<strong>Explanation:</strong> All subarrays are: 
[8] with maximum absolute diff |8-8| = 0 &lt;= 4.
[8,2] with maximum absolute diff |8-2| = 6 &gt; 4. 
[8,2,4] with maximum absolute diff |8-2| = 6 &gt; 4.
[8,2,4,7] with maximum absolute diff |8-2| = 6 &gt; 4.
[2] with maximum absolute diff |2-2| = 0 &lt;= 4.
[2,4] with maximum absolute diff |2-4| = 2 &lt;= 4.
[2,4,7] with maximum absolute diff |2-7| = 5 &gt; 4.
[4] with maximum absolute diff |4-4| = 0 &lt;= 4.
[4,7] with maximum absolute diff |4-7| = 3 &lt;= 4.
[7] with maximum absolute diff |7-7| = 0 &lt;= 4. 
Therefore, the size of the longest subarray is 2.
</pre>

<p><strong class="example">Example 2:</strong></p>

<pre>
<strong>Input:</strong> nums = [10,1,2,4,7,2], limit = 5
<strong>Output:</strong> 4 
<strong>Explanation:</strong> The subarray [2,4,7,2] is the longest since the maximum absolute diff is |2-7| = 5 &lt;= 5.
</pre>

<p><strong class="example">Example 3:</strong></p>

<pre>
<strong>Input:</strong> nums = [4,2,2,2,4,4,2,2], limit = 0
<strong>Output:</strong> 3
</pre>

<p>&nbsp;</p>
<p><strong>Constraints:</strong></p>

<ul>
	<li><code>1 &lt;= nums.length &lt;= 10<sup>5</sup></code></li>
	<li><code>1 &lt;= nums[i] &lt;= 10<sup>9</sup></code></li>
	<li><code>0 &lt;= limit &lt;= 10<sup>9</sup></code></li>
</ul>

## Solutions

<!-- tabs:start -->

### **Python3**

```python
from sortedcontainers import SortedList


class Solution:
    def longestSubarray(self, nums: List[int], limit: int) -> int:
        sl = SortedList()
        ans = j = 0
        for i, v in enumerate(nums):
            sl.add(v)
            while sl[-1] - sl[0] > limit:
                sl.remove(nums[j])
                j += 1
            ans = max(ans, i - j + 1)
        return ans
```

### **Java**

```java
class Solution {
    public int longestSubarray(int[] nums, int limit) {
        TreeMap<Integer, Integer> tm = new TreeMap<>();
        int ans = 0, j = 0;
        for (int i = 0; i < nums.length; ++i) {
            tm.put(nums[i], tm.getOrDefault(nums[i], 0) + 1);
            while (tm.lastKey() - tm.firstKey() > limit) {
                tm.put(nums[j], tm.get(nums[j]) - 1);
                if (tm.get(nums[j]) == 0) {
                    tm.remove(nums[j]);
                }
                ++j;
            }
            ans = Math.max(ans, i - j + 1);
        }
        return ans;
    }
}
```

### **C++**

```cpp
class Solution {
public:
    int longestSubarray(vector<int>& nums, int limit) {
        multiset<int> s;
        int ans = 0, j = 0;
        for (int i = 0; i < nums.size(); ++i) {
            s.insert(nums[i]);
            while (*s.rbegin() - *s.begin() > limit) {
                s.erase(s.find(nums[j++]));
            }
            ans = max(ans, i - j + 1);
        }
        return ans;
    }
};
```

### **Go**

```go
func longestSubarray(nums []int, limit int) (ans int) {
	tm := treemap.NewWithIntComparator()
	j := 0
	for i, v := range nums {
		if x, ok := tm.Get(v); ok {
			tm.Put(v, x.(int)+1)
		} else {
			tm.Put(v, 1)
		}
		for {
			a, _ := tm.Min()
			b, _ := tm.Max()
			if b.(int)-a.(int) > limit {
				if x, _ := tm.Get(nums[j]); x.(int) == 1 {
					tm.Remove(nums[j])
				} else {
					tm.Put(nums[j], x.(int)-1)
				}
				j++
			} else {
				break
			}
		}
		ans = max(ans, i-j+1)
	}
	return
}

func max(a, b int) int {
	if a > b {
		return a
	}
	return b
}
```

### **...**

```

```

<!-- tabs:end -->
