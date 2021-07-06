[TOC]

## pymysql 常用指令

* 连接数据库

```python
db = pymysql.connect(host='localhost', user='root', password='JJP123.', port=3306, db='internship')
```

* 数据的插入

```python
def insert_data(table,key,data):
	curspr = db.cursor()
    sql = 'insert into {}({})values(%s)'.format(table,key)
    try:
        # execute执行一条 executemany执行多条
        if cursor.executemany(sql, data):	
            print('Success')
            db.commit()					# 执行提交，插入数据
	except:
        print('Failed')
        db.rollback(	db.close()
```

* 查询数据

```python
def select_data(sql):
	cursor = db.cursor()
    try:
            rows = cursor.execute(sql)
            print('Success' + '查询的行数为' + str(rows))
            return (cursor.fetchall())
    except:
        print('Failed')
        db.rollback()
    db.close()
```

## pymysql 读取文本插入数据库

* 在mysql里边直接执行文件 

```python
load data infile 'file name .txt'into table '表名''字段名'
```

* 文本字段里以制表符分隔

```python
load data infile 'file name .txt'into table '表名''字段名'
terminated by 't'
```

* 文本字段双引号包裹着分割

```python
enclosed by ""
```

