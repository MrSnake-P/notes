---

[fastapi 官方文档](https://fastapi.tiangolo.com/)
[fast admin 文档](https://github.com/long2ice/fastapi-admin)
---

[TOC]

## 创建一个简单的接口

```
import os
from fastapi import FastAPI
app = FastAPI()

# --------------------------------------------------------------------------------
@app.get("/home")
async def test():
    return "hello,wold"


if __name__ == '__main__':
    os.system("uvicorn main:app --reload --port 5000  --host 0.0.0.0")
```

## 跨域

```
from fastapi.middleware.cors import CORSMiddleware

app = FastAPI()

# 前端页面url
origins = [
    "http://localhost.tiangolo.com",
    "https://localhost.tiangolo.com",
    "http://localhost",
    "http://localhost:8080",
    "http://192.168.31.35",
]

# 后台api允许跨域
app.add_middleware(
    CORSMiddleware,
    allow_origins=origins,
    allow_credentials=True,
    allow_methods=["*"],
    allow_headers=["*"],
)
```

## 连接 MySQL 数据库

```python
sql_url = 'mysql+pymysql://root:xxxx@localhost:3306/database'
engine = create_engine(sql_url, pool_pre_ping=True)
SessionLoacal = sessionmaker(autocommit=False, autoflush=False, bind=engine)
# autocommit自动提交，autocommit自动刷新加载
session = SessionLocal()

Base = declarative_base()	# 模型继承,类似django的models.Model

class User(Base):
    __tablename__ = "user1"
    username = Column(String(18), primary_key=True, index=True)
    
    
Base.metadata.create_all(bind=engine)	# 迁移模型


# 插入数据
@app.get("/")
async def read_root():
    db_obj = User(
        username='snake'
    )
    session.add(db_obj)
    session.commit()
    session.refresh(db_obj)
    return {"Hello": "World"}


if __name__ == '__main__':
    os.system("uvicorn main:app --reload --port 5000 --host 0.0.0.0")
```

## Pydantic

[官方文档](https://pydantic-docs.helpmanual.io/usage/models/)

**pydantic 在运行时强制执行类型提示，并在数据无效时提供用户友好的错误。**

  ```python
from datetime import datetime
from typing import List, Optional
from pydantic import BaseModel


class User(BaseModel):
    id: int		# 告诉pydantic该字段是必填字段。字符串，字节或浮点数会被强制转换整数
    name = 'John Doe'	# 具有默认值，不是必须
    signup_ts: Optional[datetime] = None # 处理unix timestamp int代表日期的字符串
    #
    friends: List[int] = []		# 需要一个整数列表，类似的将转换为整数


external_data = {
    'id': '123',
    'signup_ts': '2019-06-01 12:22',
    'friends': [1, 2, '3'],
}
user = User(**external_data)
print(user.id)
#> 123
print(repr(user.signup_ts))
#> datetime.datetime(2019, 6, 1, 12, 22)
print(user.friends)
#> [1, 2, 3]
print(user.dict())
"""
{
    'id': 123,
    'signup_ts': datetime.datetime(2019, 6, 1, 12, 22),
    'friends': [1, 2, 3],
    'name': 'John Doe',
}
"""
  ```

* 如果验证失败，pydantic 会引发错误。

  ```
  from pydantic import ValidationError
  
  try:
      User(signup_ts='broken', friends=[1, 2, 'not number'])
  except ValidationError as e:
      print(e.json())
  ```

### 1.Model

在 pydantic 中定义对象的主要方法是通过模型（模型只是继承 自的类 BaseModel）

* 基本模型用法

  ```python
  from pydantic import BaseModel
  
  class User(BaseModel):
      id: int
      name = 'Mr Snake'
  
      
  user = User(id="1") # 进行初始化，未引发 ValidationError，模式是有效的
  assert user.id == 123	# 验证是否转化为了整数
  ```

* 查看初始化用户时提供的字段

  ```python
  >>> user.__fields_set__
  {'id'}
  ```

* 提供字段的字典形式并且可以修改

  ```python
  >>> user.dict()
  {'id': 1, 'name': 'Mr Snake'}
  >>> dict(user)
  {'id': 1, 'name': 'Mr Snake'}
  >>> user.id = 100		# 修改id字段的值
  >>> user.dict()
  {'id': 100, 'name': 'Mr Snake'}
  ```

  * 模型的方法和属性

    | 属性和方法       | 作用                                                         |
    | ---------------- | ------------------------------------------------------------ |
    | `dict()`         | 返回模型的字段和值的字典                                     |
    | `json()`         | 返回JSON字符串表示dict()                                     |
    | `copy()`         | 返回模型的副本（默认为浅表副本）                             |
    | `parse_obj()`    | 用于在对象不是字典的情况下通过错误处理将任何对象加载到模型中 |
    | `parse_raw()`    | 用于加载多种格式的字符串的使用程序                           |
    | `parse_file()`   | 类似`parse_raw()`但用于文件路径                              |
    | `from_orm()`     | 将数据从任意类加载到模型中                                   |
    | `schema()`       | 返回将模型表示为JSON Schema的字典                            |
    | `schema_json()`  | 返回的JSON子符串表示形式`schema()`                           |
    | `construct()`    | 用于创建模型而无需运行验证的类方法                           |
    | `__fields_set__` | 初始化模型实例时设置的字段名称集                             |
    | `__fields__`     | 模型领域的字典                                               |
    | `__config__`     | 模型的配置类                                                 |

    