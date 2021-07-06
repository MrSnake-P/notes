[TOC]

线程和进程：一个程序对应一个进程，一个进程至少有一个线程。

多进程：只要cpu不限制，那么多个进程能同时运行。

多线程：同一个进程中多个线程可以并发执行。

多线程适用于爬虫IO密集型。

## 并行

```python
# 一个简单的多线程
import threading
import time


def example():
    thread = threading.current_thread().name  # 返回当前线程名
    time.sleep(1)
    print(thread + " l am a snake")
    time.sleep(1)
    print(thread + ' l am a dog')


def example2():
    thread = threading.current_thread().name  # 返回当前线程名
    time.sleep(1)
    print(thread + ' l am a snake,too')
    time.sleep(1)
    print(thread + ' l am a dog,too')


def mainthread():
    print(threading.current_thread().name)  # 主线程先运行
    time.sleep(1)
    thread1 = threading.Thread(target=example, )
    thread2 = threading.Thread(target=example2, )
    thread1.start()  # 开始线程
    thread2.start()
    thread1.join()  # join的作用在于直到该线程执行完毕，主线程才停止
    thread2.join()
    print("MainThread is over")


if __name__ == '__main__':
    mainthread()
------------------------------------------------------------------
MainThread
Thread-1 l am a snake
Thread-2 l am a snake,too
Thread-1 l am a dog
Thread-2 l am a dog,too
MainThread is over
```

## 自定义线程方法

* 重载`run()`方法

```python
class Customize_Thread(threading.Thread):
    def __init__(self):
        threading.Thread.__init__(self)

    def run(self):
        thread = threading.current_thread().name  # 返回当前线程名
        time.sleep(1)
        print(thread + ' l am a snake')
        time.sleep(1)
        print(thread + ' l am a dog')

def customize_thread():
    thread = Customize_Thread()
    thread.start()


if __name__ == '__main__':
    for i in range(5):
        customize_thread()
------------------------------------------------------------------ 
Thread-2 l am a snake
Thread-1 l am a snake
Thread-5 l am a snake
Thread-4 l am a snake
Thread-3 l am a snake
Thread-2 l am a dog
Thread-3 l am a dog
Thread-1 l am a dog
Thread-5 l am a dog
Thread-4 l am a dog
```

## 守护进程

​	被设置为守护线程（`setDaemon`）的线程，当剩下的线程都是守护线程时，整个程序退出。

主线程不是守护线程，因此主线程创建的所有线程默认都是 [`daemon`](https://docs.python.org/zh-cn/3/library/threading.html#threading.Thread.daemon) = `False`。

```python
def example():
    thread = threading.current_thread().name  # 返回当前线程名
    time.sleep(1)
    print(thread + " l am a snake")
    time.sleep(1)
    print(thread + ' l am a dog')


def example2():
    thread = threading.current_thread().name  # 返回当前线程名
    time.sleep(1)
    print(thread + ' l am a snake,too')
    time.sleep(1)
    print(thread + ' l am a dog,too')


def mainthread():
    print(threading.current_thread().name)  # 主线程先运行
    time.sleep(1)
    thread1 = threading.Thread(target=example, )
    thread2 = threading.Thread(target=example2, )
    thread1.setDaemon(True)
    thread2.setDaemon(True)
    thread1.start()  	# 开始线程
    thread2.start()
   	# thread1.join()	# 不能添加join方法
   	# thread2.join()
    print("MainThread is over")
```

> **note：`join()` 方法会影响守护进程，使得守护进程失效，仍然会等线程运行完毕。**

## 锁对象

​	在多线程情况下，我们同时多次调用线程中方法，这可能不是我们所期望的。我们希望运行完一次方法后，下一个线程才能继续，那么就用到加锁。加锁之后，确保同一时间只有一个线程在运行。

即不要在多线程中同时使用一个代理对象，除非你用锁保护它。

（而在不同进程中使用 *相同* 的代理对象却没有问题。）

```python
import threading
import time
from threading import Lock


def thread_lock(l):
    global k
    l.acquire()
    try:
        temp = k
        time.sleep(0.1)
        k = temp-1
    finally:
        l.release()
        # print(k)


if __name__ == '__main__':
    lock = Lock()
    k = 38
    list = []
    for i in range(10):   # 创建10个线程
        thread = threading.Thread(target=thread_lock,args=(lock,))
        thread.start()
        list.append(thread)

    for i in list:  # 等待线程完成
        i.join()
    print(k)
    -------------------------
    28
```

## 信号量

​		信号量(Semaphore)，有时被称为信号灯，是在[多线程](https://baike.baidu.com/item/多线程/1190404)环境下使用的一种设施，是可以用来保证两个或多个关键代码段不被[并发](https://baike.baidu.com/item/并发/11024806)调用。在进入一个关键代码段之前，线程必须获取一个信号量；一旦该关键代码段完成了，那么该线程必须释放信号量。其它想进入该关键代码段的线程必须等待直到第一个线程释放信号量。为了完成这个过程，需要创建一个信号量VI，然后将Acquire Semaphore VI以及Release Semaphore VI分别放置在每个关键代码段的首末端。确认这些信号量VI引用的是初始创建的信号量。[^1]

**信号量允许一个任务有指定个数的线程访问**

```python
import threading
import time
from threading import Semaphore


def thread_sem(sem=0):
    with sem:
        time.sleep(1)
        print(threading.current_thread().name,end='\t')
        print("l am a snake")
        time.sleep(1)
        print('-'*30)



if __name__ == '__main__':
    list = []
    semaphore = threading.BoundedSemaphore(5)   # 最大允许5个线程访问
    for i in range(10):   # 创建10个线程
        thread = threading.Thread(target=thread_sem,
                                  args=(semaphore,))
        thread.start()
        list.append(thread)
    for i in list:  # 等待线程完成
        i.join()
-------------------------------------------------------------------
# 输出可以看到一次同时只有5个进程访问
Thread-3	l am a snake
Thread-2	l am a snake
Thread-1	l am a snake
Thread-4	l am a snake
Thread-5	l am a snake
------------------------------
------------------------------
------------------------------
------------------------------
------------------------------
Thread-6	l am a snake
Thread-7	l am a snake
Thread-9	l am a snake
Thread-8	l am a snake
Thread-10	l am a snake
------------------------------
------------------------------
------------------------------
------------------------------
------------------------------
```

## 事件对象

线程之间通信的最简单机制之一，一个线程发出事件信号，其他线程等待该信号

| 方法       | 作用                                                         |
| ---------- | ------------------------------------------------------------ |
| `is_set()` | 仅当内部表示为true时返回`True`                               |
| `set()`    | 将内部表示设置为true，所有等待这个事件的线程被唤醒，并且 `wait()`方法不会被阻塞 |
| `clear()`  | 将内部表示设置为false，`wait()`方法被阻塞                    |
| `wait()`   | 内部变量false时，阻塞，直到调用`set()`。true时，立即返回     |

```python
import threading
import time


def lineup(event):
    """排队吃饭，人满了，前面有10人"""
    customers = 10
    while True:
        try:
            event.clear()	# 内部标识设置为false
            print("Thera are still ", customers)
            time.sleep(1)
            customers -= 1
        except:
            print("over")
        finally:	# 当顾客排完队，发出信号true，可以用餐
            if customers == 0:
                event.set()
                break


def dining(event):
    """一直等待，等待20人拍完队伍"""
    while True:
        if event.is_set():	# 判断内部标识
            print("\033[0;41mlt's my turn\033[0m")
            break
        else:	# 
            print("l must waiting")
            event.wait()	# 会被阻塞，知道排队事件发出信号true


if __name__ == '__main__':
    event = threading.Event()
    line = threading.Thread(target=lineup,args=(event,))
    dining = threading.Thread(target=dining,args=(event,))
    line.start()
    dining.start()
-------------------------------------------------------------------
Thera are still  10
l must waiting
Thera are still  9
Thera are still  8
Thera are still  7
Thera are still  6
Thera are still  5
Thera are still  4
Thera are still  3
Thera are still  2
Thera are still  1
lt's my turn
```

## 定义的函数

| 函数                         | 作用                     |
| ---------------------------- | ------------------------ |
| `threading.active_count()`   | 返回存活Thread对象的数量 |
| `threading.current_thread()` | 返回当前控制的线程对象   |

| 方法          | 作用                                             |
| ------------- | ------------------------------------------------ |
| `start()`     | 开始线程                                         |
| `run()`       | 重载这个方法，与构造Thread对象相同               |
| `join`()      | 主线程等待线程结束而结束，并且可以设置超时参数。 |
| `is_alive()`  | 返回线程是否存活                                 |
| `setDaemon()` | 设置守护进程                                     |

[^1]: https://baike.baidu.com/item/%E4%BF%A1%E5%8F%B7%E9%87%8F/9807501?fr=aladdin ↩