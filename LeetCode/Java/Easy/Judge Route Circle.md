Initially, there is a Robot at position (0, 0). Given a sequence of its moves, judge if this robot makes a circle, which means it moves back to the original place.

The move sequence is represented by a string. And each move is represent by a character. The valid robot moves are R (Right), L (Left), U (Up) and D (down). The output should be true or false representing whether the robot makes a circle.
```
Example 1:
Input: "UD"
Output: true
Example 2:
Input: "LL"
Output: false
```

***
讨论区有人说题目交代不清楚，出题意图应该是end as a circle，而不是出现过环。恩，仔细审题确实有歧义，不过我默认是按照end as a circle理解的==

1. My Solution:
判断Robot是否回到原点，即是判断moves中，R和L次数相等并且U和D次数相等。
运行62个用例耗时12ms，击败了94%的java提交者……

```java
class Solution {
    public boolean judgeCircle(String moves) {
        if(moves == null)
        {
            return false;
        }
        if(moves.length() == 0)
        {
            return true;
        }
        
        int rCount = 0, lCount = 0, uCount = 0, dCount = 0;
        for(char move: moves.toCharArray())
        {
            switch(move)
            {
                case 'R':
                    rCount ++;
                    break;
                case 'L':
                    lCount ++;
                    break;
                case 'U':
                    uCount ++;
                    break;
                case 'D':
                    dCount ++;
                    break;
                default:
                    break;
            }
        }
        return (rCount == lCount) && (uCount == dCount);
        
    }
}
```

看到评论区有人的解法中，多加了一次判断，仅String的长度为偶数，才有可能构成环，所以不是偶数直接return false。可以再节省一部分时间。

```java
    if(moves.length() % 2 != 0)
    {
        return false;
    }
```

2. 换成仅用2个额外的int变量计算，大同小异：
```java
public class Solution {
    public boolean judgeCircle(String moves) {
        int x = 0;
        int y = 0;
        for (char ch : moves.toCharArray()) {
            if (ch == 'U') y++;
            else if (ch == 'D') y--;
            else if (ch == 'R') x++;
            else if (ch == 'L') x--;
        }
        return x == 0 && y == 0;
    }
}
```

3. 利用String.split方法，仅需2行代码，但效率应该比我的低：
```java
 public boolean judgeCircle(String moves) {
        moves=" " + moves + " ";
        return moves.split("L").length==moves.split("R").length && moves.split("U").length == moves.split("D").length;
    }
```
    
