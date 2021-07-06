[TOC]

# 协程

一个简单的示例，需要python3.7+

运行一个协程，asyncio提供了三种机制：

* 等待协程`await`
* 运行创建的协程`asyncio.run()`
* 实现并发`asyncio.create_task()`

```python
import asyncio
import time


async def greetings(name):  # 用async声明协程任务
    # print("hello " + name)
    await asyncio.sleep(2)  # await定义可等待对象，意为可以挂起
    print('nice to meet you ' + name)


async def main():
    start = time.time()
    
    # 用asyncio.create_task使任务并发运行
    task1 = asyncio.create_task(greetings("snake"))
    task2 = asyncio.create_task(greetings("tom"))
    # 等待创建的任务完成
    await task1
    await task2
    
    print("总耗时：" + str(time.time()-start))


asyncio.run(main())	# 运行协程程序，python3.7+有效
------------------------------------------------------------------
nice to meet you snake
nice to meet you tom
总耗时：2.0054218769073486
```

## 可等待对象

如果一个对象可以在 [`await`](https://docs.python.org/zh-cn/3/reference/expressions.html#await) 语句中使用，那么它就是 **可等待** 对象。

*可等待* 对象有三种主要类型: **协程**, **任务** 和 **Future**。

> 协程属于可等待对象，可以在其他协程中被等待
>
> 任务被用来设置日程，使得并发执行协程
>
> > 当一个协程通过 [`asyncio.create_task()`](https://docs.python.org/zh-cn/3/library/asyncio-task.html#asyncio.create_task) 等函数被打包为一个 *任务*，该协程将自动排入日程准备立即运行
>
> 当一个 Future 对象 *被等待*，这意味着协程将保持等待直到该 Future 对象在其他地方操作完毕。

## 创建任务

* 将协程创建为一个task加入日程，准备执行

  `asyncio.create_task(coro, *, name=None)`

* 在python3.7之前更为底层的创建函数

  `asyncio.ensure_future()`

## python3.6协程函数示例

```python
import asyncio
import time


async def greetings(name):  # 用async声明异步任务
    # print("hello " + name)
    await asyncio.sleep(2)  # await定义可等待对象，意为可以挂起
    print('nice to meet you ' + name)


start = time.time()
loop = asyncio.get_event_loop() # 获取事件循环
# asyncio.wait()参数为可迭代对象，处理多个协程任务
loop.run_until_complete(asyncio.wait([greetings("snake"),greetings("tom")]))
print("总耗时: " + str(time.time() - start))
------------------------------------------------------------------
nice to meet you tom
nice to meet you snake
总耗时: 2.00179123878479
```

