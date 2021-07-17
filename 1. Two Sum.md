# 1. Two Sum
## Description
給定一整數list以及整數目標，找出<font color="#f00">list內兩數值相加剛好等於目標並回傳indices</font>
## Solutions
### 1. 
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