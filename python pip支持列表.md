### 查看pip支持的版本

```python
import pip._internal


print(pip._internal.pep425tags.get_supported())
```

### 将模块放入sys模块中，便于导入

```python
import sys


sys.path.append(os.path.dirname(os.path.abspath(__file__))) 
# 获得当前目录的绝对路径
# 将路径放到python模块路径中
```

