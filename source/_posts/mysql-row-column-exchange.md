---
title: mysql 行列转换
categories: 其他
tags: []
date: 2017-02-13 15:25:00
---

准备数据
```sql
DROP TABLE IF EXISTS `score`;
CREATE TABLE `score` (
  `id` int(11) NOT NULL AUTO_INCREMENT,
  `name` varchar(255) DEFAULT NULL,
  `score` int(11) DEFAULT NULL,
  `type` varchar(255) DEFAULT '',
  PRIMARY KEY (`id`)
) ENGINE=InnoDB AUTO_INCREMENT=7 DEFAULT CHARSET=utf8;

INSERT INTO `score`
(`name`, `type`, `score`)
VALUES
('张三','语文',89),
('张三','数学',100),
('张三','英语',93),
('李四','语文',90),
('李四','数学',94),
('李四','英语',83);
```
score 表：
| name | score | type |
|----- | ----- | ---- |
| 张三 | 89 | 语文 |
| 张三 | 100 | 数学 |
| 张三 | 93 | 英语 |
|李四 | 90 | 语文 |
|李四 | 94 | 数学 |
|李四 | 83 | 英语 |

想要的效果：
| 姓名 | 语文 | 数学 | 英语 | 总分 |
| -----|------|-------| ---- | --- |
| 张三 | 89.00 | 100.00 | 93.00 | 282 |
| 李四 | 90.00 | 94.00 | 83.00 | 267 |
| 平均分 | 89.50 | 97.00 | 88.00 | - |


采用 IF 语法查询
```sql
SELECT name AS '姓名',
SUM(IF(type='语文', score, 0)) AS '语文',
SUM(IF(type='数学', score, 0)) AS '数学',
SUM(IF(type='英语', score, 0)) AS '英语',
SUM(score) AS '总分'
FROM score 
GROUP BY name

UNION ALL
SELECT '平均分',
ROUND(AVG(IF(type = '语文', score, NULL)), 2) AS '语文',
ROUND(AVG(IF(type = '数学', score, NULL)), 2) AS '数学',
ROUND(AVG(IF(type = '英语', score, NULL)), 2) AS '英语',
'-' AS '总分'
FROM score
```

使用 CASE 语法查询
```sql
SELECT name AS '姓名',
MAX(CASE type WHEN '语文' THEN score ELSE 0 END) AS '语文',
MAX(CASE type WHEN '数学' THEN score ELSE 0 END) AS '数学',
MAX(CASE type WHEN '英语' THEN score ELSE 0 END) AS '英语',
SUM(score) AS "总分"
FROM score 
GROUP BY `name`

UNION ALL
SELECT '平均分',
ROUND(AVG(CASE type WHEN '语文' THEN score ELSE NULL END), 2) AS '语文',
ROUND(AVG(CASE type WHEN '数学' THEN score ELSE NULL END), 2) AS '数学',
ROUND(AVG(CASE type WHEN '英语' THEN score ELSE NULL END), 2) AS '英语',
'-' AS '总分'
FROM score
```

> 函数 `AVG()` 忽略 `NULL` 值，而不是将其作为 0 参与计算，也就是说 取平均值的时候 NULL 不参与计数
