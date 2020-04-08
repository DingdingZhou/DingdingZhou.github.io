---
layout: post
title: "Leetcode 30 -- Day 7"
subtitle: "Counting Elements"
date: "2020-04-06 12:00:00"
author: "Dingding"
header-img: "img/post/thrift-header.jpg"
header-mask: 0.3
catalog: true
tags:
    - leetcode
    - 30days
---

## Question

```
 Given an integer array arr, count element x such that x + 1 is also in arr.
 If there're duplicates in arr, count them seperately.
```

## Example

Example 1:

-   Input: arr = [1,2,3]
-   Output: 2
-   Explanation: 1 and 2 are counted cause 2 and 3 are in arr.

Example 2:

-   Input: arr = [1,1,3,3,5,5,7,7]
-   Output: 0
-   Explanation: No numbers are counted, cause there's no 2, 4, 6, or 8 in arr.

Example 3:

-   Input: arr = [1,3,2,3,5,0]
-   Output: 3
-   Explanation: 0, 1 and 2 are counted cause 1, 2 and 3 are in arr.

Example 4:

-   Input: arr = [1,1,2,2]
-   Output: 2
-   Explanation: Two 1s are counted cause 2 is in arr.

Constraints:

-   1 <= arr.length <= 1000
-   0 <= arr[i] <= 1000

## Code C

```c
int countElements(int *arr, int arrSize)
{
    int data[1002] = {0};
    for (int i = 0; i < arrSize; ++i)
    {
        data[arr[i]] = 1;
    }
    int sum = 0;
    for (int i = 0; i < arrSize; ++i)
    {
        if (data[arr[i] + 1] == 1)
        {
            ++sum;
        }
    }
    return sum;
}
```

## Code Golang

```golang
func countElements(arr []int) int {
	data := make([]bool, 1002)
	for _, v := range arr {
		data[v] = true
	}
	sum := 0
	for _, v := range arr {
		if data[v+1] {
			sum = sum + 1
		}
	}
	return sum
}
```

## Code Python

@Ricky

```python
class Solution:
    def countElements(self, arr: List[int]) -> int:
        count=0
        for x in arr:
            if x+1 in arr:
                count += 1
        return count
```
