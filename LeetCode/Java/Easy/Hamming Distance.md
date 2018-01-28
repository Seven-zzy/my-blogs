The Hamming distance between two integers is the number of positions at which the corresponding bits are different.

Given two integers x and y, calculate the Hamming distance.

Note:
0 ≤ x, y < 231.

Example:
```
Input: x = 1, y = 4

Output: 2

Explanation:
1   (0 0 0 1)
4   (0 1 0 0)
       ↑   ↑

The above arrows point to positions where the corresponding bits are different.
```

解法1：用Integer中的toBinaryString API转换二进制，比29%的小伙伴运行快。
```java
/**
* 注意边界值，Integer.MAX_VALUE=2^31-1，第一次提交写成了x >= Integer.MAX_VALUE
**/
class Solution {
    public int hammingDistance(int x, int y) {
        if(x < 0 || x > Integer.MAX_VALUE || y < 0 || y > Integer.MAX_VALUE)
        {
            return 0;
        }
        int count = 0;
        int result =  x ^ y;
        char[] binary = Integer.toBinaryString(result).toCharArray();
        for(int i = 0; i < binary.length; i++)
        {
            if(binary[i] == '1')
            {
                count ++;
            }
        }
        return count;
    }
 }
 ```
 
 解法2：自己写方法转换二进制，149个用例耗时11ms，比40.8%的小伙伴运行结果快。
 ```java
 class Solution {
    public int hammingDistance(int x, int y) {
        if(x < 0 || x > Integer.MAX_VALUE || y < 0 || y > Integer.MAX_VALUE)
        {
            return 0;
        }
        int count = 0;
        int result =  x ^ y;
        int[] binary = toBinarryArray(result);
        for(int i = 0; i < binary.length; i++)
        {
            if(binary[i] == 1)
            {
                count ++;
            }
        }
        return count;
    }

    /**
    * 将int的每一位与1的结果保存起来
    **/
    private int[] toBinarryArray(int x)
    {
        int[] binary = new int[32];
        int test = 1;
        for(int i = 0; i < 32; i++)
        {
            if((x & test) != 0)
            {
                binary[i] = 1;
            }
            else
            {
                binary[i] = 0;
            }
            test = test << 1;
        }
        return binary;
    }
}
```
