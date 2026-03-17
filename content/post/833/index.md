---
title: "数据持久化存储-微信每日发送问候更新"
slug: "833"
date: 2024-10-04T18:39:00+08:00
lastmod: 2024-10-04T18:40:45+08:00
categories: ["随笔"]
tags: ["python", "微信"]
draft: false
---

## 前言

之前发过一篇文章，介绍如何用微信公众号发送早晚安

[bspost cid="112"]

调用接口获取的情话，有时候会超出字符限制，如果自己写一个情话，就可以完全自定义，下面记录方法。

### 正文

刚开始想到的是写死在脚本里面，直接情话都放在数组里，写一个方法，类似于下面这种：

```py
def getGreeting():
    greetingHub = ['我爱你','我喜欢你','这是一句情话','这是另一句情话']
    currentGreeting = random.choice(greetingHub)
    return currentGreeting
```

这种方案也可以，只是无法做到每天按数组序号依次发送，也无法做到随机发送一条，第二天发送除了第一天发过的数据中的一个。

这就需要数据持久化，有两个方案，一个是把情话写在文本文件里，脚本直接访问文本文件，另一个是写在数据库里，通过数据库来访问

#### 文本文件

示例代码如下：

```py
def getNightGreeting():
    sentence = [
        # 所有情话列表...
    ]
    try:
        with open("night_greeting_index.txt", "r") as file:
            index = int(file.read().strip())
    except (FileNotFoundError, ValueError):
        index = 0

    currentSentence = sentence[index]
    new_index = (index + 1) % len(sentence)

    with open("night_greeting_index.txt", "w") as file:
        file.write(str(new_index))

    return currentSentence
```

#### 数据库方案

以Mysql为例，代码如下：

```py
import mysql.connector
from mysql.connector import Error

    
# 数据库连接配置
db_config = {
    'host': '127.0.0.1',
    'user': 'xxx',
    'password': 'xxx',
    'database': 'xxx'
}

def connect_to_db(config):
    """尝试连接到数据库"""
    try:
        conn = mysql.connector.connect(**config)
        return conn
    except Error as e:
        print(f"Error connecting to MySQL Database: {e}")
        return None

def getNightGreeting():
    """从数据库获取每日情话，未使用的，并更新状态"""
    conn = connect_to_db(db_config)
    if conn is not None:
        cursor = conn.cursor()
        try:
            # 选择一个未使用的情话
            cursor.execute("SELECT id, greeting FROM daily_greetings WHERE used = 0 ORDER BY RAND() LIMIT 1") # 随机抽取（默认）
            # cursor.execute("SELECT id, greeting FROM daily_greetings WHERE used = 0 LIMIT 1") # 顺序抽取（可选）
            result = cursor.fetchone()
            if result:
                greeting_id, greeting = result
                # 更新为已使用
                cursor.execute("UPDATE daily_greetings SET used = 1, date_used = CURDATE() WHERE id = %s", (greeting_id,))
                conn.commit()
                return greeting
            else:
                # 如果所有情话都已使用，重置状态
                cursor.execute("UPDATE daily_greetings SET used = 0, date_used = NULL")
                conn.commit()
                # 递归调用自身，再次尝试获取情话
                return getNightGreeting()
        except Error as e:
            print(f"Database error: {e}")
        finally:
            cursor.close()
            conn.close()
    else:
        # 如果连接失败，返回默认情话
        return "我爱你"
```

代码里面标注了随机和顺序的两种方法，默认使用随机，对应的Mysql建表语句如下：

```sql
CREATE TABLE daily_greetings (
    id INTEGER PRIMARY KEY AUTO_INCREMENT,
    greeting TEXT NOT NULL,
    used BOOLEAN NOT NULL DEFAULT 0,
    date_used DATE,
    created_at TIMESTAMP DEFAULT CURRENT_TIMESTAMP
);
```

对应的插入语句如下：

```sql
INSERT INTO daily_greetings (greeting, used) VALUES ('我爱你', 0);
```

一次插入多条：

```sql
INSERT INTO daily_greetings (greeting, used) VALUES ('I love you', 0),('我爱你', 0);
```

最后，请记得安装对应的Python模块，MySQL的模块安装命令如下：

```bash
pip install mysql-connector-python
```

## 结语

建议使用文本文件方案，因为简单，这个东西本来也不复杂，没必要弄数据库，但数据库的好处就是扩展性高一些，更多的选择