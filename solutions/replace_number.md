# Replace Number

### 卡码网题目链接

[54.替换数字](https://kamacoder.com/problempage.php?pid=1064)

### 题目大意

给定一个字符串 `s`，它包含小写字母和数字字符，请编写一个函数，将字符串中的字母字符保持不变，而将每个数字字符替换为`number`，如，对于输入字符串 `"a1b2c3"`，函数应该将其转换为 `"anumberbnumbercnumber"`

`s` 仅包含小写字母和数字字符

```js
Input: a1b2c3
Output: anumberbnumbercnumber
```

说明：
- 1 <= s.length < 10000

### 解题

首先扩充数组到每个数字字符替换成 `number` 后的大小，如字符串 `"a5b"` 的长度为 `3`，则将数字字符变成字符串 `number` 后的字符串为 `"anumberb"` 长度为 `8`

然后从后向前替换数字字符，即`双指针法`:
- `l` 指向旧长度的末尾，`r` 指向新长度的末尾
- 旧数组从后向前，新数组也是从后向前，开始赋值到新数组 -> `s[r] = s[l]`
- 遇到数字则开始填充 `number`，从后向前  
- 继续从后向前填充，覆盖已有内容  
- 到最后 `l` 和 `r` 指向同一位置，但操作也是将 `s[l] 赋给 s[r]`
  
> 为什么要从后向前填充而不是从前向后呢？
> - 从前向后的时间复杂度是 `O(n^2)`，因为每次添加元素都要将添加元素后的所有元素整体向后移动
>
> **很多数组填充类的问题，做法都是先预先给数组扩容带填充后的大小，然后在从后向前进行操作**
>
> 好处：
> - 不用申请新数组
> - 从后向前填充元素，避免从前向后填充元素时每次添加元素都要将添加元素后的所有元素向后移动的问题

```java
import java.util.Scanner;

class Main {
  public static void main(String[] args) {
    Scanner in = new Scanner(System.in);
    String s = in.nextLine();
    StringBuilder sb = new StringBuilder();
    for (int i = 0; i < s.length(); i++) {
      if (Character.isDigit(s.charAt(i))) {
        sb.append("number");
      } else {
        sb.append(s.charAt(i));
      }
      System.out.println(sb);
    }
  }
}
```
- 时间复杂度：`O(n)`
- 空间复杂度：`O(1)`





