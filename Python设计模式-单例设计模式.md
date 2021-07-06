[TOC]

## 理解单例设计模式

单例设计模式的目的

* 确保类有且只有一个对象被创建。
* 为对象提供一个访问点，以使程序可以全局访问该对象。
* 控制共享资源的并行访问。

例如，操作数据库时，为了数据的一致性，需要确保每次只能生成一个实例。也就是说，对象在多次调用和创建时，返回同一个对象。

## 用代码实现经典的单例模式

1. 只允许 Singleton 类生成一个实例。

2. 如果已经有实例，则返回同一个对象。

```python
class Singleton(object):
    def __new__(cls):	# __new__ python 用于实例化对象的特殊方法
        if not hasattr(cls, 'instance'):	# hasattr用来了解对象是否具有某个属性
            # 检查cls是否具有属性instance，该属性检查该类是否生成一个对象
            cls.instance = super(Singleton, cls).__new__(cls)
            # 如果不具有直接调用父类的实例。
        return cls.instance
```

```python
>>> s1 = Singleton()		# s通过__new__方法创建。
>>> s2 = Singleton()
>>> print(s1)
>>> <__main__.Singleton object at 0x02E181D8>
>>>	print(s2)
>>>	<__main__.Singleton object at 0x02E181D8>
```

## 懒汉式实例化

他能确保在实际需要时才创建，能够节约资源。

## Monostate （单态）单例模式

* 实例共享相同的状态，关注状态和行为，而不是同一性。
* 基于所有对象共享相同状态。

```python
class Borg:
    __shared_state = {"1":"2"}
    def __init__(self):
        self.x = 1
        self.__dict__ = self.__shared_state 
        # __dict__是一个特殊变量，用来存储一个类所有对象的状态
        # 将__shared_state赋给已经创建的实例
        pass
    
>>> b = Borg()
>>> b1 = Borg()
>>> b,b1
(<__main__.Borg object at 0x03918370>, <__main__.Borg object at 0x039181F0>)
# 不同的对象
>>> b.__dict__,b1.__dict__
({'1': '2'}, {'1': '2'})
# 相同的值
>>> b.x = 4
>>> b.__dict__,b1.__dict__
# b对象的变量发生变化，会复制到被所有对象共享的__dict__变量
({'1': '2', 'x': 4}, {'1': '2', 'x': 4})
```

* 通过 `__new__` 实现 Borg 模式

```python
class Borg(object):
    _shared_state = {}
    def __new__(cls, *args, **kwargs):
        obj = super(Borg, cls).__new__(cls, *args, **kwargs)
        obj.__dict__ = cls._shared_state
        return obj
```

## 单例和元类

### 1. 元类

元类是一个类的类



