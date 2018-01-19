1. parameterType="Map"可用来传多个过滤参数，key作为参数名称，value为参数值

2.批量in过滤：
```sql
<select id="getXXXByColum1AndColum2s" resultType="xxx" parameterType="Map">
    select uuid, colum1, colum2 from table1 where colum1=#{colum1} and colum2 in
    <foreach collection="colum2s" item="colum2" open="(" close=")" separator=",">
        #{colum2}
    </foreach>
</select>
```

3.批量PUT操作，无则创建，有则更新
实现关键：on duplicate key
```sql
<update id="updatexxx" parameterType="Map">
    insert into table1(uuid, colum1, colum2, colum3) values
    <foreach collection="list" index="index" item="item" separator=",">
    ( #{item.uuid}, #{item.colum1}, #{item.colum2},
        <if test="item.colum3 != null">
            #{item.colum3}
        </if> )
    </foreach>
    on duplicate key update colum1=values(colum1), colum2=values(colum2);
</update>
```
