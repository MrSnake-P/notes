[TOC]

## concurrent.futures 

该模块提供异步执行回调的高层接口

可以由 [`ThreadPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ThreadPoolExecutor) 使用线程或由 [`ProcessPoolExecutor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.ProcessPoolExecutor) 使用单独的进程来实现。

## ThreadPoolExecutor

`（max_workers=None, thread_name_prefix='', initializer=None, initargs=())`

> `max_workers` 指定线程数，线程池会异步执行调用
>
> `thread_name_prefix` 线程名称
>
> `initializer` 线程开始处调用的一个可选可调用对象
>
> `initargs` 传递给初始化器的元组参数

* 一个简单的例子

  ```python
  from concurrent.futures import ThreadPoolExecutor
  
  
  with ThreadPoolExecutor(max_workers=1) as executor:
      future = executor.submit(pow, 323, 1235)	# 返回future对象
      print(future.result())
  ```

* 异步执行请求多个网址

  ```python
  import concurrent.futures
  import urllib.request
  from concurrent.futures import ThreadPoolExecutor
  import time
  
  def timer(func):
      def deco(*args):
          start_time = time.time()
          func(*args)
          end_time = time.time()
          cost = end_time - start_time
          print("总共耗时：", cost)
  
      return deco
  
  URLS = ['http://www.foxnews.com/',
          'http://www.cnn.com/',
          'http://europe.wsj.com/',
          'http://www.bbc.co.uk/',
          'http://some-made-up-domain.com/']
  
  # Retrieve a single page and report the URL and contents
  def load_url(url, timeout):
      with urllib.request.urlopen(url, timeout=timeout) as conn:
          return conn.read()
  
  # 使用上下文管理器，不需要显示调用shutdown()
  with concurrent.futures.ThreadPoolExecutor(max_workers=5) as executor:
      # 创建future对象对应每个url
      future_to_url = {executor.submit(load_url, url, 60): url for url in URLS}
      for future in concurrent.futures.as_completed(future_to_url):
          # 返回future实例的迭代器
          url = future_to_url[future]
          try:
              data = future.result()
          except Exception as exc:
              print('%r generated an exception: %s' % (url, exc))
          else:
              print('%r page is %d bytes' % (url, len(data)))
  ---------------------------------------------------------------
  'http://www.foxnews.com/' page is 323030 bytes
  'http://some-made-up-domain.com/' page is 64668 bytes
  'http://europe.wsj.com/' generated an exception: <urlopen error [WinError 10060] 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。>
  'http://www.bbc.co.uk/' generated an exception: <urlopen error [WinError 10060] 由于连接方在一段时间后没有正确答复或连接的主机没有反应，连接尝试失败。>
  'http://www.cnn.com/' page is 1146114 bytes
  ```

## ProcessPoolExecutor 

`（max_workers=None, thread_name_prefix='', initializer=None, initargs=())`

> `max_workers` 指定线程数，线程池会异步执行调用
>
> `thread_name_prefix` 线程名称
>
> `initializer` 线程开始处调用的一个可选可调用对象
>
> `initargs` 传递给初始化器的元组参数

```python
import concurrent.futures
import math

PRIMES = [
    112272535095293,
    112582705942171,
    112272535095293,
    115280095190773,
    115797848077099,
    1099726899285419]

def is_prime(n):
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False

    sqrt_n = int(math.floor(math.sqrt(n)))  # 返回数字下舍取整
    for i in range(3, sqrt_n + 1, 2):
        if n % i == 0:
            return False
    return True

def main():
    with concurrent.futures.ProcessPoolExecutor() as executor:
        for number, prime in zip(PRIMES, executor.map(is_prime, PRIMES)):
            print('%d is prime: %s' % (number, prime))

if __name__ == '__main__':
    main()
```

## 模块函数

| 函数                                                         | 作用                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| `concurrent.futures.as_completed(fs, timeout=None)`          | 返回一个包含fs所指定的Future实例的迭代器                     |
| `concurrent.futures.wait(fs, timeout=None, return_when=ALL_COMPLETED)` | 等待 *fs* 指定的 [`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 实例（可能由不同的 [`Executor`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor) 实例创建）完成 |

## Future对象

*class* `concurrent.futures.Future`[文档](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future)

​		将可调用对象封装为异步执行。[`Future`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Future) 实例由 [`Executor.submit()`](https://docs.python.org/zh-cn/3/library/concurrent.futures.html#concurrent.futures.Executor.submit) 创建，除非测试，不应直接创建。

| 方法                      | 作用                                         |
| ------------------------- | -------------------------------------------- |
| `cancel()`                | 尝试取消调用                                 |
| `cancelled()`             | 如果调用成功取消返回`True`                   |
| `running()`               | 如果调用正在执行而且不能被取消那么返回`True` |
| `done()`                  | 如果调用已被取消或正常结束那么返回 `True`    |
| `result(timeout=None)`    | 返回调用返回得值，可以指定返回超时的值       |
| `exception(timeout=None)` | 返回由调用引发的异常                         |
| `add_done_callback(fn)`   | 附加可调用`fn`到`future`对象                 |