[TOC]

## 同步队列类

​		queue模块实现了多生产、多消费者队列。Queue类实现了所有所需的锁定语义。

## Queue对象

| 方法                                        | 作用                                     |
| ------------------------------------------- | ---------------------------------------- |
| `Queue.qsize()`                             | 返回队列的大小                           |
| `Queue.empty()`                             | 判断队列是否为空                         |
| `Queue.full()`                              | 判断队列是否满                           |
| `Queue.put(item, block=True, timeout=None)` | 将item放入队列，                         |
| `Queue.get(block=True, timeout=None)`       | 从队列中移除一个项目                     |
| `Queue.task_done()`                         | 表示前面排队的任务已经完成               |
| `Queue.join()`                              | 阻塞至队列中所有的元素都被接受和处理完毕 |

### 等待排队的任务被完成的示例

Queue，在处理数据时，取数据在前面，添数据在后面

```python
import threading, queue

q = queue.Queue()

def worker():
    while True:
        item = q.get()  # 取队列中的数据
        print(f'Working on {item}')
        print(f'Finished {item}')
        q.task_done()   # 一个get()任务处理完毕

# 建立多线程处理任务
threading.Thread(target=worker, daemon=True).start()

# 将3个数据放入队列中
for item in range(3):
    q.put(item)
print('All task requests sent\n', end='')

"""等待所有元素都被处理和接受完毕，每调用一次task_done()
    表示完成一个条目，未完成条目减一，当计数为0时，阻塞解除"""
q.join()
print('All work completed')
```

### `put()、get()方法中block、timeout参数`

```python
>>> from queue import Queue
>>> q = Queue(1)	# 设置最大长度为1
>>> q.put(1)	# 添加1个数据
>>> print(q.get())	# 取出数据
1
>>> print(q.get(block=False))
# 由于长度为1，那么再次调用，则会一直堵塞。
# 如果将block设置为false，直接引发错误
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "D:\software\python\lib\queue.py", line 167, in get
    raise Empty
_queue.Empty
>>> print(q.get(timeout=1))
# 阻塞2秒后，引发empty错误
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "D:\software\python\lib\queue.py", line 178, in get
    raise Empty
_queue.Empty
```

