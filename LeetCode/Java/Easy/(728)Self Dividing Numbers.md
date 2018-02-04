
31 / 31 test cases passed.
Status: Accepted
Runtime: 6 ms

Your runtime beats 64.44 % of java submissions.

A self-dividing number is a number that is divisible by every digit it contains.

For example, 128 is a self-dividing number because 128 % 1 == 0, 128 % 2 == 0, and 128 % 8 == 0.

Also, a self-dividing number is not allowed to contain the digit zero.

Given a lower and upper number bound, output a list of every possible self dividing number, including the bounds if possible.

Example 1:
Input: 
left = 1, right = 22
Output: [1, 2, 3, 4, 5, 6, 7, 8, 9, 11, 12, 15, 22]
Note:

The boundaries of each input argument are 1 <= left <= right <= 10000.


1. MySolution:

```java
class Solution {
    public List<Integer> selfDividingNumbers(int left, int right) {
        if(left > right)
        {
            return null;
        }
        List<Integer> result = new ArrayList(right - left + 1);
        for(int i = left; i <= right; i++)
        {
            if(isDividing(i))
            {
                result.add(i);
            }
        }
        return result;
    }
    
    private boolean isDividing(int num)
    {
        for(int temp = num;temp > 0;temp /=10)
        {
            int mod = temp % 10;
            if(mod == 0 || (num % mod != 0))
            {
                return false;
            }
        }
        return true;        
    }
}
```
