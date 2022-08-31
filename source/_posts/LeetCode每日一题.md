---
title: LeetCode每日一题
top: false
cover: false
toc: true
mathjax: false
date: 2022-06-30 23:21:54
author:
img:
coverImg:
password:
summary:
tags: '算法基础'
categories: '算法'
---
# 算法入门

## 1、二分搜索



```text
给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果目标值存在返回下标，否则返回 -1。
```

```java
/*左闭右闭版本*/
class Solution {
    public int search(int[] nums, int target) {
        int left = 0; 
        int right = nums.length-1; //左闭右闭
        // 避免当 target 小于nums[0] 大于nums[nums.length - 1]时多次循环运算
        if (target < nums[0] || target > nums[nums.length - 1]) {
            return -1;
        }
        //开闭边界问题：主要看左右对于的是什么 
        while(left <= right){
            int mid = letf + (right-left)/2;//防止left+right溢出（超出整数范围）。
            if(nums[mid] == target) return mid;
            else if(nums[mid]>target){
                right = mid-1;
            }else{
               left = mid+1; 
            }
        }
        return -1;
    }
}
```

```java
/*左闭右开版本*/
class Solution {
    public int search(int[] nums, int target) {
        int left = 0, right = nums.length;
        //左闭右开
        while (left < right) {
            int mid = left + ((right - left) >> 1);
           // >> 是右移运算，在计算机中是一种运算操作，但是他的运算结果正好能对应一个整数的二分之一值，这就正好能代替数学上的除2运算，但是比除2运算要快。
            if (nums[mid] == target)
                return mid;
            else if (nums[mid] < target)
                left = mid + 1;
            else if (nums[mid] > target)
                right = mid;
        }
        return -1;
    }
}
```

## 2、第一个错误的版本

```tex
你是产品经理，目前正在带领一个团队开发新的产品。不幸的是，你的产品的最新版本没有通过质量检测。由于每个版本都是基于之前的版本开发的，所以错误的版本之后的所有版本都是错的。

假设你有 n 个版本 [1, 2, ..., n]，你想找出导致之后所有版本出错的第一个错误的版本。

你可以通过调用 bool isBadVersion(version) 接口来判断版本号 version 是否在单元测试中出错。实现一个函数来查找第一个错误的版本。你应该尽量减少对调用 API 的次数。
```
```java
//二分通常有以下两种写法，分别代表「找到最靠近中心的 True」 和「找到最靠近中心的 False」
//另外根据数据范围需要注意计算 mid 时的爆 int 问题，可以通过使用类似 l + (r - l) / 2 的做法解决，也可以通过一个临时 long 来解决。

//找到最靠近中心的 True」
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int left = 1, right = n;
        //因为最终缩为一个点 所以不能写left <= right 
        while (left < right) { // 循环直至区间左右端点相同
            int mid = left + (right - left) / 2; // 防止计算时溢出 也可加个long
            if (isBadVersion(mid)) {
                right = mid; // 答案在区间 [left, mid] 中
            } else {
                left = mid + 1; // 答案在区间 [mid+1, right] 中
            }
        }
        // 此时有 left == right，区间缩为一个点，即为答案
        return left;
    }
}

 //和「找到最靠近中心的 False」
public class Solution extends VersionControl {
    public int firstBadVersion(int n) {
        int l = 1, r = n;
        while (l < r) {
            long tmp = (long)l + r + 1 >> 1;
            int mid = (int)tmp;
            if (!isBadVersion(mid)) {
                l = mid;
            } else {
                r = mid - 1;
            }
        }
        return !isBadVersion(r) ? r + 1 : r;
    }
}
```

## 3、搜索插入位置

```tex
给定一个排序数组和一个目标值，在数组中找到目标值，并返回其索引。如果目标值不存在于数组中，返回它将会被按顺序插入的位置。
```

```java
class Solution {
    public int searchInsert(int[] nums, int target) {
        int left = 0,right = nums.length-1;
        while(left <= right){
            int mid = left + (right -left)/2;
            if(nums[mid] > target){
                right = mid - 1;
            }else if (nums[mid] < target){
                left = mid + 1;
            }else{
                return mid;
            }
        }
        return left;
    }
}
```

```Java
//分类讨论版
class Solution {
    public int searchInsert(int[] nums, int target) {
        
        //特殊情况
        if(target<=nums[0]) return 0;
        if(target>nums[nums.length-1])return nums.length;
        if(target==nums[nums.length-1])return nums.length-1;
        int low = 0;
        int high = nums.length-1;
        while(low<high){
            if(nums[(low+high)/2]>target){
                high = (low+high)/2-1;
            }else if(nums[(low+high)/2]<target){
                low = (low+high)/2+1;
            }else{
                return (low+high)/2;
            }
        }
        if(target>nums[low]){
            return low+1;
        }
        return low;
    }
}
```

## 4、有序数组的平方

```te
给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

输入：nums = [-4,-1,0,3,10]
输出：[0,1,9,16,100]
解释：平方后，数组变为 [16,1,0,9,100]
排序后，数组变为 [0,1,9,16,100]
```

```java
//暴力排序法 先平方再次排序
class Solution {
    public int[] sortedSquares(int[] nums) {
        for(int i =0 ; i < nums.length; i++){
            nums[i]*=nums[i];
        }
        Arrays.sort(nums);
        return nums;
    }
}
```

```java
//双指针法 
class Solution {
    public int[] sortedSquares(int[] nums) {
        int left = 0;
        int right = nums.length - 1;
        //存储数组
        int[] result = new int[nums.length];
        int index = result.length - 1;
        while (left <= right) {
            //如果左边平方大于右边 就把左边平方数放到后边
            if (nums[left] * nums[left] > nums[right] * nums[right]) {
                result[index--] = nums[left] * nums[left];
                //左指针前进
                ++left;
            } else {
                //否则就把右边数放到后面
                result[index--] = nums[right] * nums[right];
                //右指针前进
                --right;
            }
            //整个数组是从大到小从后往前形成的
        }
        return result;
    }
}
```

## 5、旋转数组

```te

给定一个数组，将数组中的元素向右移动 k 个位置，其中 k 是非负数。
//t首尾相接 旋转k格

进阶：
尽可能想出更多的解决方案，至少有三种不同的方法可以解决这个问题。
你可以使用空间复杂度为 O(1) 的 原地 算法解决这个问题吗？

示例1：
输入: nums = [1,2,3,4,5,6,7], k = 3
输出: [5,6,7,1,2,3,4]
解释:
向右旋转 1 步: [7,1,2,3,4,5,6]
向右旋转 2 步: [6,7,1,2,3,4,5]
向右旋转 3 步: [5,6,7,1,2,3,4]
```

```java
/*
1、首先对整个数组实行翻转，这样子原数组中需要翻转的子数组，就会跑到数组最前面。
2、这时候，从 kk 处分隔数组，左右两数组，各自进行翻转即可。
*/
class Solution {
    public void rotate(int[] nums, int k) {
        k %= nums.length;
        reverse(nums, 0, nums.length - 1);
        reverse(nums, 0, k - 1);
        reverse(nums, k, nums.length - 1);
    }
    public void reverse(int[] nums, int start, int end) {
        while (start < end) {
            int temp = nums[start];
            nums[start++] = nums[end];
            nums[end--] = temp;
        }
    }
}
```

## 6、移动0

```te
给定一个数组 nums，编写一个函数将所有 0 移动到数组的末尾，同时保持非零元素的相对顺序。

示例:

输入: [0,1,0,3,12]
输出: [1,3,12,0,0]
```

```java
class Solution {
    public void moveZeroes(int[] nums) {
        int j = 0;
        for(int i = 0; i < nums.length ; i++){
            //将非零元素左移
           if(nums[i]!= 0){
               nums[j++] = nums[i];
           } 
        }
        for(;j < nums.length ; j++ ){
            //将后面元素置为0
            nums[j] = 0;
        }
    }
}
```

```java
//快慢指针
class Solution {
    public void moveZeroes(int[] nums) {
        int n = nums.length, left = 0, right = 0;
        while (right < n) {
            if (nums[right] != 0) {
                swap(nums, left, right);
                left++;
            }
            right++;
        }
    }

    public void swap(int[] nums, int left, int right) {
        int temp = nums[left];
        nums[left] = nums[right];
        nums[right] = temp;
    }
}
```

## 7、两数之和

```te
给定一个已按照 升序排列  的整数数组 numbers ，请你从数组中找出两个数满足相加之和等于目标数 target 。

函数应该以长度为 2 的整数数组的形式返回这两个数的下标值。numbers 的下标 从 1 开始计数 ，所以答案数组应当满足 1 <= answer[0] < answer[1] <= numbers.length 。

你可以假设每个输入只对应唯一的答案，而且你不可以重复使用相同的元素。
```



```java
class Solution {
    public int[] twoSum(int[] numbers, int target) {
        int left = 0,right = numbers.length-1;
        while(left < right){
            //如果两数之和大于target 说明右边太大，右指针前进一格 
            //如果两数之和小于target 说明左边太小 左指针前进一g
             if(numbers[left] + numbers[right] == target){
                 //下标从 1 开始 所以加上1
                return new int[]{left+1,right+1};
            }else if(numbers[left] + numbers[right] > target){
                right--;
            }else{
                left++;
            }
        }
        return new int[0];
    }
}
```

## 8、反转字符串

```te
编写一个函数，其作用是将输入的字符串反转过来。输入字符串以字符数组 char[] 的形式给出。

不要给另外的数组分配额外的空间，你必须原地修改输入数组、使用 O(1) 的额外空间解决这一问题。

你可以假设数组中的所有字符都是 ASCII 码表中的可打印字符。

 

示例 1：

输入：["h","e","l","l","o"]
输出：["o","l","l","e","h"]
```

```java
//二分交换
class Solution {
    public void reverseString(char[] s) {
        //二分法
        int n = s.length-1;
        for(int i = 0; i < s.length/2; i++){
            char temp = s[i];
            s[i] = s[n];
            s[n] = temp;
            n--;
        }
    }
}
```

## 9、反转单词

```te
给定一个字符串，你需要反转字符串中每个单词的字符顺序，同时仍保留空格和单词的初始顺序。

示例：

输入："Let's take LeetCode contest"
输出："s'teL ekat edoCteeL tsetnoc"

```

```java
//双指针局部反转
class Solution {
    public String reverseWords(String s) {
        char[] arr = s.toCharArray();
        int n = arr.length;
        int left = 0,right = 0;
        while(right <= n) {
            if(right == n ||arr[right] == ' ') {
                reverseString(arr, left, right -1);
                left = right + 1;
            }
            right++;     
        }
        return new String(arr);
    }
    public void reverseString(char[] s,int left, int right) {
        while(left < right) {
            s[left] ^= s[right];
            s[right] ^= s[left];
            s[left] ^= s[right];
            left++;
            right--;
        }
    }
}
```

## 10、链表的中间节点

```te
给定一个头结点为 head 的非空单链表，返回链表的中间结点。

如果有两个中间结点，则返回第二个中间结点。

示例 1：

输入：[1,2,3,4,5]
输出：此列表中的结点 3 (序列化形式：[3,4,5])
返回的结点值为 3 。 (测评系统对该结点序列化表述是 [3,4,5])。
注意，我们返回了一个 ListNode 类型的对象 ans，这样：
ans.val = 3, ans.next.val = 4, ans.next.next.val = 5, 以及 ans.next.next.next = NULL.
```

```java
//思路：快指针q每次走2步，慢指针p每次走1步，当q走到末尾时p正好走到中间
 /**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { 
 	   this.val = val; 
 	   this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
       ListNode p = head,q = head;
       while(q!=null && q.next != null){
           q = q.next.next;
           p = p.next;
       }
        return p;
    }
}
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode middleNode(ListNode head) {
      ListNode node = head;
      int count = 0;
      //建立存储表
      ArrayList<ListNode> list = new ArrayList<>();
      //遍历链表获取长度，将值放入存储表中，返回中间位置值
      while(node != null){
          list.add(node);
          count++;
          node = node.next;
      }
      return list.get(count/2);
    }
}
```

## 11、删除链表中倒数第n个元素

```tex
给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。

进阶：你能尝试使用一趟扫描实现吗？


示例 1：


输入：head = [1,2,3,4,5], n = 2
输出：[1,2,3,5]
```

```java
//计算长度我们首先从头节点开始对链表进行一次遍历，得到链表的长度 L。随后我们再从头节点开始对链表进行一次遍历，当遍历到第 L-n+1 个节点时，它就是我们需要删除的节点。
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        int length = getLength(head);
        ListNode cur = dummy;
        for (int i = 1; i < length - n + 1; ++i) {
            cur = cur.next;
        }
        cur.next = cur.next.next;
        ListNode ans = dummy.next;
        return ans;
    }

    public int getLength(ListNode head) {
        int length = 0;
        while (head != null) {
            ++length;
            head = head.next;
        }
        return length;
    }
}
```

```java
//双指针
class Solution {
    public ListNode removeNthFromEnd(ListNode head, int n) {
        ListNode dummy = new ListNode(0, head);
        ListNode first = head;
        ListNode second = dummy;
        for (int i = 0; i < n; ++i) {
            first = first.next;
        }
        while (first != null) {
            first = first.next;
            second = second.next;
        }
        second.next = second.next.next;
        ListNode ans = dummy.next;
        return ans;
    }
}
```

## 12、无重复字符的最长子串

```te
给定一个字符串 s ，请你找出其中不含有重复字符的 最长子串 的长度。

 

示例 1:

输入: s = "abcabcbb"
输出: 3 
解释: 因为无重复字符的最长子串是 "abc"，所以其长度为 3。
```

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        int n = s.length(), ans = 0;
        //建立哈希表存储字符
        Map<Character, Integer> map = new HashMap<>();
        for (int end = 0, start = 0; end < n; end++) {
            //返回索引处的char值
            char alpha = s.charAt(end);
            if (map.containsKey(alpha)) {
                //更新start位置
                start = Math.max(map.get(alpha), start);
            }
            ans = Math.max(ans, end - start + 1);
            map.put(s.charAt(end), end + 1);
        }
        return ans;
    }
}
```

```java
class Solution {
    public int lengthOfLongestSubstring(String s) {
        //记录字符上一次出现的位置
        int []last = new int[128];
        for(int i = 0;i < 128 ; i++){
            last[i] = -1;
        }
        int n = s.length();

        int res = 0;
        int start = 0;//窗口开始位置
        for(int i = 0; i < n; i++){
            int index = s.charAt(i);
            start = Math.max(start,last[index]+1);
            res = Math.max(res,i-start+1);
            last[index] = i;
        }
        return res;
    }
}
```

## 13、字符串的排列

```te
给你两个字符串 s1 和 s2 ，写一个函数来判断 s2 是否包含 s1 的排列。
换句话说，s1 的排列之一是 s2 的 子串 。 

示例 1：

输入：s1 = "ab" s2 = "eidbaooo"
输出：true
解释：s2 包含 s1 的排列之一 ("ba")
```

```te
滑动窗口 + 字典
分析一： 题目要求 s1 的排列之一是 s2 的一个子串。而子串必须是连续的，所以要求的 s2 子串的长度跟 s1 长度必须相等。

分析二： 那么我们有必要把 s1 的每个排列都求出来吗？当然不用。如果字符串 a 是 b 的一个排列，那么当且仅当它们两者中的每个字符的个数都必须完全相等。
```

```java
class Solution {
    public boolean checkInclusion(String s1, String s2) {
        int n = s1.length(), m = s2.length();
        if (n > m) {
            return false;
        }
        int[] cnt1 = new int[26];
        int[] cnt2 = new int[26];
        for (int i = 0; i < n; ++i) {
            ++cnt1[s1.charAt(i) - 'a'];
            ++cnt2[s2.charAt(i) - 'a'];
        }
        if (Arrays.equals(cnt1, cnt2)) {
            return true;
        }
        for (int i = n; i < m; ++i) {
            ++cnt2[s2.charAt(i) - 'a'];
            --cnt2[s2.charAt(i - n) - 'a'];
            if (Arrays.equals(cnt1, cnt2)) {
                return true;
            }
        }
        return false;
    }
}
```

## 14、图像渲染

```tex
有一幅以二维整数数组表示的图画，每一个整数表示该图画的像素值大小，数值在 0 到 65535 之间。

给你一个坐标 (sr, sc) 表示图像渲染开始的像素值（行 ，列）和一个新的颜色值 newColor，让你重新上色这幅图像。

为了完成上色工作，从初始坐标开始，记录初始坐标的上下左右四个方向上像素值与初始坐标相同的相连像素点，接着再记录这四个方向上符合条件的像素点与他们对应四个方向上像素值与初始坐标相同的相连像素点，……，重复该过程。将所有有记录的像素点的颜色值改为新的颜色值。

最后返回经过上色渲染后的图像。

示例 1:

输入: 
image = [[1,1,1],[1,1,0],[1,0,1]]
sr = 1, sc = 1, newColor = 2
输出: [[2,2,2],[2,2,0],[2,0,1]]
解析: 
在图像的正中间，(坐标(sr,sc)=(1,1)),
在路径上所有符合条件的像素点的颜色都被更改成2。
注意，右下角的像素没有更改为2，
因为它不是在上下左右四个方向上与初始点相连的像素点。
```

```java
/*思路及算法

我们从给定的起点开始，进行深度优先搜索。每次搜索到一个方格时，如果其与初始位置的方格颜色相同，就将该方格的颜色更新，以防止重复搜索；如果不相同，则进行回溯。

注意：因为初始位置的颜色会被修改，所以我们需要保存初始位置的颜色，以便于之后的更新操作。*/
class Solution {
     public int[][] floodFill(int[][] image, int sr, int sc, int newColor) {
        return dfs(image, sr, sc, newColor, image[sr][sc]);
    }

    public int[][] dfs(int[][] image, int i, int j, int newColor, int num){
        if(i<0 || i>=image.length || j<0 || j>=image[0].length || image[i][j]==newColor || image[i][j]!=num){

        }else{
            int temp=image[i][j];
            image[i][j]=newColor;
            dfs(image, i+1, j, newColor, temp);
            dfs(image, i-1, j, newColor, temp);
            dfs(image, i, j+1, newColor, temp);
            dfs(image, i, j-1, newColor, temp);
           
        }
        return image;
    }
}
```

## 15、岛屿的最大面积

```te
给定一个包含了一些 0 和 1 的非空二维数组 grid 。

一个 岛屿 是由一些相邻的 1 (代表土地) 构成的组合，这里的「相邻」要求两个 1 必须在水平或者竖直方向上相邻。你可以假设 grid 的四个边缘都被 0（代表水）包围着。

找到给定的二维数组中最大的岛屿面积。(如果没有岛屿，则返回面积为 0 。) 

示例 1:

[[0,0,1,0,0,0,0,1,0,0,0,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,1,1,0,1,0,0,0,0,0,0,0,0],
 [0,1,0,0,1,1,0,0,1,0,1,0,0],
 [0,1,0,0,1,1,0,0,1,1,1,0,0],
 [0,0,0,0,0,0,0,0,0,0,1,0,0],
 [0,0,0,0,0,0,0,1,1,1,0,0,0],
 [0,0,0,0,0,0,0,1,1,0,0,0,0]]
对于上面这个给定矩阵应返回 6。注意答案不应该是 11 ，因为岛屿只能包含水平或垂直的四个方向的 1 。

示例 2:

[[0,0,0,0,0,0,0,0]]
对于上面这个给定的矩阵, 返回 0。
```

```java
/*
我们想知道网格中每个连通形状的面积，然后取最大值。

如果我们在一个土地上，以 4 个方向探索与之相连的每一个土地（以及与这些土地相连的土地），那么探索过的土地总数将是该连通形状的面积。

为了确保每个土地访问不超过一次，我们每次经过一块土地时，将这块土地的值置为 0。这样我们就不会多次访问同一土地。*/
class Solution {
    public int maxAreaOfIsland(int[][] grid) {
        int ans = 0;
        for (int i = 0; i != grid.length; ++i) {
            for (int j = 0; j != grid[0].length; ++j) {
                ans = Math.max(ans, dfs(grid, i, j));
            }
        }
        return ans;
    }

    public int dfs(int[][] grid, int cur_i, int cur_j) {
        if (cur_i < 0 || cur_j < 0 || cur_i == grid.length || cur_j == grid[0].length || grid[cur_i][cur_j] != 1) {
            return 0;
        }
        grid[cur_i][cur_j] = 0;
        int[] di = {0, 0, 1, -1};
        int[] dj = {1, -1, 0, 0};
        int ans = 1;
        for (int index = 0; index != 4; ++index) {
            int next_i = cur_i + di[index], next_j = cur_j + dj[index];
            ans += dfs(grid, next_i, next_j);
        }
        return ans;
    }
}
```

## 16、合并二叉树

```tex
给定两个二叉树，想象当你将它们中的一个覆盖到另一个上时，两个二叉树的一些节点便会重叠。

你需要将他们合并为一个新的二叉树。合并的规则是如果两个节点重叠，那么将他们的值相加作为节点合并后的新值，否则不为 NULL 的节点将直接作为新二叉树的节点。

示例 1:

输入: 
	Tree 1                     Tree 2                  
          1                         2                             
         / \                       / \                            
        3   2                     1   3                        
       /                           \   \                      
      5                             4   7                  
输出: 
合并后的树:
	     3
	    / \
	   4   5
	  / \   \ 
	 5   4   7
```

```java
/*可以使用深度优先搜索合并两个二叉树。从根节点开始同时遍历两个二叉树，并将对应的节点进行合并。
两个二叉树的对应节点可能存在以下三种情况，对于每种情况使用不同的合并方式。

1、如果两个二叉树的对应节点都为空，则合并后的二叉树的对应节点也为空；

2、如果两个二叉树的对应节点只有一个为空，则合并后的二叉树的对应节点为其中的非空节点；

3、如果两个二叉树的对应节点都不为空，则合并后的二叉树的对应节点的值为两个二叉树的对应节点的值之和，此时需要显性合并两个节点。

对一个节点进行合并之后，还要对该节点的左右子树分别进行合并。这是一个递归的过程。
*/
class Solution {
    public TreeNode mergeTrees(TreeNode t1, TreeNode t2) {
        if (t1 == null) {
            return t2;
        }
        if (t2 == null) {
            return t1;
        }
        TreeNode merged = new TreeNode(t1.val + t2.val);
        merged.left = mergeTrees(t1.left, t2.left);
        merged.right = mergeTrees(t1.right, t2.right);
        return merged;
    }
}
```

## 17、填充每个节点的下一个右侧节点指针

```te
给定一个 完美二叉树 ，其所有叶子节点都在同一层，每个父节点都有两个子节点。二叉树定义如下：

struct Node {
  int val;
  Node *left;
  Node *right;
  Node *next;
}
填充它的每个 next 指针，让这个指针指向其下一个右侧节点。如果找不到下一个右侧节点，则将 next 指针设置为 NULL。
```

![img](https://assets.leetcode.com/uploads/2019/02/14/116_sample.png)

```te
初始状态下，所有 next 指针都被设置为 NULL。
输入：root = [1,2,3,4,5,6,7]
输出：[1,#,2,3,#,4,5,6,7,#]
解释：给定二叉树如图 A 所示，你的函数应该填充它的每个 next 指针，以指向其下一个右侧节点，如图 B 所示。序列化的输出按层序遍历排列，同一层节点由 next 指针连接，'#' 标志着每一层的结束。
```

```java
/
class Solution {
    public Node connect(Node root) {
        if (root == null) {
            return null;
        }
        if (root.left != null) {
            root.left.next = root.right;
            if (root.next != null) {
                root.right.next = root.next.left;
            }
        }
        connect(root.left);
        connect(root.right);
        return root;
    }
}
```

## 18、矩阵

```te
给定一个由 0 和 1 组成的矩阵 mat ，请输出一个大小相同的矩阵，其中每一个格子是 mat 中对应位置元素到最近的 0 的距离。

两个相邻元素间的距离为 1 。

 

示例 1：



输入：mat = [[0,0,0],[0,1,0],[0,0,0]]
输出：[[0,0,0],[0,1,0],[0,0,0]]

题目意思：给你个矩阵，里面的元素不是0就是1，把每个元素替换成走到最近的0的距离，如果是0，那距离0的最近距离是0，如果是1，那就找出距离这个1最近的0
```

```java
//BFS
/*
首先把每个源点 0 入队，然后从各个 0 同时开始一圈一圈的向 1 扩散（每个 1 都是被离它最近的 0 扩散到的 ），扩散的时候可以设置 int[][] dist 来记录距离（即扩散的层次）并同时标志是否访问过。对于本题是可以直接修改原数组 int[][] matrix 来记录距离和标志是否访问的，这里要注意先把 matrix 数组中 1 的位置设置成 -1 （设成Integer.MAX_VALUE啦，m * n啦，10000啦都行，只要是个无效的距离值来标志这个位置的 1 没有被访问过就行辣~）
*/
class Solution {
    public int[][] updateMatrix(int[][] matrix) {
        // 首先将所有的 0 都入队，并且将 1 的位置设置成 -1，表示该位置是 未被访问过的 1
        Queue<int[]> queue = new LinkedList<>();
        int m = matrix.length, n = matrix[0].length;
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                if (matrix[i][j] == 0) {
                    queue.offer(new int[] {i, j});
                } else {
                    matrix[i][j] = -1;
                } 
            }
        }
        
        int[] dx = new int[] {-1, 1, 0, 0};
        int[] dy = new int[] {0, 0, -1, 1};
        while (!queue.isEmpty()) {
            int[] point = queue.poll();
            int x = point[0], y = point[1];
            for (int i = 0; i < 4; i++) {
                int newX = x + dx[i];
                int newY = y + dy[i];
                // 如果四邻域的点是 -1，表示这个点是未被访问过的 1
                // 所以这个点到 0 的距离就可以更新成 matrix[x][y] + 1。
                if (newX >= 0 && newX < m && newY >= 0 && newY < n 
                        && matrix[newX][newY] == -1) {
                    matrix[newX][newY] = matrix[x][y] + 1;
                    queue.offer(new int[] {newX, newY});
                }
            }
        }

        return matrix;
    }
}
```

![image-20210903173924390](C:\Users\阿海\AppData\Roaming\Typora\typora-user-images\image-20210903173924390.png)

```java
//dp
class Solution {
  public int[][] updateMatrix(int[][] matrix) {
    int m = matrix.length, n = matrix[0].length;
    int[][] dp = new int[m][n];
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        dp[i][j] = matrix[i][j] == 0 ? 0 : 10000;
      }
    }

    // 从左上角开始
    for (int i = 0; i < m; i++) {
      for (int j = 0; j < n; j++) {
        if (i - 1 >= 0) {
          dp[i][j] = Math.min(dp[i][j], dp[i - 1][j] + 1);
        }
        if (j - 1 >= 0) {
          dp[i][j] = Math.min(dp[i][j], dp[i][j - 1] + 1);
        }
      }
    }
    // 从右下角开始
    for (int i = m - 1; i >= 0; i--) {
      for (int j = n - 1; j >= 0; j--) {
        if (i + 1 < m) {
          dp[i][j] = Math.min(dp[i][j], dp[i + 1][j] + 1);
        }
        if (j + 1 < n) {
          dp[i][j] = Math.min(dp[i][j], dp[i][j + 1] + 1);
        }
      }
    }
    return dp;
  }
}
```

## 19、腐烂的橘子

```tex
在给定的网格中，每个单元格可以有以下三个值之一：

值 0 代表空单元格；
值 1 代表新鲜橘子；
值 2 代表腐烂的橘子。
每分钟，任何与腐烂的橘子（在 4 个正方向上）相邻的新鲜橘子都会腐烂。

返回直到单元格中没有新鲜橘子为止所必须经过的最小分钟数。如果不可能，返回 -1。
```

![img](https://assets.leetcode-cn.com/aliyun-lc-upload/uploads/2019/02/16/oranges.png)

```tex
输入：[[2,1,1],[1,1,0],[0,1,1]]
输出：4
```

```java
//dfs
class Solution {
    int[][] grid;
    public int orangesRotting(int[][] grid) {
        if (grid == null || grid.length == 0) {
            return 0;
        }

        this.grid = grid;    
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 2) {
                    dfs(i, j, 2); // 开始传染
                } 
            }
        }

        // 经过dfs后，grid数组中记录了每个橘子被传染时的路径长度，找出最大的长度即为腐烂全部橘子所用的时间。
        int maxLevel = 0;
        for (int i = 0; i < grid.length; i++) {
            for (int j = 0; j < grid[0].length; j++) {
                if (grid[i][j] == 1) {
                    return -1; // 若有新鲜橘子未被传染到，直接返回-1
                } else {
                    maxLevel = Math.max(maxLevel, grid[i][j]);
                }
            }
        }

        return maxLevel == 0? 0: maxLevel - 2; 
    }

    private void dfs(int i, int j, int level) { // level用来记录传染路径的长度（当然最后要减2）
        if (i < 0 || i >= grid.length || j < 0 || j >= grid[0].length ) {
            return;
        }
        if (grid[i][j] != 1 && grid[i][j] < level) { // 只有新鲜橘子或者传播路径比当前路径长的橘子，才继续进行传播。
            return;
        } 
   
        grid[i][j] = level; // 将传染路径的长度存到grid[i][j]中
        level++;
        dfs(i - 1, j, level);
        dfs(i + 1, j, level);
        dfs(i, j - 1, level);
        dfs(i, j + 1, level);
    }
}
```

## 20、合并两个有序链表

```tex
将两个升序链表合并为一个新的 升序 链表并返回。新链表是通过拼接给定的两个链表的所有节点组成的。 

示例 1：

输入：l1 = [1,2,4], l2 = [1,3,4]
输出：[1,1,2,3,4,4]

```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode mergeTwoLists(ListNode l1, ListNode l2) {
        //归并排序用链表实现罢了
        ListNode newHead = new ListNode(0);
        ListNode cur = newHead;
        while (l1 != null&&l2 != null){
            if (l1.val < l2.val) {
                cur.next = l1;
                cur = cur.next;
                l1 = l1.next;
            } else {
                cur.next = l2;
                cur = cur.next;
                l2 = l2.next;
            }
        }
        //剩余部分直接连接上
         if (l1 == null) {
            cur.next = l2;
        } else {
            cur.next = l1;
        }
        return newHead.next;
    }
}
```

## 21、反转链表

```tex
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
 
示例 1：

输入：head = [1,2,3,4,5]
输出：[5,4,3,2,1]
```

```java
/**
 * Definition for singly-linked list.
 * public class ListNode {
 *     int val;
 *     ListNode next;
 *     ListNode() {}
 *     ListNode(int val) { this.val = val; }
 *     ListNode(int val, ListNode next) { this.val = val; this.next = next; }
 * }
 */
class Solution {
    public ListNode reverseList(ListNode head) {
        ListNode pre = null; //前指针节点
        ListNode cur = head;
        while(cur != null){
            ListNode nextTemp = cur.next;
            cur.next = pre;
            pre = cur;
            cur = nextTemp;
        }
        return pre;
    }
}
```

## 22、组合

```tex
给定两个整数 n 和 k，返回范围 [1, n] 中所有可能的 k 个数的组合。

你可以按 任何顺序 返回答案。

 

示例 1：

输入：n = 4, k = 2
输出：
[
  [2,4],
  [3,4],
  [2,3],
  [1,2],
  [1,3],
  [1,4],
]
```

```java
class Solution {
    
    List<List<Integer>> res = new ArrayList<>();  // 存储结果 

    public List<List<Integer>> combine(int n, int k) {
        LinkedList<Integer> track = new LinkedList<>();  // 一条遍历路径上的结果
        backtrack(n, k, 1, track);
        return res;
    }

    // 递归函数
    public void backtrack(int n, int k, int start, LinkedList<Integer> track) {
        // 到达树的底部
        if (track.size() == k) {
            res.add(new ArrayList<>(track));
            return;
        }
        // i 从 start 开始递增
        for (int i = start; i <= n - (k - track.size()) + 1; i++) {
            // 做选择，向 track 中添加元素
            track.add(i);
            // 递归
            backtrack(n, k, i + 1, track);
            // 回溯，撤销选择
            track.removeLast();
        }
    }
}
```

## 23、全排列

```tex
给定一个不含重复数字的数组 nums ，返回其 所有可能的全排列 。你可以 按任意顺序 返回答案。

 

示例 1：

输入：nums = [1,2,3]
输出：[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]
```

```java
class Solution {

    List<List<Integer>> result = new ArrayList<>();// 存放符合条件结果的集合
    LinkedList<Integer> path = new LinkedList<>();// 用来存放符合条件结果
    boolean[] used;
    public List<List<Integer>> permute(int[] nums) {
        if (nums.length == 0){
            return result;
        }
        used = new boolean[nums.length];
        permuteHelper(nums);
        return result;
    }

    private void permuteHelper(int[] nums){
        if (path.size() == nums.length){
            result.add(new ArrayList<>(path));
            return;
        }
        for (int i = 0; i < nums.length; i++){
            if (used[i]){
                continue;
            }
            used[i] = true;
            path.add(nums[i]);
            permuteHelper(nums);
            path.removeLast();
            used[i] = false;
        }
    }
}
```

## 24、大小写字母全排列

```tex
```

```java
/*思路

从左往右依次遍历字符，过程中保持 ans 为已遍历过字符的字母大小全排列。

例如，当 S = "abc" 时，考虑字母 "a", "b", "c"，初始令 ans = [""]，依次更新 ans = ["a", "A"]， ans = ["ab", "Ab", "aB", "AB"]， ans = ["abc", "Abc", "aBc", "ABc", "abC", "AbC", "aBC", "ABC"]。

算法

如果下一个字符 c 是字母，将当前已遍历过的字符串全排列复制两份，在第一份的每个字符串末尾添加 lowercase(c)，在第二份的每个字符串末尾添加 uppercase(c)。

如果下一个字符 c 是数字，将 c 直接添加到每个字符串的末尾。*/
class Solution {
    public List<String> letterCasePermutation(String S) {
        List<StringBuilder> ans = new ArrayList();
        ans.add(new StringBuilder());

        for (char c: S.toCharArray()) {
            int n = ans.size();
            if (Character.isLetter(c)) {
                for (int i = 0; i < n; ++i) {
                    ans.add(new StringBuilder(ans.get(i)));
                    ans.get(i).append(Character.toLowerCase(c));
                    ans.get(n+i).append(Character.toUpperCase(c));
                }
            } else {
                for (int i = 0; i < n; ++i)
                    ans.get(i).append(c);
            }
        }

        List<String> finalans = new ArrayList();
        for (StringBuilder sb: ans)
            finalans.add(sb.toString());
        return finalans;
    }
}
```

## 25、走楼梯

```tex
假设你正在爬楼梯。需要 n 阶你才能到达楼顶。

每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？

注意：给定 n 是一个正整数。

示例 1：

输入： 2
输出： 2
解释： 有两种方法可以爬到楼顶。
1.  1 阶 + 1 阶
2.  2 阶
示例 2：

输入： 3
输出： 3
解释： 有三种方法可以爬到楼顶。
1.  1 阶 + 1 阶 + 1 阶
2.  1 阶 + 2 阶
3.  2 阶 + 1 阶
```

```java
//累加
class Solution {
    public int climbStairs(int n) {
        if(n == 1) {
            return 1;
        }
        if(n == 2){
            return 2;
        }
        int a = 1,b = 2,temp;
        for(int i = 3; i <= n;i++){
            temp = a;
            //n-1
            a = b;
            //n-2
            b = temp + b;
        }
        return b ;
    }
}
```

## 26、打家劫舍

```tex
你是一个专业的小偷，计划偷窃沿街的房屋。每间房内都藏有一定的现金，影响你偷窃的唯一制约因素就是相邻的房屋装有相互连通的防盗系统，如果两间相邻的房屋在同一晚上被小偷闯入，系统会自动报警。

给定一个代表每个房屋存放金额的非负整数数组，计算你 不触动警报装置的情况下 ，一夜之内能够偷窃到的最高金额。

 

示例 1：

输入：[1,2,3,1]
输出：4
解释：偷窃 1 号房屋 (金额 = 1) ，然后偷窃 3 号房屋 (金额 = 3)。
     偷窃到的最高金额 = 1 + 3 = 4 。
```

![image-20210906201403016](C:\Users\阿海\AppData\Roaming\Typora\typora-user-images\image-20210906201403016.png)

```java
class Solution {
    public int rob(int[] nums) {
        if (nums == null || nums.length == 0) return 0;
		if (nums.length == 1) return nums[0];
        
        int []dp = new int [nums.length]; 
        dp[0] = nums[0];
		dp[1] = Math.max(dp[0], nums[1]);
        for(int i = 2; i < nums.length;i++){
            dp[i] = Math.max(dp[i-2]+nums[i],dp[i-1]);
        }
        return dp[nums.length-1];
    }
}
```

## 27、三角形最小路径和

```tex
给定一个三角形 triangle ，找出自顶向下的最小路径和。

每一步只能移动到下一行中相邻的结点上。相邻的结点 在这里指的是 下标 与 上一层结点下标 相同或者等于 上一层结点下标 + 1 的两个结点。也就是说，如果正位于当前行的下标 i ，那么下一步可以移动到下一行的下标 i 或 i + 1 。

 

示例 1：

输入：triangle = [[2],[3,4],[6,5,7],[4,1,8,3]]
输出：11
解释：如下面简图所示：
   2
  3 4
 6 5 7
4 1 8 3
自顶向下的最小路径和为 11（即，2 + 3 + 5 + 1 = 11）。
```

```java
//自底向上
class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        int[] dp = new int[triangle.get(n-1).size()+1];
        for(int i = n-1 ; i>=0 ; i--){
            for(int j = 0 ; j < triangle.get(i).size() ; j++){
                dp[j] = triangle.get(i).get(j)+Math.min(dp[j],dp[j+1]);
            }
        }
        return dp[0];
    }
}
```

## 28、2的幂

```tex
给你一个整数 n，请你判断该整数是否是 2 的幂次方。如果是，返回 true ；否则，返回 false 。
如果存在一个整数 x 使得 n == 2x ，则认为 n 是 2 的幂次方。

示例 1：

输入：n = 1
输出：true
解释：20 = 1
示例 2：

输入：n = 16
输出：true
解释：24 = 16
```

```java
//递归
class Solution {
    public boolean isPowerOfTwo(int n) {
        if(n == 1) return true;
        if(n == 0) return false;
        if(n % 2 != 0) return false;
            return isPowerOfTwo(n/2); 
    }
}
```

```java
//位运算
class Solution {
    public boolean isPowerOfTwo(int n) {
        return n > 0 && (n & (n - 1)) == 0;
    }
}
```

![image-20210907102118226](C:\Users\阿海\AppData\Roaming\Typora\typora-user-images\image-20210907102118226.png)

## 29、位1的个数

```tex
编写一个函数，输入是一个无符号整数（以二进制串的形式），返回其二进制表达式中数字位数为 '1' 的个数（也被称为汉明重量）。

提示：
请注意，在某些语言（如 Java）中，没有无符号整数类型。在这种情况下，输入和输出都将被指定为有符号整数类型，并且不应影响您的实现，因为无论整数是有符号的还是无符号的，其内部的二进制表示形式都是相同的。
在 Java 中，编译器使用二进制补码记法来表示有符号整数。因此，在上面的 示例 3 中，输入表示有符号整数 -3。

示例 1：

输入：00000000000000000000000000001011
输出：3
解释：输入的二进制串 00000000000000000000000000001011 中，共有三位为 '1'。
```

```java
//位数检查：遍历每一位统计 1 的个数 n >> i 左移i位
public class Solution {
    public int hammingWeight(int n) {
        int ans = 0;
        for (int i = 0; i < 32; i++) {
            ans += ((n >> i) & 1);
        }
        return ans;
    }
}
```

