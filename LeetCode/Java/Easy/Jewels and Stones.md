You're given strings J representing the types of stones that are jewels, and S representing the stones you have.  Each character in S is a type of stone you have.  You want to know how many of the stones you have are also jewels.

The letters in J are guaranteed distinct, and all characters in J and S are letters. Letters are case sensitive, so "a" is considered a different type of stone from "A".
```
Example 1:

Input: J = "aA", S = "aAAbbbb"
Output: 3
Example 2:

Input: J = "z", S = "ZZ"
Output: 0
```
Note:

S and J will consist of letters and have length at most 50.
The characters in J are distinct.

解法1： 讨论中给的，5行代码搞定，运行时间18ms。不过没有判断输入有效性和合法性。
```java
public int numJewelsInStones(String J, String S) {
    int result = 0;
    Set jSet = new HashSet();
    for(char j : J.toCharArray()) { jSet.add(j); }
    for(char s : S.toCharArray()) { if(jSet.contains(s) result ++; }
    return result;
}
```

解法2： 脑袋坏掉了要自己造个数组存J，调试了半天边界值，运行耗时21ms

```java
/**
     * 1. 判空，字符串长度
     * 2. J中有重复字符
     * 3. J或S中有非字母
     * @param J
     * @param S
     * @return
     */
    public int numJewelsInStones(String J, String S) {
        final int MAX_SIZE = 50;
        final int ALPHABET_SIZE = 52;

        if(J == null || S == null || J.length() > MAX_SIZE || S.length() > MAX_SIZE)
        {
            return 0;
        }

        byte[] jArray = J.getBytes();
        byte[] sArray = S.getBytes();

        //将“J”中的字母直接存进数组中作为索引，前26个为小写，后26个大写。jContainer[index]为0表示J中没有这个字母。方便S中进行比较
        int[] jContainer = new int[ALPHABET_SIZE];
        int index = 0;
        for(int i = 0; i < jArray.length; i++)
        {
            index = getIndex(jArray[i]);
            if(index < 0 || index > ALPHABET_SIZE - 1)
            {
                //不是字母，直接返回
                return 0;
            }
            if(jContainer[index] == 1)
            {
                //存在重复数据，直接返回
                return 0;
            }
            jContainer[index] = 1;
        }

        int count = 0;
        for(int k = 0; k < sArray.length; k++)
        {
            index = getIndex(sArray[k]);
            if(index < 0 || index > ALPHABET_SIZE - 1)
            {
                //不是字母，直接返回
                return 0;
            }
            if(jContainer[index] == 1)
            {
                count ++;
            }
        }
        return count;
    }

    private int getIndex( byte b) {
        final byte lowerA = 'a';
        final byte upperA = 'A';
        int index;
        if(b - lowerA >= 0)
        {
            //小写字母
            index = b - lowerA;
        }
        else
        {
            //大写字母
            index = b - upperA + 26;
        }
        return index;
    }
```
