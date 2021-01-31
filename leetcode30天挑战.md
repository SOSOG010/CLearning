

# 1. Single Number

Given a non-empty array of integers, every element appears twice except for one. Find that single one.

Note:

Your algorithm should have a linear runtime complexity. Could you implement it without using extra memory?

Exmple 1:

```
Input: [2,2,1]
output: 1
```

Exmple 2:

```
Input: [4,1,2,1,2]
output: 4
```

```c
int singleNumber(int* nums, int numsSize){
	int n = nums[0];
	for(int i = 1; i < numsSize; i++){
		n ^= nums[i];
	}
	return n;
}
```

> 注：异或运算：A ^ A == 0 ; A ^ B == B ^ A ; A ^ 0 == A。其实，如果一个数列中有奇数个X，另外的都是偶数个，就可以用异或的方法找出X。



# 2. Happy Number(Cycle Detection)

Write an algorithm to determine if a number is "happy".

A happy number is a number defined by the following process: 

- Starting with any positive integer, replace the number by the number by the sum of the squares of its digits.
- repeat the process until the number equals 1(where it will stay), or it loops endlessly in a cycle which does not include 1. 
- Those numbers for which this process ends in 1 are happy numbers.

Return true if n is a happy number, and false if not.

<img src=".\images\happyNumber.jpg" style="zoom: 80%;" />

```c
//calculate the next n
int next_n(int n){
	int temp,square = 0;
	while(n){
		temp = n % 10;
		n /= 10;
		square += temp * temp;
	}
	return square;
}

//using two pointers "slow" and "fast" represent tortoise and hare respectively
bool isHappy(int n){
	int slow = n, fast = n;
	do{
		slow = next_n(slow):
		fast = next_n(next_n(fast));
	}while(slow != fast);
	return fast == 1;	// equals if(fast == 1) return true; else return false;
}
```



# 3. Maximum Subarray

Given an integer array nums, find the contiguous subarray (containing at least one number) which has the largest sum  and return its sum.

Example:

```
Input: [-2,1,-3,4,-1,2,1,-5,4],
Output: 6
Explanation: [4,-1,2,1] has the largest sum = 6.
```

Follow up:

if you have figured out the O(n) solution, try coding another solution using the divide and conquer approach, which is more subtle.

```
int maxSubArray(int* nums, int numsSize){
	
}
```

