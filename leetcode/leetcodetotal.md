---
title: Leetcode各类题目模板分析题目总结
categories:
  - java
  
abbrlink: 2701
date: 2025.5.9
tags: 
   - java 
   - 数据结构
   - leetcode
   - 算法
---

# 滑动窗口

看到一串数组，要求数量的时候，可能就会用到滑动窗口

## 定长

只需要考虑进出即可

然后考虑窗口大小不足的时候,continue

模板：

```java
class Solution643{
    public double findMaxAverage(int[] nums, int k){
        int maxS = Integer.MIN_VALUE;
        int s=  0;
        for (int i =0;i<nums.length;i++){
            s +=nums[i];
            if (i<k-1){
                continue;
            }
            maxS = Math.max(maxS,s);
            s -=nums[i-k+1];
        }
        return (double) maxS/k;
    }
}
```

头进尾部出，然后考虑变化

## 不定长

不定长的基本就是考虑窗口的缩小。

### 求最值

模板：

```java
class Solution2958{
    public int maxSubarrayLength(int[] nums, int k){
        int ans = 0, left= 0;
        Map<Integer,Integer> cnt = new HashMap<>();
        for (int right = 0;right<nums.length;right++){
            cnt.merge(nums[right],1,Integer::sum);
            while (cnt.get(nums[right])>k){
                cnt.merge(nums[left++],-1,Integer::sum);
            }
            ans = Math.max(ans,right-left+1);
        }
        return ans;
    }
}
```

就是右边进入了，然后check一下，然后进行某些操作，然后左边出去

一般到最后需要的都是窗口的长度right-left+1

### 求数目

分为越长越合法，越短越合法和**恰好形**

**越长越合法**是指

[left,right] 这个子数组是不满足题目要求的，但在退出循环之前的最后一轮循环，**[left−1,right]** 是满足题目要求的。由于子数组越长，越能满足题目要求，所以除了 [left−1,right]，还有 [left−2,right],[left−3,right],…,[0,right] 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 0,1,2,…,left−1 的所有子数组都是满足要求的，这一共有 **left** 个。

**越短越合法**是指：

[left,right] 这个子数组是满足题目要求的。由于子数组越短，越能满足题目要求，所以除了 [left,right]，还有 **[left+1,right]**,[left+2,right],…,[right,right] 都是满足要求的。也就是说，当右端点固定在 right 时，左端点在 left,left+1,left+2,…,right 的所有子数组都是满足要求的，这一共有 **right−left+1** 个。

```java
class Solution713{
    public int numSubarrayProductLessThanK(int[] nums, int k){
        if (k<=1){
            return 0;
        }
        int ans= 0;
        int x = 1;
        int left  = 0;
        for (int right = 0;right<nums.length;right++){
            x *=nums[right];
            while (x>=k){
                x /=nums[left++];
            }
            ans +=right-left+1;
        }
        return ans;
    }
}
```

基本差不多，都是右边进去了，，然后经过check，然后左边出去，窗口缩小。‘

只不过返回的不同罢了

**恰好**就是正好为这个的数目

例如，要计算有多少个元素和恰好等于 k 的子数组，可以把问题变成：

计算有多少个元素和 ≥k 的子数组。
计算有多少个元素和 >k，也就是 ≥k+1 的子数组。
答案就是元素和 ≥k 的子数组个数，减去元素和 ≥k+1 的子数组个数。这里把 > 转换成 ≥，从而可以把滑窗逻辑封装成一个函数 f，然后用 **f(k) - f(k + 1)** 计算，无需编写两份滑窗代码。

总结：**「恰好」可以拆分成两个「至少」，也就是两个「越长越合法」的滑窗问题。**

注：也可以把问题变成 **≤k 减去 ≤k−1（两个至多）。可根据题目选择合适的变形方式。**

也可以把两个滑动窗口合并起来，维护同一个右端点 *right* 和两个左端点 *left*1 和 *left*2，我把这种写法叫做**三指针滑动窗口**。

一般来说一个函数写f(k) - f(k + 1)

另一个函数实现滑动窗口，就足够了。

```java
class Solution930{
    public int numSubarraysWithSumA(int[] nums, int goal){
        int ans1 = 0, left1=0,left2=0,ans2=0;
        int sum1=0,sum2=0;
        for (int right = 0;right<nums.length;right++){
            sum1 +=nums[right];
            while (sum1>=goal&&left1<=right){
                sum1 -=nums[left1++];
            }
            ans1 +=left1;
            sum2 +=nums[right];
            while (sum2>=goal+1&&left2<=right){
                sum2 -=nums[left2++];
            }
            ans2 +=left2;
        }
        return ans1-ans2;
    }
    public int numSubarraysWithSum(int[] nums, int goal){
        return atMost(nums,goal)-atMost(nums,goal+1);
    }
    private int atMost(int[] nums, int goal){
        int ans = 0,left = 0,sum = 0;
        for (int right = 0;right<nums.length;right++){
            sum +=nums[right];
            while (sum>=goal&&left<=right){
                sum -=nums[left++];
            }
            ans +=left;
        }
        return ans;
    }
}
```



# 二分查找

二分查找的原理就是取一个中间值，然后那中间值和目标值进行比较。

如果比目标值大的话，说明目标值在左边，中间值mid就变为右边right

相对应的，小于目标值的话，说明目标值在右边，中间值mid就变为left

二分查找的总结：

必须数组/序列是**有序的**，二分前必须先进行排序。

要确定搜索区间常见形式：`[lo, hi]`、`[lo, hi)`、`(lo, hi]`、`(lo, hi)`

确定开区间闭区间

开区间：

```java
private int lowerBound(int[] nums,int right,int target){
        int left=-1;
        while (left+1<right){
            int mid = (left+right)>>>1;
            if (nums[mid]>=target){
                right=mid;
            }else {
                left=mid;
            }
        }
        return right;
    }
```

闭区间：

```java
class Solution{
    public int searchInsert(int[] nums,int target){
        int left = 0,right = nums.length-1;
        while (left<=right){
            int mid = (left+right)/2;
            if (nums[mid] == target){
                return mid;
            }
            else if (nums[mid]<target){
                left =mid+1;
            }
            else {
                right = mid-1;
            }
        }
        return left;
    }
}
```

mid的取值：通常用 `mid = lo + (hi - lo) / 2`（或无符号右移 `>>> 1`）这样来防止溢出

还要设计check条件：

将问题转化为一个布尔函数 `check(mid)`，能准确告诉你“mid 是否满足某侧条件”。

根据 `check(mid)` 结果，把 `lo` 或 `hi` 缩到 `mid` 及其左／右一侧。

```java
int lo = L, hi = R;
while (lo < hi) {
    int mid = lo + (hi - lo) / 2;
    if (check(mid)) {
        hi = mid;      // 保留 mid
    } else {
        lo = mid + 1;  // 丢弃 mid
    }
}
return lo;

```

`lowerBound`: 找到**第一个 ≥ target** 的索引

`upperBound`: 找到**第一个 > target** 的索引

| 场景                    | 区间形式   | 备注                                           |
| ----------------------- | ---------- | ---------------------------------------------- |
| 查找某值 / 插入位置     | `[0, n-1]` | 经典闭区间；找不到时返回 `lo` 作为插入点       |
| lowerBound / upperBound | `(-1, n]`  | 开区间；`left = -1, right = n`                 |
| 最接近元素（差值比较）  | `[0, n-k]` | 窗口长度为 `k`，比较左右边界距离取决于差值大小 |
| 双指针对撞              | `lo < hi`  | 例如找最大满足条件的下标                       |

**可以使用查找某些值的问题，省去了遍历**

# 双指针

## 单序列

相向双指针：两个指针 *left*=0, *right*=*n*−1，从数组的两端开始，向中间移动，这叫**相向双指针**。上面的滑动窗口相当于**同向双指针**。

然后到中间去汇聚

一般来说条件是left<right

然后进行运行

### **同向双指针**

两个指针的移动方向相同（都向右，或者都向左）。

从同一个方向开始

外层循环控制右指针，内层条件满足时移动左指针收缩窗口

就是滑动窗口哈哈

### 背向双指针

从中间开始往两边

### **原地修改：**

主要是运用到了栈的思想

常用于数组原地修改，慢指针标记“结果区域”，快指针用于扫描

**原地修改+下标**

就是下标的问题

如果是顺序排序，不缺少元素的话，

|nums[i]| = index+1 防止负数哈

然后如果是乱序的话，也是可以对应起来的的 

如果if (nums[index]>0){
                nums[index] *=-1;
            }

的话，缺少的那个正好是正数

然后要返回那个数的话，i+1即可

上面那个题，出现两次跟这个缺少是一样的

会把index变为负的

```
1. 值域是 [1, n]，考虑 nums[i] - 1 做下标
2. 标记：将 nums[nums[i] - 1] *= -1
3. 查询：
   - 找缺失 ➜ 哪些 index 上还为正，对应值就是缺失的 i+1
   - 找重复 ➜ 哪些 index 第一次访问时就已经是负数

```

👆

## 双序列

### 子序列问题

还是标准的子序列问题，两个指针移动，当满足条件的时候子序列的指针移动

然后字典的一直移动

看最后子序列的指针能不能到末尾，到了就说明是，反则不是

```java
private boolean isSubseq(String s, String t){
        int i =0;
        for (char c:t.toCharArray()){
            if (s.charAt(i)==c&&++i==s.length()){
                return true;
            }
        }
        return false;
    }
```

