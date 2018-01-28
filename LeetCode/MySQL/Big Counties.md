There is a table World

+-----------------+------------+------------+--------------+---------------+
| name            | continent  | area       | population   | gdp           |
+-----------------+------------+------------+--------------+---------------+
| Afghanistan     | Asia       | 652230     | 25500100     | 20343000      |
| Albania         | Europe     | 28748      | 2831741      | 12960000      |
| Algeria         | Africa     | 2381741    | 37100000     | 188681000     |
| Andorra         | Europe     | 468        | 78115        | 3712000       |
| Angola          | Africa     | 1246700    | 20609294     | 100990000     |
+-----------------+------------+------------+--------------+---------------+
A country is big if it has an area of bigger than 3 million square km or a population of more than 25 million.

Write a SQL solution to output big countries' name, population and area.

For example, according to the above table, we should output:

+--------------+-------------+--------------+
| name         | population  | area         |
+--------------+-------------+--------------+
| Afghanistan  | 25500100    | 652230       |
| Algeria      | 37100000    | 2381741      |
+--------------+-------------+--------------+


##### 我的答案：OR
```sql
SELECT name, population, area FROM world WHERE area > 3000000 OR population > 25000000
```
##### 可能更快的写法：UNION
```sql
SELECT name, population, area FROM world WHERE area > 3000000

UNION

SELECT name, population, area FROM world WHERE population > 25000000
```

为何UNION比OR快？一般这种表都会对过滤字段建索引，假设population和area分别有索引，但or只能命中第一个字段的索引，第二个字段还是要全表扫描。
而UNION是分别建立子查询，子查询都能命中索引，再把查询结果合并。不过具体还是要看表有没有对population和area分别建立索引。如果没有，那么UNION肯定没有OR查询快。

更多内容可以查看：
mysql 实战 or、in与union all 的查询效率
http://www.cnblogs.com/shipengzhi/archive/2012/09/20/2695825.html
