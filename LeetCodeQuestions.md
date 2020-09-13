### [Find Median Sorted Arrays](https://leetcode.com/problems/median-of-two-sorted-arrays/)
>What is median? Dividing a set into two equal length subsets, such that one subset is always greater than the other. 

``` python
[ 1, 2, 3, 4]
[1, 2]
```

given two sets 
$$
x_1 x_2 | x_3 x_4 x_5 x_6 \\
y_1 y_2 y_3 y_4 y_5 | y_6 y_7 y_8 \\

x_2 \le y_6 \\
y_5 \le x_3 \\
avg(max(x_2, y_5), min(x_3, y_8))\\
O(log(min(x,y)))
$$

you want to make sure that the COMBINED length of the left hand side of set x and the left-hand side of set y equal the COMBINED length of the right-hand side of set x and the right-hand side of set y equal (as per the definition of median). Thus, note the pipes currently.

The partition of x = (start + end)/2
The partition of y = (len(x) + len(y) + 1)/2 - partition of x (NOTE: the +1 is to help offset better between odd length arrays and even length arrays)

ex:
``` python
x = [1,3,8,9,5]
y = [7,11,18,19,21,25] 
```
Step 1
- start = 0 
- end = 4
- partition_x = (4+0)/2 = 2
- partition_y = (5+6+1)/2 - 2 = 4

```
x = [1,3 | 8,9,15]
y = [7,11,18,19 | 21,25] 
3 < 21 (yep)
19 < 8 (nope) --> this indicates that we need to shift y index by 1 down and x up by 1 (partition_x + 1)
```

Step 2
- start = 3
- end = 4
- partition_x = (3+4)/2 = 3
- partition_y = (5+6+1)/2 - 3 = 3

```
x = [1,3,8 | 9,15]
y = [7,11,18 | 19,21,25] 
8 < 19 (yep) (if this is were not true, then partion_x - 1)
18 < 9 (nope, so partion_x + 1) 
```

Step 3
- start = 4
- end = 4
- partition_x = (4 + 4)/2 = 4
- partition_y = (5 + 5 + 1)/2 - 4 = 2

```
x = [1,3,8,9 | 15]
y = [7,11 | 18,19,21,25] 
9 < 18 (yep)
11 < 15 (yep) 
```
median (max(9,11), as the array is odd)

### [Linked-List Merge](https://leetcode.com/problems/merge-k-sorted-lists/)
``` java
public ListNode sortList(ListNode head) {
        
        if(head == null || (head != null && head.next == null)) return head;
        
        ListNode slow = head;
        ListNode fast = head;
        ListNode prev = null;
        
        while(fast != null && fast.next != null){
            prev = slow;
            slow = slow.next;
            fast = fast.next.next;
        }
        
        prev.next = null;
        
        ListNode left = sortList(head);
        ListNode right = sortList(slow);
        
        return merge(left, right);
    }

```

### [Dynamic Programming Max Profit Assignment](#)
``` java
class Solution {
    public int maxProfitAssignment(int[] difficulty, int[] profit, int[] worker) {
        
        if(worker == null || difficulty == null || profit == null || profit.length==0) {
            return 0;
        }
        int n = profit.length;
        int[] p = new int[(int)(1e5+1)];
        for(int i=0;i<n;i++){
            p[difficulty[i]] = p[difficulty[i]] > profit[i] ?  p[difficulty[i]] : profit[i];
            
        }
        for(int i=1;i<p.length;i++){
            p[i] = p[i] > p[i-1] ? p[i] : p[i-1];
            
        }
        int cnt = 0;
        for(int i=0;i<worker.length;i++){
            cnt+=p[worker[i]];
        }
        return cnt;
    }
}

```
### [Group Anagrams](https://leetcode.com/problems/group-anagrams/)
Protip: Using keys instead of values to store information 
Compressing array info via string or tuples

### [Longest Substring](https://leetcode.com/problems/longest-substring-without-repeating-characters/)
- While loops are op (more flexibility with indexes)
- Use "pointers"
- Identify repetitive or unnecessary checks/ find ways to rule them out

### [K-Sum question](https://leetcode.com/problems/4sum/)
- Recursive solution (use the two sum solution to get 3 sum, then use the 3 sum solution to get the overrall 4 sum solution)
``` python
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        def kSum(nums: List[int], target: int, k: int) -> List[List[int]]:
            res = []
            if len(nums) == 0 or nums[0] * k > target or target > nums[-1] * k:
                return res
            if k == 2:
                return twoSum(nums, target)
            for i in range(len(nums)):
                if i == 0 or nums[i - 1] != nums[i]:
                    for _, set in enumerate(kSum(nums[i + 1:], target - nums[i], k - 1)):
                        res.append([nums[i]] + set)
            return res

        def twoSum(nums: List[int], target: int) -> List[List[int]]:
            res = []
            lo, hi = 0, len(nums) - 1
            while (lo < hi):
                sum = nums[lo] + nums[hi]
                if sum < target or (lo > 0 and nums[lo] == nums[lo - 1]):
                    lo += 1
                elif sum > target or (hi < len(nums) - 1 and nums[hi] == nums[hi + 1]):
                    hi -= 1
                else:
                    res.append([nums[lo], nums[hi]])
                    lo += 1
                    hi -= 1
            return res

        nums.sort()
        return kSum(nums, target, 4)
```

### [Valid Parentheses](https://leetcode.com/problems/valid-parentheses/)
- Can utilize the Stack (basically a list where you always pop FILO order (first in, last out))
- If you have two matches next to each other in the stack (open and close parentheses), pop out the top two (which should be the match) --> allows the algorithm to be O(n) (unlike my solution)

### [Longest Valid Parentheses](https://leetcode.com/problems/longest-valid-parentheses/solution/)
``` python
        if(len(s) == 0):
            return 0
        result = 0
        stack = [-1]
        for i in range(len(s)):
            if s[i] == '(':
                stack.insert(0, i)
            if(len(stack) >= 1 and s[i] == ")"):
                temp = stack.pop(0)
                if(len(stack) == 0):
                    stack.insert(0,i)
                else:
                    result = max(result, i - stack[0])

```



### [Next-Permutations](https://leetcode.com/problems/next-permutation/solution/)
In order for a solution to be valid, all the digits in the sequence must be in descending order
1. Find the first decreasing element (starting from the righthand-side)
2. Find the last number that is larger than the first decreasing element starting from the the first decreasing element to the end of the list
3. Swap those two values
4. from the index of the first decreasing element `i`, reverse all numbers `i+1` onwards to `len(nums) - 1`

Solution: Single Pass Approach
``` java
public class Solution {
    public void nextPermutation(int[] nums) {
        int i = nums.length - 2;
        while (i >= 0 && nums[i + 1] <= nums[i]) {
            i--;
        }
        if (i >= 0) {
            int j = nums.length - 1;
            while (j >= 0 && nums[j] <= nums[i]) {
                j--;
            }
            swap(nums, i, j);
        }
        reverse(nums, i + 1);
    }

    private void reverse(int[] nums, int start) {
        int i = start, j = nums.length - 1;
        while (i < j) {
            swap(nums, i, j);
            i++;
            j--;
        }
    }

    private void swap(int[] nums, int i, int j) {
        int temp = nums[i];
        nums[i] = nums[j];
        nums[j] = temp;
    }
}
```