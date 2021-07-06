---
[官方文档]（https://docs.python.org/3/howto/logging.html#logging-basic-tutorial）
---

[TOC]

[TOC]

## 快速使用

* 日志划分等级，从低到高。

  [`debug()`](https://docs.python.org/3/library/logging.html#logging.debug)，[`info()`](https://docs.python.org/3/library/logging.html#logging.info)，[`warning()`](https://docs.python.org/3/library/logging.html#logging.warning)，[`error()`](https://docs.python.org/3/library/logging.html#logging.error)和 [`critical()`](https://docs.python.org/3/library/logging.html#logging.critical)

| 级别     | 说明                                             |
| -------- | ------------------------------------------------ |
| DEBUG    | 详细信息，通常只有在诊断问题时才感兴趣           |
| INFO     | 确认事情正如预期的运作                           |
| WARNING  | 意外时间发生的迹象，能正常运行（如磁盘空间不足） |
| ERROR    | 由于一个严重的问题，无法执行一些功能             |
| CRITICAL | 严重错误，表示程序本身可能无法继续运行           |

**默认级别为 WARNING**

* 简单例子

  ```python
  >>> import logging
  >>> logging.waring('MrSnake')
  >>> WARNING:root:MrSnake
  >>> logging.info('not print')	# 由于默认级别为WARNING，所有级别比它低不会被打印
  >>>
  ```

* 将日志写入文件

  ```python
  >>> logging.basicConfig(filename='snake.txt', encoding='utf-8', level=logging.DEBUG)
  # 也可以指定filemode参数
  >>> logging.debug("write to file")
  >>> logging.info('too')
  >>> logging.warning('too')
  # 级别设置为DEBUG，最低级别，都会被写入文件。
  
  /snake.txt
  DEBUG:root:write to file
  INFO:root:too
  WARNING:root:too
  ```

* 格式化日志输出格式 `format参数`

  ```
  >>> logging.basicConfig(format='%(asctime)s %(message)s', datefmt='%m/%d/%Y %I:%M:%S %p')
  >>> logging.debug("l'm snake")
  01/06/2021 02:21:46 PM l'm snake
  ```

## 进阶使用

日志库采用模块化方法，并提供了几类组件: 日志记录程序、处理程序、过滤器和格式化程序。

- Loggers expose the interface that application code directly uses.

  记录器公开应用程序代码直接使用的接口。

- Handlers send the log records (created by loggers) to the appropriate destination.

  处理程序将日志记录(由记录程序创建)发送到相应的目标。

- Filters provide a finer grained facility for determining which log records to output.

  过滤器为确定输出哪些日志记录提供了更细粒度的工具。

- Formatters specify the layout of log records in the final output.

  格式化程序在最终输出中指定日志记录的布局。

> 命名日志记录器，这是一个很好的约定
>
> `logger = logging.getLogger("snake")`
>
> 日志记录器名称会跟踪包/模块层次结构，并且直观地显示仅从日志记录器名称记录的事件位置。
>
> basicConfig()为消息设置的默认格式是：`severity:logger name:message`

### Loggers 对象

​	它有三个作用: 向应用程序公开几个方法、日志记录器对象根据级别或过滤器确定执行消息、记录器对象将相关的日志消息传递给日志处理程序。

#### 1.配置日志记录对象

首先命名 `logger = logging.getLogger("snake")`

**常用方法**

| 方法                      | 功能                         |
| ------------------------- | ---------------------------- |
| `logger.setLevel()`       | 设置日志记录器的级别         |
| `logger.addHandler()`     | 从logger对象中添加程序对象   |
| `logger.removerHandler()` | 从logger对象中删除程序对象   |
| `logger.addFilter()`      | 从logger对象中添加过滤器对象 |
| `logger.removeFilter()`   | 从logger对象中删除过滤器对象 |

#### 2.配置完日志记录器后，就可以创建日志信息

```python
logger.debug(), logginer.info(), logger.warning(), logger.error(), logger.critical()
# 都可一日志信息
logger.exception()	# 与logger.error()类似，不同之处在于会转储堆栈跟踪。仅从异常处理程序调用此方法。
logger.log()	# 更详细
```

### Handlers 处理程序

​	Handler 对象负责将适当的日志消息(基于日志消息的严重性)发送到处理程序指定的目的地。Logger 对象可以使用 addHandler ()方法向自己添加零个或多个处理程序对象。

**常用方法**

| 方法             | 功能                     |
| ---------------- | ------------------------ |
| `setLevel()`     | 设置处理程序的级别       |
| `setFormatter()` | 为处理程序格式化字符串   |
| `addFilter()`    | 为处理程序添加过滤器对象 |
| `removeFilter()` | 为处理程序删除过滤器对象 |

> `'%(asctime)s - %(levelname)s - %(message)s'`	# 浅显易懂的格式化显示

#### 示例

```python
logger = logging.getLogger('MrSnake')
logger.setLevel(logging.DEBUG)		# 创建日志记录器对象，并设置级别

handler = logging.StreamHandler()	# 创建处理器的一种适当格式类
handler.setLevel(logging.DEBUG)	# 设置处理器的级别

formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')
handler.setFormatter(formatter)	# 格式化显示

logger.addHandler(ch)	# 添加到记录器中

# 运行
logger.debug('MrSnake')
-----------------------------------------------------------------------------------------------
2021-01-06 14:38:37,832 - MrSnake - DEBUG - MrSnake
```

## 一些处理程序

### 1.StreamHandler

将信息发送到流（类似文件的对象）

### 2.FileHandler

> 参数 filename, mode='a', encoding=None

将信息发送到磁盘文件

### 3.RotatingFileHandler

将信息发送到磁盘文件，并支持设置日志文件大小

>  参数 `maxBytes,backupCount` 

当超过 `maxBytes` 会被关闭，并且生且一个后缀名 ".1" 新日志文件，生成最多新日志文件数取决于 `backupCount`

**`maxBytes` 为非零，`backupCount` 至少为1**

### 4.TImedRotatingFileHandler

> 参数 when,interval,backupCount

> > when 	 'S'，'M',  'H',  'D'
> >
> > interval	传递值
> >
> > 示例 when = 'S', interval='10' 则10s 生成一个新的日志文件

消息发送到磁盘文件，支持设置间隔，生成备份

## 有用的日志记录示例

### 从多个线程记录

```python
import logging
import threading
import time


def worker(arg):
    while not arg['stop']:
        logging.debug('Hi mr snake')
        time.sleep(0.5)


def main():
    logging.basicConfig(level=logging.DEBUG, format='%(relativeCreated)6d %(threadName)s %(message)s')
    info = {'stop': False}
    thread = threading.Thread(target=worker, args=(info,))
    thread.start()
    while True:
        try:
            logging.debug('Hi mr snake mainthread')
            time.sleep(0.75)
        except KeyboardInterrupt:
            info['stop'] = True
            break
    thread.join()

if __name__ == '__main__':
    main()
-----------------------------------------------------------------------------------------------
   284 Thread-1 Hi mr snake
   284 MainThread Hi mr snake mainthread
   784 Thread-1 Hi mr snake
  1034 MainThread Hi mr snake mainthread
  1285 Thread-1 Hi mr snake
  1784 MainThread Hi mr snake mainthread
  1786 Thread-1 Hi mr snake
  2286 Thread-1 Hi mr snake
  2535 MainThread Hi mr snake mainthread

```

## 常用格式化属性

| 格式                  | 描述                                                      |
| --------------------- | --------------------------------------------------------- |
| `%(asctime)s`         | 格式为时间，默认“ 2021-01-08 12:05:45,896”,逗号后面为毫秒 |
| `%(levelname)s`       | 日志记录级别                                              |
| `%(message)s`         | 日志消息                                                  |
| `%(name)s`            | 记录器定义的名称，默认为root                              |
| `%(relativeCreated)d` | 加载日志模块的时间                                        |
| `%(process)d`         | 进程id                                                    |
| `%(processName)s`     | 进程名                                                    |
| `%(thread)d`          | 线程id                                                    |
| `%(threadName)s`      | 线程名                                                    |

