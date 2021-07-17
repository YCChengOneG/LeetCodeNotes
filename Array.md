# Array 題型
## 1. Two Sum
### Description
給定一整數list以及整數目標，找出<font color="#f00">list內兩數值相加剛好等於目標並回傳indices</font>
### Examples
Input: nums = [2,7,11,15], target = 9

Output: [0,1]

Output: Because nums[0] + nums[1] == 9, we return [0, 1].


### Solutions
#### 1. 
用一空的dict紀錄target與list內元素相減的值，若往後查找list內的值發現有對應dict內，即可傳回index

```python
class Solution:
    def twoSum(self, nums: List[int], target: int) -> List[int]:
        # 宣告一個空的dict用來記錄target與元素的差值
        d = {}
        
        # 基本先來個迴圈跑遍list內所有元素
        for i, num in enumerate(nums):
            # 先判定這個元素值是否為先前元素與target的差值，若否則在else處存至d
            if num in d:
                return i, d[num] #找到差值存在，回傳當下的idx以及dict內儲存的idx
                
            else:
                diff = target - num
                d[diff] = i # 以差值當key, list idx當value
```

## 167. Two Sum II - Input array is sorted
### Description
類似第一題，但輸入為遞增數列，每題僅有答案一組，不可重複使用元素
### Solutions
宣告兩指標，一為由小至大(左至右)，二為大至小(右至左)，判斷兩指標value相加是否為target，大於target則移動指標二(相對來說將兩值的和減少)，小於target則移動指標一(相對來說兩值的和增加)
#### 1.
```python=
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers)-1
        while left<right:
            currentsum = numbers[left]+numbers[right] #相加左右指標的value
            if currentsum == target:
                return [left+1,right+1] #回傳為1-based index
    
            elif currentsum>target: #大於則移動右指標 (將兩值之和減少)
                right -= 1
            elif currentsum<target: #小於則移動左指標 (將兩值之和增加)
                left += 1

```
#### 2.
用第一題概念
```python=
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:        
        dic = {}
        for index, value in enumerate(numbers):
            diff = target - value
            if diff in dic:
                return [dic[diff], index+1] #查找到的idx一定比當下idx小，所以這樣排列
            dic[value] = index + 1 #以當下值當key, index當value

```
## 15. 3Sum
### Description
給定一數列，回傳<font color="#f00">相加為0的三個元素，答案不只一組</font>，元素不可重複使用
### Solutions
#### 1.
利用167題的Solution 1的概念，先將輸入排序後，從最小元素開始跑迴圈(curr)，找到右側數列中兩數之和為(0 - curr)
```python=
class Solution(object):
    def threeSum(self, nums):
        """
        :type nums: List[int]
        :rtype: List[List[int]]
        """
        nums.sort() # O(nlogn) - timsort
        result = []
        #set的特性: 內部元素不重複
        #宣告set，用於檢查新元素是否已存在
        resultSet = set() #the result will take up O(n) space anyway, having a set to keep track of unique will -> O(2*n)S -> O(n)S
        for i in range(len(nums)-2): #跑迴圈遍歷元素至len-2 (最後兩個不用)
            left = i + 1 #如2sum，宣告左指標
            right = len(nums) - 1 #如2sum，宣告右指標
            current = nums[i] 
	#如2sum，開始跑左右指標
            while left < right: #Time complexity of O(n^2)T comes from the nested for-while loops
                sumToCheck = current + nums[left] + nums[right]
                if sumToCheck == 0 : #檢查三值相加是否為target
                    if self.getHashableKey([current,nums[left],nums[right]]) not in resultSet: #檢查是否已存在，若不存在則append
                        result.append([current,nums[left],nums[right]])
                        resultSet.add(self.getHashableKey([current,nums[left],nums[right]]))
                    left += 1
                    right -= 1
                elif sumToCheck > 0 :
                    right -= 1
                else:
                    left += 1
        return result
    def getHashableKey(self,key):
        return str(key[0]) + ":" + str(key[1]) + ":" + str(key[2])

```
## 16. 3Sum Closest
### Description
15的變化題，找到離target最近的三個元素總和並回傳，答案僅有一個
### Examples
Input: nums = [-1,2,1,-4], target = 1

Output: 2

Explanation: The sum that is closest to the target is 2. (-1 + 2 + 1 = 2).

### Solutions
#### 1.
利用15題的概念，從最小元素開始跑迴圈(curr)，找到右側數列中兩數之和最接近(target - curr)，跑完整個數列同時也更新出最小的總和值
```python=
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort() # O(nlogn) - timsort
        compare = float('inf')
        result = 0
        for i in range(len(nums)-2): #跑迴圈遍歷元素至len-2 (最後兩個不用)
            left = i + 1 #如2sum，宣告左指標
            right = len(nums) - 1 #如2sum，宣告右指標
            current = nums[i] 
	#如2sum，開始跑左右指標
            while left < right: #Time complexity of O(n^2)T comes from the nested for-while loops
                sumToCheck = current + nums[left] + nums[right]
                diff = sumToCheck - target
                if diff == 0 : #檢查diff是否為target
                    return target
                elif diff > 0 :
                    right -= 1
                else:
                    left += 1
                diff_abs = abs(diff) 
                if diff_abs < compare:#更新最小的距離值以及總和
                    compare = diff_abs
                    result = sumToCheck
        return result

```
#### 2.
Algo:

1.sort the array(using built-in method or use quick sort)

2.use two pointers, start = 0 and end = a.length - 1

3.use binary search to find the exact number or a index closer to that number

4.if binary search give start or end position itself, then check left and right index, whichever has min difference take that

5.if the sum of all 3 < target do start++

6.else do end--;

此解法可理解為，假定起點及終點為答案，找尋數列中間的值使三值相加可以最接近target，判斷相加大於/小於target決定移動起點/終點，同時更新最小的距離值以及總和
```python=
class Solution:
    def threeSumClosest(self, nums: List[int], target: int) -> int:
        nums.sort() # O(nlogn) - timsort
        min_diff = float('inf')
        result = 0

        start = 0 
        end = len(nums) - 1;
        while(start < end):
	#先記錄頭尾的和
            _sum = nums[start] + nums[end]
	#利用二分搜尋法找到離 (target - _sum)最近的index
            index = self.binarysearch(nums, 0, len(nums) - 1, target - _sum);
            #若二分法找不到最近的index，則找index左右方向最接近index的值
            index = self.findItem(nums, start, end, index);
            #將找到的index加上去
            _sum = _sum + nums[index];

            if(_sum == target):
                return target
            if(min_diff > abs(target - _sum)):#更新最小的距離值以及總和
                min_diff = abs(target - _sum)
                result = _sum
            #針對_sum值大於/小於target，移動start/end指標
            if(target > _sum):
                start = start + 1
            else:
                end = end - 1
        
        return result
    
    
    def findItem(self, a: List[int], start: int, end: int, index: int):
        #若index不等於start或end，則直接回傳 (代表在二元搜索時有找到最近值)
        #反之若index為start或end，則代表二元搜索沒找到最近值
        if(index != start and index != end):
            return index
       
        _min = float('inf')
        #初始化兩個指標
        pre = index
        _next = index
        #這邊用while是防止出現 pre-1之後等於start
        while(pre == start or pre == end):
            pre = pre - 1
        #若pre存在，則更新_min
        if(pre >= 0 and _min > abs(a[index] - a[pre])):
            _min = abs(a[index] - a[pre])
        #這邊用while是防止出現 _next+1之後等於end
        while(_next == start or _next == end):
            _next = _next + 1;
        #若_next存在且value值與index差值較pre近，則回傳，反之則回傳pre
        return _next if (_next < len(a) and _min >= abs(a[index] - a[_next])) else pre
    
    def binarysearch(self, a: List[int], start: int, end: int, key: int)  -> int:
        #找到距離與key最近的index值
        s = start
        e = end
        while(start <= end):
            mid = start + ((end - start) >> 1)
            if(key == a[mid]):
                return mid
            if(key < a[mid]):
                end = mid - 1
            else:
                start = mid + 1

        #若end小於s，代表key小於這段數列
        if(end < s):
            return s
        #若start大於e，代表key大於這段數列
        if(start > e):
            return e
        #回傳較接近key的index
        return end if (abs(key - a[end]) <= abs(key - a[start])) else start

```
## 18. 4Sum
### Description
給定一數列及target，回傳數列內四個數字相加等於target的組合，元素不可重複使用
### Examples
Input: nums = [1,0,-1,0,-2,2], target = 0

Output: [[-2,-1,1,2],[-2,0,0,2],[-1,0,0,1]]
### Solutions
#### 1.
多一層迴圈的15題
```python=
class Solution:
    def fourSum(self, nums: List[int], target: int) -> List[List[int]]:
        nums.sort() # O(nlogn) - timsort
        result = []
        #set的特性: 內部元素不重複
        #宣告set，用於檢查新元素是否已存在
        resultSet = set() #the result will take up O(n) space anyway, having a set to keep track of unique will -> O(2*n)S -> O(n)S
        for i in range(len(nums)-3): #跑迴圈遍歷元素至len-3 (最後三個不用)
            for j in range(i+1, len(nums)-2): #跑迴圈遍歷元素至len-2 (最後兩個不用)
                left = j + 1 #如2sum，宣告左指標
                right = len(nums) - 1 #如2sum，宣告右指標
                current = nums[j] 
	            #如2sum，開始跑左右指標
                while left < right: #Time complexity of O(n^2)T comes from the nested for-while loops
                    sumToCheck = nums[i] + current + nums[left] + nums[right]
                    if sumToCheck == target : #檢查四值相加是否為target
                        if self.getHashableKey([nums[i], current,nums[left],nums[right]]) not in resultSet: #檢查是否已存在，若不存在則append
                            result.append([nums[i], current,nums[left],nums[right]])
                            resultSet.add(self.getHashableKey([nums[i], current,nums[left],nums[right]]))
                        left += 1
                        right -= 1
                    elif sumToCheck > target :
                        right -= 1
                    else:
                        left += 1
        return result
    def getHashableKey(self,key):
        return str(key[0]) + ":" + str(key[1]) + ":" + str(key[2])

```