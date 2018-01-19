1. 批量操作必须用分页，原则上不要处理json字段（大字段）
2. 频繁使用的查询条件必须建索引，使用前缀索引，避免全表扫描 idx_xxx
3. 前缀索引对order by语句失效，通过order by过滤的字段需要建立全索引
4. 谨慎定义字段类型，能用tinyint就不用int，能用char就不用varchar
5. 在ibatis中杜绝使用association
6. in后面的数据最好一次不超过10条。
