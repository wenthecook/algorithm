# 704 二分查找 27 移除元素 977 有序数组的平方

## 二分查找

**前提** 有序数组 不重复

重点在于区间的开闭，解法分成两组

- 左闭右闭 [l, r]
- 左闭右开 [l, r)

### 左闭右闭 条件 ( l **<=** r )

每一次取**区间中值**和target比较

如果区间中值等于target，则返回中值的位置。

如果比区间中值大，则 l = mid + 1, r = r

如果比区间中值小，则 l = l, r = mid - 1 (因为mid已经比较过了，不参与下一轮)

![左闭右闭](./左闭右闭.jpg)

极端情况

> 本轮为[a, a + 1] 此时M为 a + 1 / 2 = a + 0.5 => a
>
> 下一轮则是 [a, a] 或 [a + 1, a + 1]

> 本轮为 [a, a] 则 M = a
>
> 中值即为 nums[a]
>
> 若nums[a] 不等于 target 则下一轮为 [a, a - 1] 或 [a + 1, a], 破了条件 ( l <= r )

```c#
public class Solution {
    public int Search(int[] nums, int target) {
        int l = 0;
        int r = nums.Length - 1;

        while (l <= r)
        {
            int mid = l + ((r - l) / 2);
            if (nums[mid] < target)
            {
                l = mid + 1;
            }
            else if (nums[mid] > target)
            {
                r = mid - 1;
            }
            else
                return mid;
        }
        return -1;
    }
}
```

### 左闭右开 条件 ( l **<** r )

每一次取**区间中值**和target比较

如果区间中值等于target，则返回中值的位置。

如果比区间中值大，则 l = mid + 1, r = r

如果比区间中值小，则 l = l, r = mid (因为mid已经比较过了，不参与下一轮, 这次r可以等于mid)

![左闭右开](./左闭右开.jpg)

极端情况

> 本轮为[a, a + 1] 此时M为 a + 1 / 2 = a + 0.5 => a
>
> 下一轮则是 [a, a] 或 [a + 1, a + 1]
>
> 下一轮直接不满足 l < r

```c#
public class Solution {
    public int Search(int[] nums, int target) {
        int l = 0;
        int r = nums.Length;

        while (l < r)
        {
            int mid = l + ((r - l) / 2);
            if (nums[mid] < target)
            {
                l = mid + 1;
            }
            else if (nums[mid] > target)
            {
                r = mid;
            }
            else
                return mid;
        }
        return -1;
    }
}
```

## 移除元素

给你一个数组 nums 和一个值 val，你需要 原地 移除所有数值等于 val 的元素。元素的顺序可能发生改变。然后返回 nums 中与 val 不同的元素的数量。

假设 nums 中不等于 val 的元素数量为 k，要通过此题，您需要执行以下操作：

更改 nums 数组，使 nums 的前 k 个元素包含不等于 val 的元素。nums 的其余元素和 nums 的大小并不重要。
返回 k。

### 暴力解法（双循环）

外层遍历数组，碰到给定元素就开启内层循环

内层从当前元素开始，所有元素向前移动一个。

移动结束之后，当前位置指针减1，总长度（尾部指针位置）减1

![暴力解法](./27.移除元素-暴力解法.gif)

```c#
public class Solution {
    public int RemoveElement(int[] nums, int val) {
        int size = nums.Length;
        for (int i = 0; i < size; i++)
        {
            if (nums[i] == val)
            {
                for (int j = i; j < size - 1; j++)
                {
                    nums[j] = nums[j + 1];
                }
                i--;
                size--;
            }
        }
        return size;
    }
}
```

### 双指针法

设置一快一慢双指针，一开始双指针都从0开始

如果快指针指向的元素不用删除，将快指针所指数字移动到慢指针处

慢指针加1

快指针加1

如果快指针指向数组外，停止循环，返回慢指针所指下标（因为慢指针已经提前加1，所以此时慢指针的值就是长度）
![移除元素-双指针法](./27.移除元素-双指针法.gif)

```c#
public class Solution {
    public int RemoveElement(int[] nums, int val) {
        int slowPointer = 0;
        for (int fastPointer = 0; fastPointer < nums.Length; fastPointer++)
        {
            if (nums[fastPointer] != val)
            {
                nums[slowPointer] = nums[fastPointer];
                slowPointer++;
            }
        }
        return slowPointer;
    }
}
```