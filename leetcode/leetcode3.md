---
title: Leetcode双指针
categories:
  - java
  
abbrlink: 2701
date: 2025.5.12
tags: 
   - java 
   - 数据结构
   - leetcode
   - 算法
---

# 相向双指针

两个指针 *left*=0, *right*=*n*−1，从数组的两端开始，向中间移动，这叫**相向双指针**。上面的滑动窗口相当于**同向双指针**。

## [344. 反转字符串](https://leetcode.cn/problems/reverse-string/)

编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 `s` 的形式给出。

不要给另外的数组分配额外的空间，你必须**[原地](https://baike.baidu.com/item/原地算法)修改输入数组**、使用 O(1) 的额外空间解决这一问题。

```java
class Solution344{
    public void reverseString(char[] s){
        int n = s.length;
        for (int left=0,right=n-1;left<right;left++,right--){
            char tmp = s[left];
            s[left] = s[right];
            s[right] = tmp;
        }
    }
}

```

简单的双指针解决

就是让头和尾互换值就ok了

## [125. 验证回文串](https://leetcode.cn/problems/valid-palindrome/)

如果在将所有大写字符转换为小写字符、并移除所有非字母数字字符之后，短语正着读和反着读都一样。则可以认为该短语是一个 **回文串** 。

字母和数字都属于字母数字字符。

给你一个字符串 `s`，如果它是 **回文串** ，返回 `true` ；否则，返回 `false` 。

```java
class Solution125{
    public boolean isPalindrome(String s){
        int n = s.length();
        int left = 0;
        int right = n-1;
        while (left<right){
            if (!Character.isLetterOrDigit(s.charAt(left))){
                left++;
            }else if (!Character.isLetterOrDigit(s.charAt(right))){
                right--;
            }else if (Character.toLowerCase(s.charAt(left))==Character.toLowerCase(s.charAt(right))){
                left++;
                right--;
            }else {
                return false;
            }
        }
        return true;
    }
}
```

遇到数字啥的就跳过

如果left与right转成小写相等的话，就继续往前，这样就可以转成回文

然后没有的话，就为false

## [1750. 删除字符串两端相同字符后的最短长度](https://leetcode.cn/problems/minimum-length-of-string-after-deleting-similar-ends/)



给你一个只包含字符 `'a'`，`'b'` 和 `'c'` 的字符串 `s` ，你可以执行下面这个操作（5 个步骤）任意次：

1. 选择字符串 `s` 一个 **非空** 的前缀，这个前缀的所有字符都相同。
2. 选择字符串 `s` 一个 **非空** 的后缀，这个后缀的所有字符都相同。
3. 前缀和后缀在字符串中任意位置都不能有交集。
4. 前缀和后缀包含的所有字符都要相同。
5. 同时删除前缀和后缀。

请你返回对字符串 `s` 执行上面操作任意次以后（可能 0 次），能得到的 **最短长度** 。

```java
class Solution1750{
    public int minimumLength(String S){
        char[] s = S.toCharArray();
        int n = s.length;
        int left = 0,right = n-1;
        while (left<right&&s[left]==s[right]){
            char c = s[left];
            while (left<=right&&s[left]==c) left++;
            while (left<=right&&s[right]==c) right--;
        }
        return right-left+1;
    }
}
```

简单的双指针问题，最后剩下的窗口的长度就是最后的


代码



测试用例



测试结果

测试结果

## [2105. 给植物浇水 II](https://leetcode.cn/problems/watering-plants-ii/)

Alice 和 Bob 打算给花园里的 `n` 株植物浇水。植物排成一行，从左到右进行标记，编号从 `0` 到 `n - 1` 。其中，第 `i` 株植物的位置是 `x = i` 。

每一株植物都需要浇特定量的水。Alice 和 Bob 每人有一个水罐，**最初是满的** 。他们按下面描述的方式完成浇水：

-  Alice 按 **从左到右** 的顺序给植物浇水，从植物 `0` 开始。Bob 按 **从右到左** 的顺序给植物浇水，从植物 `n - 1` 开始。他们 **同时** 给植物浇水。
- 无论需要多少水，为每株植物浇水所需的时间都是相同的。
- 如果 Alice/Bob 水罐中的水足以 **完全** 灌溉植物，他们 **必须** 给植物浇水。否则，他们 **首先**（立即）重新装满罐子，然后给植物浇水。
- 如果 Alice 和 Bob 到达同一株植物，那么当前水罐中水 **更多** 的人会给这株植物浇水。如果他俩水量相同，那么 Alice 会给这株植物浇水。

给你一个下标从 **0** 开始的整数数组 `plants` ，数组由 `n` 个整数组成。其中，`plants[i]` 为第 `i` 株植物需要的水量。另有两个整数 `capacityA` 和 `capacityB` 分别表示 Alice 和 Bob 水罐的容量。返回两人浇灌所有植物过程中重新灌满水罐的 **次数** 。

 

```java
class Solution2105{
    public int minimumRefill(int[] plants, int capacityA, int capacityB){
        int ans = 0;
        int a = capacityA;
        int b = capacityB;
        int i =0,j = plants.length-1;
        while (i<j){
            if (a<plants[i]){
                ans++;
                a = capacityA;
            }
            a -=plants[i++];
            if (b<plants[j]){
                ans++;
                b  = capacityB;
            }
            b -=plants[j--];
        }
        if (i==j&&Math.max(a,b)<plants[i]){
            ans++;
        }
        return ans;
    }
}
```

一看他们一个从左开始一个从右开始

一看就是相向双指针

如果水不够，就再装一瓶

够的话，就直接交就行

然后相遇的时候i=j了，直接比大小就行

## [977. 有序数组的平方](https://leetcode.cn/problems/squares-of-a-sorted-array/)

给你一个按 **非递减顺序** 排序的整数数组 `nums`，返回 **每个数字的平方** 组成的新数组，要求也按 **非递减顺序** 排序。

```java
class Solution977{
    public int[] sortedSquares(int[] nums){
        int n = nums.length;
        int[] ans = new int[n];
        int i =0,j=n-1;
        for (int p =n-1;p>=0;p--){
            int x = nums[i]* nums[i];
            int y = nums[j]*nums[j];
            if (x>y){
                ans[p] = x;
                i++;
            }else {
                ans[p] = y;
                j--;
            }
        }
        return ans;
    }
}
```

简单的双指针，从后面开始遍历，大的就留下，然后指针移动

最后返回ans数组

## [658. 找到 K 个最接近的元素](https://leetcode.cn/problems/find-k-closest-elements/)

给定一个 **排序好** 的数组 `arr` ，两个整数 `k` 和 `x` ，从数组中找到最靠近 `x`（两数之差最小）的 `k` 个数。返回的结果必须要是按升序排好的。

整数 `a` 比整数 `b` 更接近 `x` 需要满足：

- `|a - x| < |b - x|` 或者
- `|a - x| == |b - x|` 且 `a < b`

二分写法：

```java
class Solution658B{
    public List<Integer> findClosestElements(int[] arr, int k, int x){
        int n = arr.length;
        List<Integer> list = new ArrayList<>();
        int left = 0,right = n-k;
        while (left<=right){
            int mid = (left+right)>>>1;
            if (mid+k<n&&x-arr[mid]>arr[mid+k]-x){
                left = mid+1;
            }else {
                right = mid-1;
            }
        }
        for (int i =left;i<left+k;i++){
            list.add(arr[i]);
        }
        return list;
    }

}
```

双指针写法：

```java
class Solution658B{
    public List<Integer> findClosestElementsA(int[] arr, int k, int x){
        int n = arr.length;
        List<Integer> list = new ArrayList<>();
        int left = 0,right = n-k;
        while (left<=right){
            int mid = (left+right)>>>1;
            if (mid+k<n&&x-arr[mid]>arr[mid+k]-x){
                left = mid+1;
            }else {
                right = mid-1;
            }
        }
        for (int i =left;i<left+k;i++){
            list.add(arr[i]);
        }
        return list;
    }
    public List<Integer> findClosestElements(int[] arr, int k, int x){
        int n = arr.length;
        List<Integer> list = new ArrayList<>();
        int left = 0,right = n-1;
        int del  = n-k;
        while (del>0){
            if (x-arr[left]>arr[right]-x){
                left++;
            }else {
                right--;
            }
            del--;
        }
        for (int i =left;i<left+k;i++){
            list.add(arr[i]);
        }
        return list;
    }
}
```

其实大同小异哈啊

## [1471. 数组中的 k 个最强值](https://leetcode.cn/problems/the-k-strongest-values-in-an-array/)

给你一个整数数组 `arr` 和一个整数 `k` 。

设 `m` 为数组的中位数，只要满足下述两个前提之一，就可以判定 `arr[i]` 的值比 `arr[j]` 的值更强：

-  `|arr[i] - m| > |arr[j] - m|`
-  `|arr[i] - m| == |arr[j] - m|`，且 `arr[i] > arr[j]`

请返回由数组中最强的 `k` 个值组成的列表。答案可以以 **任意顺序** 返回。

**中位数** 是一个有序整数列表中处于中间位置的值。形式上，如果列表的长度为 `n` ，那么中位数就是该有序列表（下标从 0 开始）中位于 `((n - 1) / 2)` 的元素。

- 例如 `arr = [6, -3, 7, 2, 11]`，`n = 5`：数组排序后得到 `arr = [-3, 2, 6, 7, 11]` ，数组的中间位置为 `m = ((5 - 1) / 2) = 2` ，中位数 `arr[m]` 的值为 `6` 。
- 例如 `arr = [-7, 22, 17, 3]`，`n = 4`：数组排序后得到 `arr = [-7, 3, 17, 22]` ，数组的中间位置为 `m = ((4 - 1) / 2) = 1` ，中位数 `arr[m]` 的值为 `3` 。



```java
class Solution1471{
    public int[] getStrongest(int[] arr, int k){
        int n  = arr.length;
        Arrays.sort(arr);
       int m = arr[(n-1)/2];
       int left = 0,right = n-1;
       int[] ans = new int[k];
      while (k-->0){
           if (m-arr[left]>=arr[right]-m){
               ans[k] = arr[left++];
           }else {
               ans[k] = arr[right--];
           }
       }
       return ans;
    }
}
```

使用双指针

如果比较他们谁更强力，就加入到数组之中。然后指针移动

## [167. 两数之和 II - 输入有序数组](https://leetcode.cn/problems/two-sum-ii-input-array-is-sorted/)



给你一个下标从 **1** 开始的整数数组 `numbers` ，该数组已按 **非递减顺序排列** ，请你从数组中找出满足相加之和等于目标数 `target` 的两个数。如果设这两个数分别是 `numbers[index1]` 和 `numbers[index2]` ，则 `1 <= index1 < index2 <= numbers.length` 。

以长度为 2 的整数数组 `[index1, index2]` 的形式返回这两个整数的下标 `index1` 和 `index2`。

你可以假设每个输入 **只对应唯一的答案** ，而且你 **不可以** 重复使用相同的元素。

你所设计的解决方案必须只使用常量级的额外空间。

```java
class Solution167{
    public int[] twoSum(int[] numbers, int target){
        int left = 0,right = numbers.length-1;
        while (true){
            int s= numbers[left]+numbers[right];
            if (s==target){
                return new int[]{left+1,right+1};
            }
            if (s>target){
                right--;
            }else {
                left++;
            }
        }
    }
}
```

简单的双指针查找，但是注意一个下标从 **1** 开始的整数数组 `numbers`

所以返回的时候返回的式{left+1,right+1}

## [2824. 统计和小于目标的下标对数目](https://leetcode.cn/problems/count-pairs-whose-sum-is-less-than-target/)

给你一个下标从 **0** 开始长度为 `n` 的整数数组 `nums` 和一个整数 `target` ，请你返回满足 `0 <= i < j < n` 且 `nums[i] + nums[j] < target` 的下标对 `(i, j)` 的数目。

```java
class Solution2824{
    public int countPairs(List<Integer> nums, int target){
        Collections.sort(nums);
        int ans = 0,left = 0,right = nums.size()-1;
        while (left<right){
            if (nums.get(left)+nums.get(right)<target){
                ans +=right-left;
                left++;
            }else {
                right--;
            }
        }
        return ans;
    }
}
```

因为式看到两个ij就想到了双指针，然后判断是不是和小于target就行了

统计小于的数目，因为是(i,j)的数目，就是right-left

然后存储到ans中，如果大了就right--;



## [LCP 28. 采购方案](https://leetcode.cn/problems/4xy4Wx/)

小力将 N 个零件的报价存于数组 `nums`。小力预算为 `target`，假定小力仅购买两个零件，要求购买零件的花费不超过预算，请问他有多少种采购方案。

注意：答案需要以 `1e9 + 7 (1000000007)` 为底取模，如：计算初始结果为：`1000000008`，请返回 `1`





```java
class Solutionlcp28{
    public int purchasePlans(int[] nums, int target){
        Arrays.sort(nums);
        int ans = 0,left = 0,right = nums.length-1;
        while (left<right){
            if (nums[left]+nums[right]<target){
                ans +=right-left;
                left++;
            }else {
                right--;
            }
            ans %=1_000_000_007;
        }
        return ans;
    }
}
```

同上，只不过要先mod之后再加入ans总的，要不然数太大会报错

## [1616. 分割两个字符串得到回文串](https://leetcode.cn/problems/split-two-strings-to-make-palindrome/)



给你两个字符串 `a` 和 `b` ，它们长度相同。请你选择一个下标，将两个字符串都在 **相同的下标** 分割开。由 `a` 可以得到两个字符串： `aprefix` 和 `asuffix` ，满足 `a = aprefix + asuffix` ，同理，由 `b` 可以得到两个字符串 `bprefix` 和 `bsuffix` ，满足 `b = bprefix + bsuffix` 。请你判断 `aprefix + bsuffix` 或者 `bprefix + asuffix` 能否构成回文串。

当你将一个字符串 `s` 分割成 `sprefix` 和 `ssuffix` 时， `ssuffix` 或者 `sprefix` 可以为空。比方说， `s = "abc"` 那么 `"" + "abc"` ， `"a" + "bc" `， `"ab" + "c"` 和 `"abc" + ""` 都是合法分割。

如果 **能构成回文字符串** ，那么请返回 `true`，否则返回 `false` 。

**注意**， `x + y` 表示连接字符串 `x` 和 `y` 。

```java
class Solution1616{
    private boolean isPalindrome(String s, int i, int j){
        while (i<j&&s.charAt(i)==s.charAt(j)){
            ++i;
            --j;
        }
        return i>=j;
    }
    private boolean check(String a, String b){
        int i =0,j=a.length()-1;
        while (i<j&&a.charAt(i)==b.charAt(j)){
            ++i;
            --j;
        }
        return isPalindrome(a,i,j)||isPalindrome(b,i,j);
    }
    public boolean checkPalindromeFormation(String a, String b){
        return check(a,b)||check(b,a);
    }
}
```

判断是不是回文的方法：

```java
private boolean isPalindrome(String s, int i, int j){
    while (i < j && s.charAt(i) == s.charAt(j)){
        ++i;
        --j;
    }
    return i >= j;
}

```

如果 `i >= j`，说明是回文。

然后判断a,b是否能连成一个回文

一旦不相等，就说明进入中间的部分了，尝试检查剩余部分是否是回文（即 `a[i..j]` 或 `b[i..j]`）；

为可以选择 `a+b` 或 `b+a` 两种拼接方式，因此主函数里判断了 `check(a, b)` 和 `check(b, a)` 两种情况。

## [905. 按奇偶排序数组](https://leetcode.cn/problems/sort-array-by-parity/)



给你一个整数数组 `nums`，将 `nums` 中的的所有偶数元素移动到数组的前面，后跟所有奇数元素。

返回满足此条件的 **任一数组** 作为答案。

使用双指针，交换最左边的奇数和最右边的偶数

然后重复这个过程

```java
class Solution905{
    public int[] sortArrayByParity(int[] nums){
        int i =0,j=nums.length-1;
        while (i<j){
            if (nums[i]%2==0){
                i++;
            }
            else if (nums[j]%2==1){
                j--;
            }else {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
                i++;
                j--;
            }
        }
        return nums;
    }
}
```

[922. 按奇偶排序数组 II](https://leetcode.cn/problems/sort-array-by-parity-ii/)

给定一个非负整数数组 `nums`， `nums` 中一半整数是 **奇数** ，一半整数是 **偶数** 。

对数组进行排序，以便当 `nums[i]` 为奇数时，`i` 也是 **奇数** ；当 `nums[i]` 为偶数时， `i` 也是 **偶数** 。

你可以返回 *任何满足上述条件的数组作为答案* 。

```java
class Solution922{
    public int[] sortArrayByParityII(int[] nums){
        int i=0;int j =1;
        while (i<nums.length){
            if (nums[i]%2==0){
                i +=2;
            }else if (nums[j]%2==1){
                j +=2;
            }
            else {
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j] = tmp;
                i +=2;
                j+=2;
            }
        }
        return nums;
    }
}
```

跟上面相同，只不过是跳着来的



# 同向双指针

两个指针的移动方向相同（都向右，或者都向左）。

## [611. 有效三角形的个数](https://leetcode.cn/problems/valid-triangle-number/)

给定一个包含非负整数的数组 `nums` ，返回其中可以组成三角形三条边的三元组个数。

这里是遍历，应用的规则是两边之和大于第三边

a+b>c

也就是说c-a<b

然后c是大的，a是小的，的话

a=num[i]的话，c=i+2,b=i+1

```java
class Solution611{
    public int triangleNumber(int[] nums){
        Arrays.sort(nums);
        int n = nums.length;
        int ans=  0;
        for (int i =0;i<n;i++){
            int a = nums[i];
            if (a==0){
                continue;
            }
            int j = i+1;
            for (int k=i+2;k<n;k++){
                while (nums[k]-nums[j]>=a){
                    j++;
                }
                ans +=k-j;
            }
        }
        return ans;
    }

}
```

然后逐个遍历即可

然后当大于了也就说当前的b太小了，j++

最后的数目是c-b



## [581. 最短无序连续子数组](https://leetcode.cn/problems/shortest-unsorted-continuous-subarray/)

个整数数组 `nums` ，你需要找出一个 **连续子数组** ，如果对这个子数组进行升序排序，那么整个数组都会变为升序排序。

请你找出符合题意的 **最短** 子数组，并输出它的长度。

如果进入了右段，就没有比最大值小的数，所以最后一个比最大值小的数就是中段的右边界，同理，如果进入左段，就不会出现比最小值更大的情况，所以最后一个出现就视为中段左边界

```java
class Solution {
    public int findUnsortedSubarray(int[] nums) {
        int n = nums.length;
        int max = Integer.MIN_VALUE;
        int min = Integer.MAX_VALUE;
        int left = -1, right = -1;

        // 从左往右找右边界
        for (int i = 0; i < n; i++) {
            if (nums[i] >= max) {
                max = nums[i];
            } else {
                right = i; // 当前值小于 max，说明乱序
            }
        }

        // 从右往左找左边界
        for (int i = n - 1; i >= 0; i--) {
            if (nums[i] <= min) {
                min = nums[i];
            } else {
                left = i; // 当前值大于 min，说明乱序
            }
        }

        return right == -1 ? 0 : right - left + 1;
    }
}

```

## [1574. 删除最短的子数组使剩余数组有序](https://leetcode.cn/problems/shortest-subarray-to-be-removed-to-make-array-sorted/)



给你一个整数数组 `arr` ，请你删除一个子数组（可以为空），使得 `arr` 中剩下的元素是 **非递减** 的。

一个子数组指的是原数组中连续的一个子序列。

请你返回满足题目要求的最短子数组的长度。

```java
class Solution1574{
    public int findLengthOfShortestSubarray(int[] arr){
        int n  =arr.length,right = n-1;
        //找到第一个下降的位置
        while (right>0&&arr[right-1]<=arr[right]){
            right--;
        }
        if (right==0) return 0;
        int ans = right;
        for (int left = 0;left<n ; ++left){
            if (left>0&&arr[left]<arr[left-1]) break;//保证left是递增的
            while (right<n&&arr[right]<arr[left]){
                right++;//找到right保证arr[left]<=arr[right]
            }
            //删除 arr[left+1..right-1] 的长度为 right - left - 1
            ans = Math.min(ans,right-left-1);
        }
        return ans;
    }
}
```

这样的话0-left是增的 right-n-1是增的

left-right是 arr[left]<=arr[right]这样保证他们是增的

删除中间的就是一个非递减的数组了

[left+1,right-1];

# 背向双指针

## [1793. 好子数组的最大分数](https://leetcode.cn/problems/maximum-score-of-a-good-subarray/)

给你一个整数数组 `nums` **（下标从 0 开始）**和一个整数 `k` 。

一个子数组 `(i, j)` 的 **分数** 定义为 `min(nums[i], nums[i+1], ..., nums[j]) * (j - i + 1)` 。一个 **好** 子数组的两个端点下标需要满足 `i <= k <= j` 。

请你返回 **好** 子数组的最大可能 **分数** 。

```java
class Solution1793{
    public int maximumScore(int[] nums, int k){
        int n  = nums.length;
        int ans = nums[k],minH = nums[k];
        int i=k,j=k;
        for (int t = 0;t<n-1;t++){
            if (j==n-1||i>0&&nums[i-1]>nums[j+1]){
                minH = Math.min(minH,nums[--i]);
            }else {
                minH = Math.min(minH,nums[++j]);
            }
            ans = Math.max(ans,minH*(j-i+1));
        }
        return ans;
    }
}
```

就是i和j同时从k出发

--i和++j

比较 *nums*[*i*−1] 和 *nums*[*j*+1] 的大小，谁大就移动谁（一样大移动哪个都可以）。

> 按照这种移动方式，一定会在某个时刻恰好满足 *i*=*L* 且 *j*=*R*。
>
> 如果 i 先到达 L，那么此时 j<R。设 L 到 R 之间的最小元素为 m，在方法一中我们知道 nums[L−1]<m，由于 nums[i−1]=nums[L−1]<m≤nums[j+1]，那么后续一定是 j 一直向右移动到 R。对于 j 先到达 R 的情况也同理。所以一定会在某个时刻恰好满足 i=L 且 j=R。
>

# 原地修改（栈的思想）

[27. 移除元素](https://leetcode.cn/problems/remove-element/)

给你一个数组 `nums` 和一个值 `val`，你需要 **[原地](https://baike.baidu.com/item/原地算法)** 移除所有数值等于 `val` 的元素。元素的顺序可能发生改变。然后返回 `nums` 中与 `val` 不同的元素的数量。

假设 `nums` 中不等于 `val` 的元素数量为 `k`，要通过此题，您需要执行以下操作：

- 更改 `nums` 数组，使 `nums` 的前 `k` 个元素包含不等于 `val` 的元素。`nums` 的其余元素和 `nums` 的大小并不重要。
- 返回 `k`。

**把 *nums* 视作一个栈，把不等于 *val* 的元素入栈，最后返回栈的大小。**

```java
class Solution27{
    public int removeElement(int[] nums, int val){
        int stackSize = 0;
        for (int x:nums){
            if (x!=val){
                nums[stackSize++] =x;
            }
        }
        return stackSize;
    }
}
```

经典，不等于的就入栈，然后长度++

最后返回size

## [26. 删除有序数组中的重复项](https://leetcode.cn/problems/remove-duplicates-from-sorted-array/)



给你一个 **非严格递增排列** 的数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使每个元素 **只出现一次** ，返回删除后数组的新长度。元素的 **相对顺序** 应该保持 **一致** 。然后返回 `nums` 中唯一元素的个数。

考虑 `nums` 的唯一元素的数量为 `k` ，你需要做以下事情确保你的题解可以被通过：

- 更改数组 `nums` ，使 `nums` 的前 `k` 个元素包含唯一元素，并按照它们最初在 `nums` 中出现的顺序排列。`nums` 的其余元素与 `nums` 的大小不重要。
- 返回 `k` 。

```java
class Solution26A{
    public int removeDuplicates(int[] nums){
        int k = 1;
        for (int i =1;i<nums.length;i++){
            if (nums[i]!=nums[i-1]){
                nums[k++] = nums[i];
            }
        }
        return k;
    }
}
```

简单的遍历

## [80. 删除有序数组中的重复项 II](https://leetcode.cn/problems/remove-duplicates-from-sorted-array-ii/)



给你一个有序数组 `nums` ，请你**[ 原地](http://baike.baidu.com/item/原地算法)** 删除重复出现的元素，使得出现次数超过两次的元素**只出现两次** ，返回删除后数组的新长度。

不要使用额外的数组空间，你必须在 **[原地 ](https://baike.baidu.com/item/原地算法)修改输入数组** 并在使用 O(1) 额外空间的条件下完成。

 

使用栈的思想去使用

```java
class Solution80A{
    public int removeDuplicates(int[] nums){
        int stackSize = 2;
        for (int i =2;i<nums.length;i++){
            if (nums[i]!=nums[stackSize-2]){
                nums[stackSize++] = nums[i];
            }
        }
        return Math.min(stackSize,nums.length);
    }
}
```

因为是不能多于两次，所以是和栈顶-2的元素去比较

## [283. 移动零](https://leetcode.cn/problems/move-zeroes/)

给定一个数组 `nums`，编写一个函数将所有 `0` 移动到数组的末尾，同时保持非零元素的相对顺序。

**请注意** ，必须在不复制数组的情况下原地对数组进行操作。

交换排序：

```java
class Solution {
    public void moveZeroes(int[] nums){
        if (nums == null){
            return;
        }
        int j =0;
        for (int i=0;i<nums.length;i++){
            if (nums[i]!=0){
                int tmp = nums[i];
                nums[i] = nums[j];
                nums[j++] = tmp;
            }
        }
    }
}
```

遇到0就交换

栈的思想：

```java
class Solution283{
    public void moveZeroes(int[] nums){
        int stackSize = 0;
        for (int x:nums){
            if (x!=0){
                nums[stackSize++] = x;
            }
        }
        Arrays.fill(nums,stackSize,nums.length,0);
    }
}
```

## [1089. 复写零](https://leetcode.cn/problems/duplicate-zeros/)



给你一个长度固定的整数数组 `arr` ，请你将该数组中出现的每个零都复写一遍，并将其余的元素向右平移。

注意：请不要在超过该数组长度的位置写入元素。请对输入的数组 **就地** 进行上述修改，不要从函数返回任何东西。

 

```java
class Solution1089{
    public void duplicateZeros(int[] arr){
        int n  = arr.length;
        int countZeros = 0;
        for (int i =0;i<n;i++){
            if (arr[i] ==0){
                countZeros++;
            }
        }
        int i = n-1;
        int j = n+countZeros-1;
        while (i>=0){
            if (arr[i]==0){
                if (j<n) arr[j] = 0;
                j--;
            }
            if (j<n){
                arr[j] = arr[i];
            }
            i--;
            j--;
        }
    }

```

先去统计0的次数

然后i为原数组的最后一位

j为新数组的最后一位

i>=0时且这个位为0的话

然后将零复制到目标位置，并且目标位置要前移两位

将非零元素复制到目标位置，并且目标位置前移一位

这个再图上画一下就好理解多了



## [442. 数组中重复的数据](https://leetcode.cn/problems/find-all-duplicates-in-an-array/)



给你一个长度为 `n` 的整数数组 `nums` ，其中 `nums` 的所有整数都在范围 `[1, n]` 内，且每个整数出现 **最多****两次** 。请你找出所有出现 **两次** 的整数，并以数组形式返回。

你必须设计并实现一个时间复杂度为 `O(n)` 且仅使用常量额外空间（不包括存储输出所需的空间）的算法解决此问题。



```java
class Solution {
    public List<Integer> findDuplicates(int[] nums) {
        // 原地数组法
        // List<Integer> list = new ArrayList<>();
        // for(int i = 0;i < nums.length;i++){
        //     int cur = Math.abs(nums[i]);
        //     int index = cur - 1;
        //     if(nums[index] < 0){
        //         list.add(cur);
        //     }else{
        //         nums[index] *= -1;
        //     }
        // }
        // return list;

        int[] map = new int[nums.length + 1];
        for (int item : nums) {
            map[item]++;
        }
        List<Integer> res=new ArrayList<>();
        for (int i = 0; i < map.length; ++i) {
            if (map[i] == 2) {
                res.add(i);

            }
```

hash表是可以的哈哈

## [448. 找到所有数组中消失的数字](https://leetcode.cn/problems/find-all-numbers-disappeared-in-an-array/)



给你一个含 `n` 个整数的数组 `nums` ，其中 `nums[i]` 在区间 `[1, n]` 内。请你找出所有在 `[1, n]` 范围内但没有出现在 `nums` 中的数字，并以数组的形式返回结果。

```java
class Solution448{
    public List<Integer> findDisappearedNumbers(int[] nums){
        List<Integer> res = new ArrayList<Integer>();
        for (int i =0;i< nums.length;++i){
            int index= Math.abs(nums[i])-1;
            if (nums[index]>0){
                nums[index] *=-1;
            }
        }
        for (int i=0;i<nums.length;++i){
            if (nums[i]>0){
                res.add(i+1);
            }
        }
        return res;
    }
}
```

在这里和上面那个题都用到了同一个方法

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

# 双序列双指针

## [2109. 向字符串添加空格](https://leetcode.cn/problems/adding-spaces-to-a-string/)



给你一个下标从 **0** 开始的字符串 `s` ，以及一个下标从 **0** 开始的整数数组 `spaces` 。

数组 `spaces` 描述原字符串中需要添加空格的下标。每个空格都应该插入到给定索引处的字符值 **之前** 。

- 例如，`s = "EnjoyYourCoffee"` 且 `spaces = [5, 9]` ，那么我们需要在 `'Y'` 和 `'C'` 之前添加空格，这两个字符分别位于下标 `5` 和下标 `9` 。因此，最终得到 `"Enjoy ***Y***our ***C***offee"` 。

请你添加空格，并返回修改后的字符串*。*

```java
 class  Solution2109{
     public String addSpaces(String s, int[] spaces){
         StringBuilder ans = new StringBuilder(s.length()+spaces.length);
         int j= 0;
         for (int i = 0;i<s.length();i++){
             if (j<spaces.length&&spaces[j]==i){
                 ans.append(' ');
                 j++;
             }
             ans.append(s.charAt(i));
         }
         return ans.toString();
     }
 }
```

就是两个指针，I指针指向的是字符串s,然后J指针指向的是数组

当j<spaces.lenght且spaces[j]==i的时候

这个时候加入' '；

然后再就加入s[i]；

**over**



## [2540. 最小公共值](https://leetcode.cn/problems/minimum-common-value/)



给你两个整数数组 `nums1` 和 `nums2` ，它们已经按非降序排序，请你返回两个数组的 **最小公共整数** 。如果两个数组 `nums1` 和 `nums2` 没有公共整数，请你返回 `-1` 。

如果一个整数在两个数组中都 **至少出现一次** ，那么这个整数是数组 `nums1` 和 `nums2` **公共** 的。

```java
class Solution2540{
    public int getCommon(int[] nums1, int[] nums2){
        int i =0,j=0;
        while (i<nums1.length&&j<nums2.length){
            int a = nums1[i],b = nums2[j];
            if (a==b) return a;
            if (a<b) i++;
            else j++;
        }
        return -1;
    }

}
```

双指针写法，一个遍历nums1，一个遍历nums2

然后相等就返回，因为是按顺序排列的，所以第一个返回的就是最小的

然后直接返回就行

然后根据情况移动指针

## [88. 合并两个有序数组](https://leetcode.cn/problems/merge-sorted-array/)



给你两个按 **非递减顺序** 排列的整数数组 `nums1` 和 `nums2`，另有两个整数 `m` 和 `n` ，分别表示 `nums1` 和 `nums2` 中的元素数目。

请你 **合并** `nums2` 到 `nums1` 中，使合并后的数组同样按 **非递减顺序** 排列。

**注意：**最终，合并后数组不应由函数返回，而是存储在数组 `nums1` 中。为了应对这种情况，`nums1` 的初始长度为 `m + n`，其中前 `m` 个元素表示应合并的元素，后 `n` 个元素为 `0` ，应忽略。`nums2` 的长度为 `n` 。

```java
class Solution88{
    public void merge(int[] nums1, int m, int[] nums2, int n){
        int p1 = m-1;
        int p2 = n-1;
        int p = m+n-1;
        while (p2>=0){
            if (p1>=0&&nums1[p1]>nums2[p2]){
                nums1[p--] = nums1[p1--];
            }
            else {
                nums1[p--] = nums2[p2--];
            }
        }
    }
}
```

合并的时候从后面开始看

大的先排进去就行

然后指针移动

## [LCP 18. 早餐组合](https://leetcode.cn/problems/2vYnGI/)



小扣在秋日市集选择了一家早餐摊位，一维整型数组 `staple` 中记录了每种主食的价格，一维整型数组 `drinks` 中记录了每种饮料的价格。小扣的计划选择一份主食和一款饮料，且花费不超过 `x` 元。请返回小扣共有多少种购买方案。

注意：答案需要以 `1e9 + 7 (1000000007)` 为底取模，如：计算初始结果为：`1000000008`，请返回 `1`



```java
class SolutionLCP88{
    public int breakfastNumber(int[] staple, int[] drinks, int x){
        Arrays.sort(staple);
        Arrays.sort(drinks);
        int mod = 1_000_000_007;
        int res = 0;
        int j  = drinks.length-1;
        
        for (int i=0;i<staple.length;i++){
            if (staple[i]>x) break;
            while (j>=0&&staple[i]+drinks[j]>x){
                j--;
            }
            res = (res+(j+1))%mod;
        }
        return res;
    }
}
```

就双指针遍历

使s[i]+d[j]<=x

然后此时 j+1 个饮料都能与 staple[i] 搭配

## [1855. 下标对中的最大距离](https://leetcode.cn/problems/maximum-distance-between-a-pair-of-values/)



给你两个 **非递增** 的整数数组 `nums1` 和 `nums2` ，数组下标均 **从 0 开始** 计数。

下标对 `(i, j)` 中 `0 <= i < nums1.length` 且 `0 <= j < nums2.length` 。如果该下标对同时满足 `i <= j` 且 `nums1[i] <= nums2[j]` ，则称之为 **有效** 下标对，该下标对的 **距离** 为 `j - i` 。

返回所有 **有效** 下标对 `(i, j)` 中的 **最大距离** 。如果不存在有效下标对，返回 `0` 。

一个数组 `arr` ，如果每个 `1 <= i < arr.length` 均有 `arr[i-1] >= arr[i]` 成立，那么该数组是一个 **非递增** 数组。



简单的双指针

```java
class Solution1855 {
    public int maxDistance(int[] nums1, int[] nums2) {
        int p1 = 0;
        int p2 = 0;
        int res = 0;
        while (p1 < nums1.length && p2 <nums2.length){
            if(nums1[p1] > nums2[p2]){  //无效
                if(p1 == p2){
                    p1++;
                    p2++;
                }else p1++;
            }else {     //有效
                res =Math.max(res,p2-p1);
                p2++;
            }
        }
        return res;
    }
}
```

## [925. 长按键入](https://leetcode.cn/problems/long-pressed-name/)



你的朋友正在使用键盘输入他的名字 `name`。偶尔，在键入字符 `c` 时，按键可能会被*长按*，而字符可能被输入 1 次或多次。

你将会检查键盘输入的字符 `typed`。如果它对应的可能是你的朋友的名字（其中一些字符可能被长按），那么就返回 `True`

```java
class Solution925{
    public boolean isLongPressedName(String name, String typed){
        int i=0,j=0;
        while (j<typed.length()){
            if (i<name.length()&&typed.charAt(j) ==name.charAt(i)){
                i++;
                j++;
            }else if (j>0&&typed.charAt(j)==typed.charAt(j-1)){
                j++;
            }else {
                return false;
            }
        }
        return i ==name.length();
    }
}
```

简单的双指针问题，一个指针在name一个在type

如果i=j的话，就继续

如果j=j-1的话，说明重复了，跳过这个继续遍历

最后i=name.length说明遍历完正好这长度，返回true

不是的话返回false

## [2337. 移动片段得到字符串](https://leetcode.cn/problems/move-pieces-to-obtain-a-string/)



给你两个字符串 `start` 和 `target` ，长度均为 `n` 。每个字符串 **仅** 由字符 `'L'`、`'R'` 和 `'_'` 组成，其中：

- 字符 `'L'` 和 `'R'` 表示片段，其中片段 `'L'` 只有在其左侧直接存在一个 **空位** 时才能向 **左** 移动，而片段 `'R'` 只有在其右侧直接存在一个 **空位** 时才能向 **右** 移动。
- 字符 `'_'` 表示可以被 **任意** `'L'` 或 `'R'` 片段占据的空位。

如果在移动字符串 `start` 中的片段任意次之后可以得到字符串 `target` ，返回 `true` ；否则，返回 `false` 。

```java
class Solution2337{
    public boolean canChange(String start, String target){
        if (!start.replace("_","").equals(target.replace("_",""))){
            return false;
        }
        for (int i =0,j=0;i<start.length();i++){
            if (start.charAt(i)=='_'){
                continue;
            }
            while (target.charAt(j)=='_'){
                j++;
            }
            if (i!=j&&(start.charAt(i)=='L')==(i<j)){
                return false;
            }
            j++;
        }
        return true;
    }
}
```



这样正过来想，是有些困难的。那么我们可以反过来思考，要他们相等的话，去掉_应该是相同的，如果不相同返回false

然后进行双序列双指针遍历

如果是L且i<j L不能往右移动，不能i==j所以返回false

如果是R，且i>j R不能往左移动，不能i==j，返回false

这样检查完，如果可以的话，就返回true

在这里的话，(start.charAt(i)=='L')====(i<j)这一个句子包括了L的情况和R的情况

# 判断子序列

## [392. 判断子序列](https://leetcode.cn/problems/is-subsequence/)



给定字符串 **s** 和 **t** ，判断 **s** 是否为 **t** 的子序列。

字符串的一个子序列是原始字符串删除一些（也可以不删除）字符而不改变剩余字符相对位置形成的新字符串。（例如，`"ace"`是`"abcde"`的一个子序列，而`"aec"`不是）。

**进阶：**

如果有大量输入的 S，称作 S1, S2, ... , Sk 其中 k >= 10亿，你需要依次检查它们是否为 T 的子序列。在这种情况下，你会怎样改变代码？

```java
class Solution392{
    public boolean isSubsequence(String s, String t){
        if (s.isEmpty()) return true;
        int i = 0;
        for (char c:t.toCharArray()){
            if (s.charAt(i)==c&&++i==s.length()){
                return true;
            }
        }
        return false;
    }

}
```

就是简单的双指针，但是灵神这个方法也太简单了吧哈哈

## [524. 通过删除字母匹配到字典里最长单词](https://leetcode.cn/problems/longest-word-in-dictionary-through-deleting/)



给你一个字符串 `s` 和一个字符串数组 `dictionary` ，找出并返回 `dictionary` 中最长的字符串，该字符串可以通过删除 `s` 中的某些字符得到。

如果答案不止一个，返回长度最长且字母序最小的字符串。如果答案不存在，则返回空字符串。



```java
class Solution524{
    public boolean isSubsequence(String t, String s){
        int indext = 0,indexs=0;
        while (indext<t.length()&&indexs<s.length()){
            if (t.charAt(indext)==s.charAt(indexs)){
                indext++;
            }
            indexs++;
        }
        return indext ==t.length();
    }
    public String findLongestWord(String s, List<String> d){
        String result = "";
        for (String t:d){
            if (isSubsequence(t,s)){
                if (result.length()<t.length()||(result.length()==t.length()&&result.compareTo(t)>0)) {
                    result = t;
                }
            }
        }
        return result;
    }
}

```

使用双指针来做的话，遇到相同的就继续检查下一个，大家指针都移动，不相同就字典的自己移动。最后返回是否能检查完，就是指针到最后的末尾了

然后编写主方法，遍历字符，在result>0不为空的时候输出

## [2486. 追加字符以获得子序列](https://leetcode.cn/problems/append-characters-to-string-to-make-subsequence/)



给你两个仅由小写英文字母组成的字符串 `s` 和 `t` 。

现在需要通过向 `s` 末尾追加字符的方式使 `t` 变成 `s` 的一个 **子序列** ，返回需要追加的最少字符数。

子序列是一个可以由其他字符串删除部分（或不删除）字符但不改变剩下字符顺序得到的字符串。

```java
class Solution2486{
    public int appendCharacters(String s, String t){
        char[]cp = t.toCharArray();
        char[]cs = s.toCharArray();
        int k =0,n=cp.length;
        for (char c:cs){
            if (k<n&&cp[k]==c) k++;
        }
        return n-k;
    }
}

```

把他们都转为数组，然后遍历，如果cp[k]=cs里面的c的话，计数器k++;

然后到最后一个相等的地方，然后n-k就是需要追加的数量

## [2825. 循环增长使字符串子序列等于另一个字符串](https://leetcode.cn/problems/make-string-a-subsequence-using-cyclic-increments/)



给你一个下标从 **0** 开始的字符串 `str1` 和 `str2` 。

一次操作中，你选择 `str1` 中的若干下标。对于选中的每一个下标 `i` ，你将 `str1[i]` **循环** 递增，变成下一个字符。也就是说 `'a'` 变成 `'b'` ，`'b'` 变成 `'c'` ，以此类推，`'z'` 变成 `'a'` 。

如果执行以上操作 **至多一次** ，可以让 `str2` 成为 `str1` 的子序列，请你返回 `true` ，否则返回 `false` 。

**注意：**一个字符串的子序列指的是从原字符串中删除一些（可以一个字符也不删）字符后，剩下字符按照原本先后顺序组成的新字符串。



```java
class Solution{
    public boolean canMakeSubsequence(String str1, String str2){
        char[] s1  = str1.toCharArray();
        char[] s2 = str2.toCharArray();
        int i=0,j=0;
        while (i<s1.length&&j<s2.length){
            char a  =s1[i];
            char b = s2[j];
            if (a==b||(a-'a'+1)%26==(b-'a')){
                j++;
            }
            i++;
        }
        return j==s2.length;
    }
}
```

还是标准的子序列问题，两个指针移动，当满足条件的时候子序列的指针移动

然后字典的一直移动

看最后子序列的指针能不能到末尾，到了就说明是，反则不是

## [1023. 驼峰式匹配](https://leetcode.cn/problems/camelcase-matching/)



给你一个字符串数组 `queries`，和一个表示模式的字符串 `pattern`，请你返回一个布尔数组 `answer` 。只有在待查项 `queries[i]` 与模式串 `pattern` 匹配时， `answer[i]` 才为 `true`，否则为 `false`。

如果可以将 **小写字母** 插入模式串 `pattern` 得到待查询项 `queries[i]`，那么待查询项与给定模式串匹配。您可以在模式串中的任何位置插入字符，也可以选择不插入任何字符。



```java
class Solution1023{
    private boolean check(String s,String t){
        int m = s.length(),n = t.length();
        int i  =0,j=0;
        for (;j<n;++i,++j){
            while (i<m&&s.charAt(i)!=t.charAt(j)&&Character.isLowerCase(s.charAt(i))){
                ++i;
            }
            if (i==m||s.charAt(i)!=t.charAt(j)){
                return false;
            }
        }
        while (i<m&&Character.isLowerCase(s.charAt(i))){
            ++i;
        }
        return i==m;
    }
    public List<Boolean> camelMatch(String[] queries, String pattern){
        List<Boolean> ans  = new ArrayList<>();
        for (String q:queries){
            ans.add(check(q,pattern));
        }
        return ans;
    }
}
```

就分别取检测每一个字符串，check这个是不是能符合子序列的要求

然后check检测子序列

不相等，没到s的末尾，小写的化，指针移动跳过

然后指针到了末尾，如果不相等的化就返回false

最后看指针是不是在末尾了



## [522. 最长特殊序列 II](https://leetcode.cn/problems/longest-uncommon-subsequence-ii/)



给定字符串列表 `strs` ，返回其中 **最长的特殊序列** 的长度。如果最长特殊序列不存在，返回 `-1` 。

**特殊序列** 定义如下：该序列为某字符串 **独有的子序列（即不能是其他字符串的子序列）**。

 `s` 的 **子序列**可以通过删去字符串 `s` 中的某些字符实现。

- 例如，`"abc"` 是 `"aebdc"` 的子序列，因为您可以删除`"aebdc"`中的下划线字符来得到 `"abc"` 。`"aebdc"`的子序列还包括`"aebdc"`、 `"aeb"` 和 "" (空字符串)。

因为子序列越长，越不可能是其它字符串的子序列

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

判断子序列的简单算法

```java
class Solution522{
    private boolean isSubseq(String s, String t){
        int i =0;
        for (char c:t.toCharArray()){
            if (s.charAt(i)==c&&++i==s.length()){
                return true;
            }
        }
        return false;
    }
    public int findLUSlength(String[] strs){
        Arrays.sort(strs,(a,b)->b.length()-a.length());
        for (int i =0;i<strs.length;i++){
            boolean isSub = false;
            for (int j  = 0;j<strs.length;j++){
                if (i==j) continue;
                if (isSubseq(strs[i],strs[j])){
                    isSub  = true;
                    break;
                }
            }
            if (!isSub) return strs[i].length();
        }
        return -1;
    }
}
```

就去遍历整个

i==j继续

然后如果是子序列的化，is设为true

然后如果if (!isSub) return strs[i].length();

找到不是任何人的子序列

就返回-1

然后可以返回数量

# 三指针

## [2367. 等差三元组的数目](https://leetcode.cn/problems/number-of-arithmetic-triplets/)



给你一个下标从 **0** 开始、**严格递增** 的整数数组 `nums` 和一个正整数 `diff` 。如果满足下述全部条件，则三元组 `(i, j, k)` 就是一个 **等差三元组** ：

- `i < j < k` ，
- `nums[j] - nums[i] == diff` 且
- `nums[k] - nums[j] == diff`

返回不同 **等差三元组** 的数目*。*

hash表法先来个：

```java
class Solution2367{
    public int arithmeticTriplets(int[] nums, int diff){
        int ans = 0;
        HashSet set = new HashSet<Integer>();
        for (int x:nums) set.add(x);
        for (int x:nums){
            if (set.contains(x-diff)&&set.contains(x+diff))
                ++ans;
        }
        return ans;
    }
}
```

简单的hashset实现

三指针：

```java
class Solution2367{
    public int arithmeticTripletsA(int[] nums, int diff){
        int ans = 0;
        HashSet set = new HashSet<Integer>();
        for (int x:nums) set.add(x);
        for (int x:nums){
            if (set.contains(x-diff)&&set.contains(x+diff))
                ++ans;
        }
        return ans;
    }
    public int arithmeticTriplets(int[] nums, int diff){
        int ans = 0,i=0,j=1;
        for (int x:nums){
            while (nums[j]+diff<x){
                ++j;
            }
            if (nums[j]+diff>x){
                continue;
            }
            while (nums[i]+diff*2<x){
                ++i;
            }
            if (nums[i]+diff*2==x){
                ++ans;
            }
           
        }
        return ans;
    }
}

```

## [2563. 统计公平数对的数目](https://leetcode.cn/problems/count-the-number-of-fair-pairs/)



给你一个下标从 **0** 开始、长度为 `n` 的整数数组 `nums` ，和两个整数 `lower` 和 `upper` ，返回 **公平数对的数目** 。

如果 `(i, j)` 数对满足以下情况，则认为它是一个 **公平数对** ：

- `0 <= i < j < n`，且
- `lower <= nums[i] + nums[j] <= upper`

```java
class Solution2536{
    public long countFairPairs(int[] nums, int lower, int upper){
        Arrays.sort(nums);
        long ans  = 0;
        int l  = nums.length;
        int r  = nums.length;
        for (int j =0;j<nums.length;j++){
            while (r>0&&nums[r-1]>upper-nums[j]){
                r--;
            }
            while (l>0&&nums[l-1]>=lower-nums[j]){
                l--;
            }
            ans +=Math.min(r,j)-Math.min(l,j);
        }
        return ans;
    }
}
```

三指针写法，、

找 >*upper*−*nums*[*j*] 的第一个数

找 ≥*lower*−*nums*[*j*] 的第一个数

使用二分查找的话也是找这两个数

找到这两个数

他们之间的数量就是数目最后的答案

然后累加即可

```
while (r>0&&nums[r-1]>upper-nums[j]){
                r--;
            }
            while (l>0&&nums[l-1]>=lower-nums[j]){
                l--;
            }
```

做到了类似二分查找的功能

# 分组循环

**适用场景**：按照题目要求，数组会被分割成若干组，每一组的判断/处理逻辑是相同的。

核心思想：

外层循环负责遍历组之前的准备工作（记录开始位置），和遍历组之后的统计工作（更新答案最大值）。
内层循环负责遍历组，找出这一组最远在哪结束。
这个写法的好处是，各个逻辑块分工明确，也不需要特判最后一组（易错点）。以我的经验，这个写法是所有写法中最不容易出 bug 的，推荐大家记住。



## [1446. 连续字符](https://leetcode.cn/problems/consecutive-characters/)



给你一个字符串 `s` ，字符串的**「能量」**定义为：只包含一种字符的最长非空子字符串的长度。

请你返回字符串 `s` 的 **能量**。

```java
class Solution1446 {
    public int maxPower(String s) {
        int n = s.length(), ans = 1;
        for (int i = 0; i < n;) {
            int j = i;
            // 内层循环找出从 i 开始这一组字符最远能延伸到哪里
            while (j < n && s.charAt(j) == s.charAt(i)) {
                j++;
            }
            // 统计这一组的长度，更新最大值
            ans = Math.max(ans, j - i);
            // i 指向下一组的开头
            i = j;
        }
        return ans;
    }
}

```

## [1869. 哪种连续子字符串更长](https://leetcode.cn/problems/longer-contiguous-segments-of-ones-than-zeros/)



给你一个二进制字符串 `s` 。如果字符串中由 `1` 组成的 **最长** 连续子字符串 **严格长于** 由 `0` 组成的 **最长** 连续子字符串，返回 `true` ；否则，返回 `false` 。

- 例如，`s = "**11**01**000**10"` 中，由 `1` 组成的最长连续子字符串的长度是 `2` ，由 `0` 组成的最长连续子字符串的长度是 `3` 。

注意，如果字符串中不存在 `0` ，此时认为由 `0` 组成的最长连续子字符串的长度是 `0` 。字符串中不存在 `1` 的情况也适用此规则。

```java
class Solution1869{
    public boolean checkZeroOnes(String s){
        int n  = s.length();
        int max1=  0,max0=0;
        for (int i =0;i<n;){
            int j =i;
            while (j<n&&s.charAt(j)==s.charAt(i)) j++;
            int len=  j-i;

            if (s.charAt(i)==1){
                max1 = Math.max(max1,len);
            }else {
                max0=Math.max(max0,len);
            }
            i=j;
        }
        return max1>max0;
    }
}
```

还是简单的统计长度，是1就归为1，是0就归为0

更新最大值

然后最后看是不是大于。

## [2414. 最长的字母序连续子字符串的长度](https://leetcode.cn/problems/length-of-the-longest-alphabetical-continuous-substring/)



**字母序连续字符串** 是由字母表中连续字母组成的字符串。换句话说，字符串 `"abcdefghijklmnopqrstuvwxyz"` 的任意子字符串都是 **字母序连续字符串** 。

- 例如，`"abc"` 是一个字母序连续字符串，而 `"acb"` 和 `"za"` 不是。

给你一个仅由小写英文字母组成的字符串 `s` ，返回其 **最长** 的 字母序连续子字符串 的长度。

```java
class Solution2414{
    public int longestContinuousSubstring(String S){
        char[] s = S.toCharArray();
        int ans = 1;
        int cnt = 1;
        for (int i=1;i<s.length;i++){
            if (s[i-1]+1==s[i]){
                ans = Math.max(ans,++cnt);
            }else {
                cnt = 1;
            }
        }
        return ans;
    }
}
```

直接遍历，如果s[i-1]+1==s[i]就cnt+1,然后找出ans即可

## [1957. 删除字符使字符串变好](https://leetcode.cn/problems/delete-characters-to-make-fancy-string/)



一个字符串如果没有 **三个连续** 相同字符，那么它就是一个 **好字符串** 。

给你一个字符串 `s` ，请你从 `s` 删除 **最少** 的字符，使它变成一个 **好字符串** 。

请你返回删除后的字符串。题目数据保证答案总是 **唯一的** 。

```java
class Solution1957{
    public String makeFancyString(String s){
        StringBuilder res = new StringBuilder();
        for (char ch:s.toCharArray()){
            int n =res.length();
            if (n>=2&&ch==res.charAt(n-1)&&ch==res.charAt(n-2)){
                continue;
            }
            res.append(ch);
        }
        return res.toString();
    }
}
```

一开始我想的是遇到3个连续的就不管，统计他们数目，一看不是数目

哈哈就变成了是3个连续的就不进入

## [674. 最长连续递增序列](https://leetcode.cn/problems/longest-continuous-increasing-subsequence/)



给定一个未经排序的整数数组，找到最长且 **连续递增的子序列**，并返回该序列的长度。

**连续递增的子序列** 可以由两个下标 `l` 和 `r`（`l < r`）确定，如果对于每个 `l <= i < r`，都有 `nums[i] < nums[i + 1]` ，那么子序列 `[nums[l], nums[l + 1], ..., nums[r - 1], nums[r]]` 就是连续递增子序列。

```java
class Solution674{
    public int findLengthOfLCIS(int[] nums){
        int l =0;
        int r  = 0;
        int len = 0;
        while (r<nums.length){
            if (r==l||nums[r-1]<nums[r]){
                len = Math.max(len,r-l+1);
                r++;
            }else {
                l = r;
            }
        }
        return len;
    }
}

```

就是比较前面一个大于后面的就移动，中断了就从当前这个开始
