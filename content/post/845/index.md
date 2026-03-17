---
title: "Mysql数据库唯一约束和触发器的使用"
slug: "845"
date: 2025-04-18T23:40:00+08:00
lastmod: 2025-04-18T23:40:18+08:00
categories: ["技术"]
tags: ["linux", "Windows", "mysql"]
draft: false
---

## 说明

最近写了一点Mysql数据库东西，记录一下使用方法

### 步骤

1.创建触发器（自动计算content_hash）

```sql
DELIMITER //
CREATE TRIGGER before_insert_oneSentence
BEFORE INSERT ON oneSentence
FOR EACH ROW
BEGIN
    -- 插入前自动计算content的MD5值
    IF NEW.content_hash IS NULL THEN
        SET NEW.content_hash = LOWER(MD5(NEW.content)); -- 转小写确保一致性
    END IF;
END;
//
DELIMITER ;
```

2.为旧数据补充content_hash

```
-- 先处理旧数据（无content_hash的记录）
UPDATE oneSentence 
SET content_hash = LOWER(MD5(content)) 
WHERE content_hash IS NULL;
```

3.添加唯一索引（防重复）

```sql
-- 确保content_hash列有唯一索引
ALTER TABLE oneSentence 
ADD UNIQUE INDEX idx_unique_content_hash (content_hash);
```

#### 问题

如果以前创建过，会报：

```bash
Warning: #1831 Duplicate index 'idx_unique_content_hash' defined on the table xxx
```

这个错误是有重复的索引

1.查看所有索引

```sql
SHOW INDEX FROM your_table_name;
```

2.删除重复的索引

```sql
DROP INDEX idx_unique_content_hash ON your_table_name;
```


```sql

```