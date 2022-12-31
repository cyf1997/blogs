# msyql行列转换

## 行转列

创建表
```sql
CREATE TABLE `test` (
  `id` int(11) NOT NULL,
  `name` varchar(32) DEFAULT NULL,
  `subject` varchar(32) DEFAULT NULL,
  `score` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

插入数据
```sql
INSERT INTO `test` VALUES (1, '张三', '数学', '95');
INSERT INTO `test` VALUES (2, '张三', '语文', '80');
INSERT INTO `test` VALUES (3, '张三', '英语', '75');
INSERT INTO `test` VALUES (4, '李四', '数学', '45');
INSERT INTO `test` VALUES (5, '李四', '语文', '90');
INSERT INTO `test` VALUES (6, '李四', '英语', '66');
```

### 1.join方式

```sql
SELECT
	b.NAME,
	b.score AS '数学',
	c.score AS '语文',
	d.score AS '英语' 
FROM
	(SELECT score, NAME FROM test WHERE SUBJECT = '数学' ) b 
	INNER JOIN ( SELECT score, NAME FROM test WHERE SUBJECT = '语文' ) c USING ( NAME )
	INNER JOIN ( SELECT score, NAME FROM test WHERE SUBJECT = '英语' ) d USING ( NAME )
```

### 2.case...when

```sql
SELECT
	NAME,
	max(case subject when '数学' then score end) as '数学',
	max(case subject when '语文' then score end) as '语文',
	max(case subject when '英语' then score end) as '英语'
 from test	
 GROUP BY name 
```

### 3.if

```sql
SELECT
	NAME,
	sum(IF(subject='数学',score,0)) as '数学',
	sum(IF(subject='语文',score,0)) as '语文',
	sum(IF(subject='英语',score,0)) as '英语'
 from test	
 GROUP BY name 
```

## 列转行

创建表
```sql
CREATE TABLE `test2` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(32) DEFAULT NULL,
  `math` varchar(32) DEFAULT NULL,
  `chinese` varchar(32) DEFAULT NULL,
  `english` varchar(32) DEFAULT NULL,
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=3 DEFAULT CHARSET=utf8;
```

插入数据
```sql
INSERT INTO `test2` VALUES (1, '张三', '21', '26', '34');
INSERT INTO `test2` VALUES (2, '李四', '23', '56', '18');
```

### 1.union all
```sql
SELECT name,'数学' as subject , math as 'score' from test2
union all
SELECT name,'语文' as subject , chinese as 'score' from test2
union all
SELECT name,'英语' as subject , english as 'score' from test2
```
