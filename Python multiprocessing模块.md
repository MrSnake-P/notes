[TOC]

## Pool对象

​	进程池对象，提供一种快捷的方法，将输入数据分配给不同进程处理。

* 使用进程池的方式快捷的进行一个多进程操作

  ```python
  import time
  from multiprocessing import Process, Pipe, Pool
  
  
  def start(x):
      return x*x*x
  
  if __name__ == '__main__':
      with Pool(5) as p:      # 创建一个大小为5的进程池
          print(p.map(start,[i for i in range(4)]))   
  # map方法，可以将第二参数的值传到第一个参数中，也就是函数中
  
  ```

## Process类

 	创建对象，再用start()生成进程，与多线程类似。也可以用重载run()的方式自定义多进程。

* 简单的例子

```python
import time
from multiprocessing import Process


def example(name):
    print("l am a {}".format(name))
    time.sleep(2)
    print('end')


if __name__ == '__main__':
    # with Pool(5) as p:      # 创建一个大小为5的进程池
    #     print(p.map(start,[i for i in range(4)]))   # map方法，可以将第二参数的值传到第一个参数中，也就是函数中
    print("mainProcess")
    process1 = Process(target=example, args=("snake",))
    process2 = Process(target=example, args=("tom",))
   	# process1.daemon = process2.daemon = True 
    # 如何只剩下守护进程程序退出
    process1.start()
    process2.start()
-------------------------------------------------------------------
mainProcess
l am a tom
l am a snake
end
end
```

* 自定义,等同于上面

  ```python
  class Start(Process):
      def __init__(self,name):
          Process.__init__(self)
          self.name = name
  
      def run(self):
          print("l am a {}".format(self.name))
          time.sleep(2)
          print('end')
          
          
  if __name__ == '__main__':
      print("mainProcess")
      process1 = Process(target=example, args=("snake",))
      process2 = Process(target=example, args=("tom",))
      # process1.daemon = process2.daemon = True
      process1.start()
      process2.start()
  ```

## 在进程之间交换对象

### 队列

​	Queue类是一个近似`queue.Queue`的克隆

```python
from multiprocessing import Queue,Process


def example2(q):
    q.put([38, "snake", 'hello'])

if __name__ == '__main__':
    q = Queue()
    p = Process(target=example2, args=(q,))
    p.start()
    print(q.get())    # prints "[38, "snake", 'hello']"
    p.join()
```

### 管道

​	Pipe()函数返回一个由管道连接的连接对象，默认为双工，返回的两个对象表示Pipe()表示管道的两端，每个连接对象都send()和recv()方法。并且两个进程不能同时写入和读取同一端。

```python
from multiprocessing import Pipe,Process


def customers(c):
    """管道交换对象"""
    c.send('l am a snake')
    c.close()


if __name__ == '__main__':
    producer, customer = Pipe()
    process = Process(target=customers, args=(customer,))
    process.start()
    print(producer.recv())
    process.join()
```

## 进程间同步（进程锁）

确保一次只有一个进程打印到标准输出。

```python
from multiprocessing import Process, Lock


def f(l, i):
    l.acquire()		# 上锁确保一次只有一个进程访问
    try:
        print('hello world', i)
    finally:
        l.release()


if __name__ == '__main__':
    lock = Lock()
    l = []

    for num in range(10):	# 创建10个进程
        p = Process(target=f, args=(lock, num))
        l.append(p)
        p.start()

    for p in l:	# 等待子进程结束
        p.join()
```

## 进程间共享状态

### 共享内存

使用Value或Array将数据存储在共享内存映射中