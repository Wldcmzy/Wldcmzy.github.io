---
title: python logging库 简易自学记录
date: 2022-08-26 12:26:59
categories:
  - [基础知识, python, logging]
tags:
  -	日志
---





## 设置日志等级并在控制台打印日志

```python
import random
import logging

lst = [logging.DEBUG, logging.INFO, logging.WARNING, logging.ERROR, logging.CRITICAL]
# 设置日志输出等级
logging.basicConfig(level=random.choice(lst))

logging.debug('debug')
logging.info('info')
logging.warning('waring')
logging.error('error')
logging.critical('critical')

```



## 设置日志参数

在文件打印日志，带固定格式

```python

import logging

logging.basicConfig(
    filename= 'mylog.log',
    level= logging.DEBUG,
    format = '%(asctime)s <%(name)s> %(levelname)s: %(message)s',
    filemode= 'a',
)

logging.debug('debug')
logging.info('info')
logging.warning('waring')
logging.error('error')
logging.critical('critical')


```

参数

```python
"""
    Do basic configuration for the logging system.

    This function does nothing if the root logger already has handlers
    configured, unless the keyword argument *force* is set to ``True``.
    It is a convenience method intended for use by simple scripts
    to do one-shot configuration of the logging package.

    The default behaviour is to create a StreamHandler which writes to
    sys.stderr, set a formatter using the BASIC_FORMAT format string, and
    add the handler to the root logger.

    A number of optional keyword arguments may be specified, which can alter
    the default behaviour.

    filename  Specifies that a FileHandler be created, using the specified
              filename, rather than a StreamHandler.
    filemode  Specifies the mode to open the file, if filename is specified
              (if filemode is unspecified, it defaults to 'a').
    format    Use the specified format string for the handler.
    datefmt   Use the specified date/time format.
    style     If a format string is specified, use this to specify the
              type of format string (possible values '%', '{', '$', for
              %-formatting, :meth:`str.format` and :class:`string.Template`
              - defaults to '%').
    level     Set the root logger level to the specified level.
    stream    Use the specified stream to initialize the StreamHandler. Note
              that this argument is incompatible with 'filename' - if both
              are present, 'stream' is ignored.
    handlers  If specified, this should be an iterable of already created
              handlers, which will be added to the root handler. Any handler
              in the list which does not have a formatter assigned will be
              assigned the formatter created in this function.
    force     If this keyword  is specified as true, any existing handlers
              attached to the root logger are removed and closed, before
              carrying out the configuration as specified by the other
              arguments.
    encoding  If specified together with a filename, this encoding is passed to
              the created FileHandler, causing it to be used when the file is
              opened.
    errors    If specified together with a filename, this value is passed to the
              created FileHandler, causing it to be used when the file is
              opened in text mode. If not specified, the default value is
              `backslashreplace`.

    Note that you could specify a stream created using open(filename, mode)
    rather than passing the filename and mode in. However, it should be
    remembered that StreamHandler does not close its stream (since it may be
    using sys.stdout or sys.stderr), whereas FileHandler closes its stream
    when the handler is closed.

    .. versionchanged:: 3.2
       Added the ``style`` parameter.

    .. versionchanged:: 3.3
       Added the ``handlers`` parameter. A ``ValueError`` is now thrown for
       incompatible arguments (e.g. ``handlers`` specified together with
       ``filename``/``filemode``, or ``filename``/``filemode`` specified
       together with ``stream``, or ``handlers`` specified together with
       ``stream``.

    .. versionchanged:: 3.8
       Added the ``force`` parameter.

    .. versionchanged:: 3.9
       Added the ``encoding`` and ``errors`` parameters.
    """
```

## 一些组件的使用

```python
import logging

handlerF = logging.FileHandler('mylog2.log') # 打印到文件的Handler
handlerC = logging.StreamHandler()    # 打印到控制台的Handler
handlerF.setLevel(logging.ERROR) # 设置Handler的日志等级
handlerC.setLevel(logging.DEBUG)

formatter = logging.Formatter('%(asctime)s <%(name)s> %(levelname)s: %(message)s')
handlerF.setFormatter(formatter) # 设置输出格式
handlerC.setFormatter(formatter)

logger = logging.getLogger('MyLoggerName')
logger.setLevel(logging.WARNING)  # 设置logger的日志等级
# 即使handlerC日志等级为DEBUG, 但logger最多输出WARNING等级日志

logger.addHandler(handlerF)     # 装载Handler
logger.addHandler(handlerC)


logger.debug('debug')
logger.info('info')
logger.warning('waring')
logger.error('error')
logger.critical('critical')

```

##  使用配置文件

不想学了