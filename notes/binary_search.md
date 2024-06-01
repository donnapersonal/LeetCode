# 二分查找

> 二分思维的精髓是：通过已知信息尽可能多地收缩（折半）搜索空间，从而增加穷举效率，快速找到目标

### 前提条件

数组为 `有序数组`，同时题目还强调 `数组中无重复元素`，因为一旦有重复元素则使用二分查找返回的元素下标可能不是唯一的
> 二分查找的效率之所以高，因为它利用了数组是`有序`的这一前提条件

`二分查找真正的坑`在于：要给 `mid` 加一还是减一，`while` 中到底用 `<=` 还是 `<`

### 区间

区间的定义就是`不变量`

要在二分查找过程中保持不变量，在 `while` 寻找中每一次边界的处理都要**坚持根据区间的定义来操作**，这就是**循环不变量规则**

写二分法，区间的定义一般为两种：  
- 左闭右闭即 `[left, right]`
- 左闭右开即 `[left, right)`

> 计算 `mid` 时需要防止溢出:
> 
> 代码中 `left + (right - left) / 2` 就和 `(left + right) / 2` 的结果相同，因为直接相加可能导致`整数溢出`，尤其是当 `left` 和 `right` 都很大时


### 左闭右闭 [left, right]

因为定义 `target` 在 `[left, right]` 区间，有如下要点：
- `while (left <= right)` 要使用 `<=` ，因为 `left == right` 是有意义的 
- 初始化 `right` 的赋值是 `nums.length - 1`  
- `if (nums[mid] > target)`，`right` 要赋值为 `mid - 1`，因为当前这个`nums[mid]`一定不是 `target`，接下来要查找的左区间结束下标就是 `mid - 1`

> **技巧**: 把所有情况用 `else if` 写清楚，这样可清楚地展现所有细节

### 左闭右开 [left, right)

若定义 `target` 是在左闭右开的区间里，即 `[left, right)`，则二分法的边界处理方式则截然不同
- `while (left < right)`，这里使用 `<`，因为 `left == right` 在区间 `[left, right)` 没有意义   
- `if (nums[mid] > target)`，`right` 更新为 `mid`，因为当前 `nums[mid]` 不等于 `target`，去左区间继续寻找，而寻找区间是左闭右开区间，所以 `right` 更新为 `mid`，即：下个查询区间不会去比较 `nums[mid]`

### 最基本的二分查找算法模板

```java
// 左闭右闭
int binary_search(int[] nums, int target) {
  // 避免当 target 小于nums[0] nums[nums.length - 1]时多次循环运算 
  if (target < nums[0] || target > nums[nums.length - 1]) { 
    return -1; 
  } 
  int left = 0, right = nums.length - 1; 
  while(left <= right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid - 1; 
    } else if(nums[mid] == target) {
      // 直接返回
      return mid;
    }
  }
  // 直接返回
  return -1;
}

// 左闭右开
int binarySearch(int[] nums, int target) {
  int left = 0, right = nums.length;
  // 当left=right时，由于nums[right]不在查找范围，所以不必包括此情况
  while (left < right) {
    // 位运算 + 防止大数溢出
    int mid = left + ((right - left) >> 1);
    if (nums[mid] == target)
      return mid; 
    else if (nums[mid] < target)
      left = mid + 1; // 去右区间寻找
    else if (nums[mid] > target)
      // 若中间值大于目标值，中间值不应在下次查找的范围内，但中间值的前一个值应在；
      // 由于right本来就不在查找范围内，所以将右边界更新为中间值，若更新右边界为 mid-1 则将中间值的前一个值也踢出了下次寻找范围
      right = mid;  // 去左区间寻找
    }
  return -1;
```
```python
# 左闭右开
def binary_search(nums: List[int], target: int) -> int:
    # 设置左右下标
    left, right = 0, len(nums) - 1
    while left <= right:
        mid = left + (right - left) // 2
        if nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid - 1
        elif nums[mid] == target:
            # 找到目标值
            return mid
    # 没有找到目标值
    return -1

# 左闭右开
def binarySearch(nums:List[int], target:int) -> int:
  left = 0
  right = len(nums)  
  
  while left < right: 
    mid = left + (right - left) // 2
    if nums[mid] > target:
      right = mid  
    elif nums[middle] < target:
      left = mid + 1  
    else:
      return mid 
  return -1 
```
```js
// 左闭右闭
var binary_search = function(nums, target) {
    var left = 0, right = nums.length - 1; 
    while(left <= right) {
        var mid = left + Math.floor((right - left) / 2);
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1; 
        } else if(nums[mid] == target) {
            // 直接返回
            return mid;
        }
    }
    // 直接返回
    return -1;
}

// 左闭右开
var binarySearch = function(nums, target) {
  let mid, left = 0, right = nums.length;    
  while (left < right) {
    mid = left + ((right - left) >> 1);
    if (nums[mid] > target) {
      right = mid;  
    } else if (nums[mid] < target) {
      left = mid + 1;  
    } else {
      return mid;
    }
  }
  return -1;
};
```
- 时间复杂度：`O(log n)`，每次操作都将区间长度除以 `2`，操作次数是 `n` 的对数 (以 `2` 为底)

- 空间复杂度：`O(1)`
  - 这个算法不需要额外的存储空间来存储大量数据，它只需几个变量来存储索引（`left`, `right`, `mid`）和目标值（`target`）
  - 算法执行过程中，不管输入数据的大小，它使用的空间都保持不变
  - 由于它不使用递归实现，因此也不需要额外的栈空间

### 寻找左侧/右侧边界

如有序数组 `nums = [1,2,2,2,3]`，`target` 为 `2`，此算法返回的索引是 `2`，没错

但若想得到 `target` 的左侧边界即索引 `1`，或想得到 `target` 的右侧边界即索引 `3`，此算法是无法处理的

其寻找边界的搜索基本模板如下：

```java
// 左闭右闭
// 寻找左侧边界
int binarySearchLeft (int[] nums, int target) {
  int left = 0, right = nums.length - 1;
  while(left <= right) {
    int mid = left + (right - left) / 2;
    if(nums[mid] < right) {
      // 搜索区间变为 [mid+1, right]
      left = mid + 1;
    } else if(nums[mid] > target) {
      // 搜索区间变为 [left, mid-1]
      right = mid - 1;
    } else if (nums[mid] == target) {
      // 别返回，锁定左侧边界
      right = mid - 1;
    }
  }
  // 判断 target 是否存在于 nums 中
  // 如果越界，target 肯定不存在，返回 -1
  if (left < 0 || left >= nums.length) {
    return -1;
  }
  // 判断 nums[left] 是不是 target
  return nums[left] == target ? left : -1;
}

// 寻找右侧边界
int binarySearchRight (int[] nums, int target) {
  int left = 0, right = nums.length - 1;
  while(left <= right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid - 1;
    } else if (nums[mid] == target) {
      // 别返回，锁定右侧边界
      left = mid + 1;
    }
  }
  // 由于 while 的结束条件是 right == left - 1，且现在在求右边界
  // 所以用 right 替代 left - 1 更好记
  if (right < 0 || right >= nums.length) {
    return -1;
  }
  return nums[right] == target ? right : -1;
}

// 左闭右开
// 寻找左侧边界
int binarySearchLeft(int[] nums, int target) {
  int left = 0, right = nums.length; // 注意
  while (left < right) { // 注意
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      right = mid;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid;
    }
  }
  return left;
}

// 寻找右侧边界
int binarySearchRight(int[] nums, int target) {
  int left = 0, right = nums.length;
  while (left < right) {
    int mid = left + (right - left) / 2;
    if (nums[mid] == target) {
      left = mid + 1; // 注意
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid;
    }
  }
  return left - 1; // 注意
}
```
> 这里为什么返回 `left` 而不是 `right` -- 都一样，因为 `while` 终止的条件是 `left == right`

```python 
# 左闭右闭
# 寻找左侧边界
def binarySearchLeft(nums: List[int], target: int) -> int:
  left, right = 0, len(nums) - 1
  while left <= right:
    mid = left + (right - left) // 2
    if nums[mid] < target:
      left = mid + 1
    elif nums[mid] > target:
      right = mid - 1
    elif nums[mid] == target:
      right = mid - 1
  
  # 判断是否存在目标值
  if left < 0 or left >= len(nums):
    return -1
  
  # 判断找到的左边界是否是目标值
  return left if nums[left] == target else -1

# 寻找右侧边界
def binarySearchRight(nums: List[int], target: int) -> int:
  left, right = 0, len(nums) - 1
  while left <= right:
    mid = left + (right - left) // 2
    if nums[mid] < target:
      left = mid + 1
    elif nums[mid] > target:
      right = mid - 1
    elif nums[mid] == target:
      # 存在目标值，缩小左边界
      left = mid + 1

  # 判断是否存在目标值
  if right < 0 or right >= len(nums):
    return -1
  
  # 判断找到的右边界是否是目标值
    return right if nums[right] == target else -1

# 左闭右开
# 寻找左侧边界
def binarySearchLeft(nums: List[int], target: int) -> int:
    left, right = 0, nums.length; # 注意
    while left < right: # 注意
        mid = left + (right - left) // 2
        if nums[mid] == target:
            right = mid
        elif nums[mid] < target:
            left = mid + 1
        elif nums[mid] > target:
            right = mid; # 注意

    return left

# 寻找右侧边界
def binarySearchRight(nums: List[int], target: int) -> int:
  left, right = 0, len(nums)
  while left < right:
    mid = left + (right - left) // 2
    if nums[mid] == target:
        left = mid + 1 # 注意
    elif nums[mid] < target:
        left = mid + 1
    elif nums[mid] > target:
        right = mid
  return left - 1 # 注意
```
```js
// 左闭右闭
// 寻找左侧边界
var binarySearchLeft = function(nums, target) {
    var left = 0, right = nums.length - 1;
    while (left <= right) {
        var mid = left + Math.floor((right - left) / 2);
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定左侧边界
            right = mid - 1;
        }
    }
    // 判断 target 是否存在于 nums 中
    if (left < 0 || left >= nums.length) {
        return -1;
    }
    // 判断一下 nums[left] 是不是 target
    return nums[left] == target ? left : -1;
}

// 寻找右侧边界
var binarySearchRight = function(nums, target) {
    var left = 0, right = nums.length - 1;
    while (left <= right) {
        var mid = left + Math.floor((right - left) / 2);
        if (nums[mid] < target) {
            left = mid + 1;
        } else if (nums[mid] > target) {
            right = mid - 1;
        } else if (nums[mid] == target) {
            // 别返回，锁定右侧边界
            left = mid + 1;
        }
    }
    
    // 由于 while 的结束条件是 right == left - 1，且现在在求右边界
    // 所以用 right 替代 left - 1 更好记
    if (right < 0 || right >= nums.length) {
        return -1;
    }
    return nums[right] == target ? right : -1;
}

// 左闭右开
// 寻找左侧边界
var binarySearchLeft = function(nums, target) {
  var left = 0, right = nums.length; // 注意
  
  while (left < right) { // 注意
    var mid = left + Math.floor((right - left) / 2);
    if (nums[mid] == target) {
      right = mid;
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid; // 注意
    }
  }
  return left;
}


// 寻找右侧边界
var binarySearchRight = function(nums, target) {
  var left = 0, right = nums.length;
  
  while (left < right) {
    var mid = left + Math.floor((right - left) / 2);
    if (nums[mid] === target) {
      left = mid + 1; // 注意
    } else if (nums[mid] < target) {
      left = mid + 1;
    } else if (nums[mid] > target) {
      right = mid;
    }
  }
  return left - 1; // 注意
}
```

**综合写法**

```js
var searchRange = function(nums, target) {
  if (nums == null || nums.length == 0) return [-1, -1];
  // 定义左右边界
  let left = 0, right = nums.length - 1;
  // 二分查找
  while (left <= right) {
    let mid = left + Math.floor((right - left) / 2);
    if(nums[mid] >= target) {
      // 搜索区间变为 [left, mid-1]
      // 收缩右侧边界
      right = mid - 1;
    } else {
      // 搜索区间变为 [mid+1, right]
      left = mid + 1;
    }
  }
  // 判断是否找到
  if (left >= nums.length || nums[left] != target) return [-1, -1];
  let res = [left];
  
  // 寻找最右端的目标值索引
  right = nums.length - 1;
  while (left <= right) {
    let mid = left + Math.floor((right - left) / 2);
    if (nums[mid] <= target) {
      // 搜索区间变为 [mid+1, right]
      left = mid + 1;
    } else {
       // 搜索区间变为 [left, mid-1]
       right = mid - 1;
    }
  }
  res.push(right);
  return res;
};
```

### 总结

在具体的算法问题中，常用到的是`寻找左侧边界`和 `寻找右侧边界` 这两种场景，很少有单独`寻找一个元素`

算法题一般都让求`最值`，如求轮船的`最低运载能力` -- 求最值的过程必然是`搜索一个边界`的过程