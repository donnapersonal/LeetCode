字符串实际上就是数组，所以这里把输入设置成字符串，做题时根据具体题目自行变通即可

```java
public static void slidingWindow(String s) {
    // 用合适的数据结构记录窗口中的数据，根据具体场景变通
    // 比如，想记录窗口中元素出现的次数，就用 map
    // 想记录窗口中的元素和，就用 int
    Map<Character, Integer> window = new HashMap<>();
    int left = 0, right = 0;
    while (right < s.length()) {
        // c 是将移入窗口的字符
        char c = s.charAt(right);
        window.put(c, window.getOrDefault(c, 0) + 1);
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        // ...

        // 判断左侧窗口是否要收缩
        while (left < right && window needs shrink) {
            // d 是将移出窗口的字符
            char d = s.charAt(left);
            window.put(d, window.get(d) - 1);
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            // ...
        }
    }
}
```
```python
def slidingWindow(s: str):
    window = dict()
    
    left = 0
    right = 0
    while right < len(s):
        # c 是将移入窗口的字符
        c = s[right]
        window[c] = window.get(c, 0) + 1
        # 增大窗口
        right += 1
        # 进行窗口内数据的一系列更新
        #...
        # 判断左侧窗口是否要收缩
        while left < right and "window needs shrink":
            # d 是将移出窗口的字符
            d = s[left]
            window[d] -= 1
            if window[d] == 0:
                del window[d]
            # 缩小窗口
            left += 1
            # 进行窗口内数据的一系列更新
            #...
```
```js
var slidingWindow = function(s) {
    var window = new Map();
    
    var left = 0, right = 0;
    while (right < s.length) {
        // c 是将移入窗口的字符
        var c = s[right];
        window.set(c, (window.get(c) || 0) + 1);
        // 增大窗口
        right++;
        // 进行窗口内数据的一系列更新
        ...

        // 判断左侧窗口是否要收缩
        while (left < right && "window needs shrink condition") {
            // d 是将移出窗口的字符
            var d = s[left];
            var count = window.get(d);
            if(count == 1){
                window.delete(d);
            }else{
                window.set(d, count - 1);
            }
            // 缩小窗口
            left++;
            // 进行窗口内数据的一系列更新
            ...
        }
};
```

**这个滑动窗口框架也用了一个嵌套 `while` 循环，为什么复杂度是 `O(n)`？**

指针 `left`, `right` 不会回退（它们的值只增不减），所以字符串/数组中的每个元素都只会进入窗口一次，然后被移出窗口一次，不会说有某些元素多次进入和离开窗口，所以算法的时间复杂度就和字符串/数组的长度成正比

反观嵌套 `for` 循环的暴力解法，`j` 会回退，所以某些元素会进入和离开窗口`多次`，所以时间复杂度就是 `O(n^2)`
