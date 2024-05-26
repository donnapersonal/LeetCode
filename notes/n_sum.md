# 一个方法解决 nSum 问题

`nSum` 类型问题的通用解法，支持 2sum，3sum，4sum... 等

`n == 2` 时是 `twoSum` 的双指针解法，`n > 2` 时就是穷举第一个数字，然后递归调用计算 `(n-1)Sum`，组装答案

> 注意：调用这个 `nSum` 函数之前一定要先给 `nums` 数组排序
> 
> 因为 `nSum` 是一个递归函数，若在 `nSum` 函数里调用排序函数，则每次递归都会进行没有必要的排序，导致效率非常低

时间复杂度分析：
- `n = 2` 时，时间复杂度 `O(nlogn)`，排序所消耗的时间
- `n > 2` 时，时间复杂度为 `O(n^n-1)`，即 `n` 的 `n-1` 次方，至少是 `2` 次方，此时可省略排序所消耗的时间，如：`3sum` 为 `O(n^2)`，`4sum` 为 `O(n^3)`...

```java
class Solution {
    public List<List<Integer>> nSumTarget(int[] nums, int n, int start, int target) {
    		int len = nums.length;
            List<List<Integer>> res = new ArrayList<>();
    		// 至少是 2Sum，且数组大小不应该小于 n
            if (n < 2 || sz < n) return res;
    		// 2Sum 是 base case
            if (n == 2) {
    			// 双指针那一套操作
    			int left = start, right = len - 1;
    			while(left < right) {
    				int sum = nums[left] + nums[right];
    				if(sum < target) {
    					while(left < right && nums[left] == nums[left+1]) left++;
    					left++;
    				} else if(sum > target) {
    					while(left < right && nums[right] == nums[right - 1]) right--;
    					right--;
    				} else {
    					res.add(Arrays.asList(nums[left], nums[right]));
    				    while (left < right && nums[left] == nums[left+1]) left++;
                        while (left < right && nums[right] == nums[right-1]) right--;
    				    left++;
    				    right--;
    				}
    			}
    		} else {
      			// n > 2 时，递归计算 (n-1)Sum 的结果
    		    for (int i = start; i < len; i++) {
    				List<List<Integer>> sub = nSumTarget(nums, n - 1, i + 1, target - nums[i]);
    				for (List<Integer> arr : sub) {
    					// (n-1)Sum 加上 nums[i] 就是 nSum
                        arr.add(nums[i]);
                        res.add(arr);
    				}
    				while (i < len - 1 && nums[i] == nums[i + 1]) i++;
    			}
    		}
    		return res;
    }
}
```
```python 
def nSumTarget(nums, n, start, target):
    sz = len(nums)
    res = []
    # 至少是 2Sum，且数组大小不应该小于 n
    if n < 2 or sz < n:
        return res
    # 2Sum 是 base case
    if n == 2:
        # 双指针那一套操作
        l, r = start, sz - 1
        while l < r:
            sum = nums[l] + nums[r]
            left, right = nums[l], nums[r]
            if sum < target:
                while l < r and nums[l] == left:
                    l += 1
            elif sum > target:
                while l < r and nums[r] == right:
                    r -= 1
            else:
                res.append([left, right])
                while l < r and nums[l] == left:
                    l += 1
                while l < r and nums[r] == right:
                    r -= 1
    else:
        # n > 2 时，递归计算 (n-1)Sum 的结果
        for i in range(start, sz):
            sub = nSumTarget(nums, n - 1, i + 1, target - nums[i])
            for arr in sub:
                # (n-1)Sum 加上 nums[i] 就是 nSum
                arr.append(nums[i])
                res.append(arr)
            while i < sz - 1 and nums[i] == nums[i + 1]:
                i += 1
    return res

# 示例使用
nums = [1, 0, -1, 0, -2, 2]
target = 0
nums.sort()
result = nSumTarget(nums, 4, 0, target)
for r in result:
    print(r)
```
```js
var fourSum = function(nums, target) {
    nums.sort((a, b) => a - b);
    // 4sum之和，则 n 为 4
    return nSumTarget(nums, 4, 0, target);
}

var nSumTarget = function(nums, n, start, target) {
    let res = [];
    let len = nums.length;
    // 至少是 2Sum，且数组大小不应该小于 n
    if(n < 2 || len < n) return res;

    // 2Sum
    if(n == 2) {
        res = towSumTarget(nums, start, target);
    } else {
        for (let i = start; i < nums.length; i++) {
            // 递归求(n - 1)sum
            let subRes = nSumTarget(
                nums,
                n - 1,
                i + 1,
                target - nums[i]
            );
            for (let j = 0; j < subRes.length; j++) {
                res.push([nums[i], ...subRes[j]]);
            }
            // 跳过相同元素
            while (nums[i] === nums[i + 1]) i++;
        }
    }
    return res;
}

var towSumTarget = function (nums, start, target) {
    let res = [];
    let len = nums.length;
    let left = start;
    let right = len - 1;
    while (left < right) {
        let sum = nums[left] + nums[right];
        if (sum < target) {
            while (nums[left] === nums[left + 1]) left++;
            left++;
        } else if (sum > target) {
            while (nums[right] === nums[right - 1]) right--;
            right--;
        } else {
            // 相等
            res.push([nums[left], nums[right]]);
            // 跳过相同元素
            while (nums[left] === nums[left + 1]) left++;
            while (nums[right] === nums[right - 1]) right--;
            left++;
            right--;
        }
    }
    return res;
}
```

