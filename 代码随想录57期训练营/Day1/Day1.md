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