# Rabin Karp 算法

### 理论概念

在文本串 `t` 中搜索模式串 `p` 的起始索引，暴力字符串匹配算法是这样的：

```python
def search(t: str, p: str) -> int:
    n, l = len(t), len(p)
    for i in range(n - l + 1):
        subStr = t.substring(i, i + l)
        if subStr.equals(p):
            return i
    return -1
```

总的时间复杂度是 `O(l * n)`，优化的核心也是子串 `subStr` 和模式串 `p` 匹配的部分

`RK` 算法思想：对于给定文本串 `t` 与模式串 `p`，通过`滚动哈希算法`快速筛选出与模式串 `p` 不匹配的文本位置，然后在其余位置继续检查匹配项
- 不是每次都去一个字符一个字符地比较子串和模式串，而是维护一个`滑动窗口`，运用`滑动哈希算法`一边滑动一边计算窗口中字符串的哈希值，拿这个哈希值去和模式串的哈希值比较 
- 这样就可以避免截取子串，从而把匹配算法降低为 `O(n)`

### Rabin Karp 算法

```python
def rabinKarp(txt: str, pat: str) -> int:
    # 位数
    L = len(pat)
    # 进制（只考虑 ASCII 编码）
    R = 256
    # 取一个比较大的素数作为求模的除数
    Q = 1658598167
    # R^(L - 1) 的结果
    RL = 1
    for i in range(1, L):
        # 计算过程中不断求模，避免溢出
        RL = (RL * R) % Q
    # 计算模式串的哈希值
    patHash = 0
    for i in range(len(pat)):
        patHash = (R * patHash + ord(pat[i])) % Q

    # 滑动窗口中子字符串的哈希值
    windowHash = 0

    # 滑动窗口代码框架
    left, right = 0, 0
    while right < len(txt):
        # 扩大窗口，移入字符
        windowHash = ((R * windowHash) % Q + ord(txt[right])) % Q
        right += 1

        # 当子串的长度达到要求
        if right - left == L:
            # 根据哈希值判断是否匹配模式串
            if windowHash == patHash:
                # 当前窗口中的子串哈希值等于模式串的哈希值
                # 还需进一步确认窗口子串是否真的和模式串相同，避免哈希冲突
                if pat == txt[left:right]:
                    return left
            # 缩小窗口，移出字符
            windowHash = (windowHash - (ord(txt[left]) * RL) % Q + Q) % Q
            # X % Q == (X + Q) % Q 是一个模运算法则
            # 因为 windowHash - (txt[left] * RL) % Q 可能是负数
            # 所以额外再加一个 Q，保证 windowHash 不会是负数

            left += 1
    # 没有找到模式串
    return -1
```
这代码解决了`整数溢出`的问题
- 把一个很大的数字映射到一个较小的范围内 --> `求模（余数）`
- 无论一个数字多大，让它除以 `Q`，余数一定会落在 `[0, Q-1]` 的范围内，因此可以设置一个 `Q`，用求模的方式让 `windowHash` 和 `patHash` 保持在 `[0, Q-1]` 之间，就可以有效避免整型溢出
- 对于一个字符串，不需要把完整的 `256` 进制数字存下来，而是对这个巨大的 `256` 进制数求 `Q` 的余数，然后把这个余数作为该字符串的哈希值即可

这代码也解决了`哈希冲突`的问题
- 求模之的哈希值不能和原始字符串一一对应，可能出现一对多的情况，即哈希冲突。若 `10 % 7` 等于 `3`，而 `17 % 7` 也等于 `3`，若得到余数 `3`，不能确定原始数字就一定是 `10`
- 类似的，若发现 `windowHash == patHash`，也不能完全肯定窗口中的字符串一定就和模式串匹配，有可能它俩不匹配，但恰好求模算出来的哈希值一样，这就产生了`哈希冲突`

> 模运算的两个运算法则：
> - `X % Q == (X + Q) % Q`
> - `(X + Y) % Q == (X % Q + Y % Q) % Q`

`Rabin Karp` 算法的时间复杂度是 `O(n + l)`，n 为文本串 `t` 的长度，`l` 为模式串 `p` 的长度，每次出现哈希冲突时会使用 `O(l)` 的时间进行暴力匹配，但考虑到只要 `Q` 设置的合理，哈希冲突的出现概率会很小，所以可以忽略不计

**为什么要这个 `Q` 尽可能大呢？-- 为了降低哈希冲突的概率**
- 代码中把这个 `Q` 作为除数，余数（哈希值）一定落在 `[0, Q-1]` 之间，所以 `Q` 越大，哈希值的空间就越大，就越不容易出现哈希冲突，整个算法的效率就会高一些

**为什么这个 `Q` 要是素数呢？-- 为了降低哈希冲突的概率**
- 极端例子：令 `Q = 100`，无论一个数 `X` 再大，`X % Q` 的结果必然是 `X` 的最后两位。即 `X` 前的那些位根本没利用，因此这个哈希算法存在某些规律性，不够随机，进而更容易导致哈希冲突，降低算法的效率
- 若把 `Q` 设置为一个素数，可以更充分利用被除数 `X` 的每一位，得到的结果更随机，哈希冲突的概率更小（这个结论是能够在数学上证明的）


