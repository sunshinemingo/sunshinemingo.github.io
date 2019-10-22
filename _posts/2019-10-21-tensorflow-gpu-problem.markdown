---
layout:     post
title:      "安装tensorflow-gpu：ImportError: DLL load failed:找不到指定的模块(Windows版本) 解决方案"
subtitle:   "避免踩坑"
date:       2019-10-21
author:     "Mingo"
header-img: "img/post-bg-unix-linux.jpg"

---

## 安装tensorflow-gpu：ImportError: DLL load failed:找不到指定的模块(Windows版本) 解决方案
在按照教程安装tensorflow-gpu的过程中，出现ImportError: DLL load failed:找不到指定的模块:
```C
(tensorflow) C:\Users\Administrator>python
Python 3.6.3 |Anaconda, Inc.| (default, Mar  4 2019, 15:10:56) [MSC v.1900 64 bit (AMD64)] on win32
Type "help", "copyright", "credits" or "license" for more information.
>>> import tensorflow
Traceback (most recent call last):
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: DLL load failed: 找不到指定的模块。

During handling of the above exception, another exception occurred:

Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\__init__.py", line 22, in <module>
    from tensorflow.python import pywrap_tensorflow  # pylint: disable=unused-import
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\__init__.py", line 49, in <module>
    from tensorflow.python import pywrap_tensorflow
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 74, in <module>
    raise ImportError(msg)
ImportError: Traceback (most recent call last):
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow.py", line 58, in <module>
    from tensorflow.python.pywrap_tensorflow_internal import *
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 28, in <module>
    _pywrap_tensorflow_internal = swig_import_helper()
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\site-packages\tensorflow\python\pywrap_tensorflow_internal.py", line 24, in swig_import_helper
    _mod = imp.load_module('_pywrap_tensorflow_internal', fp, pathname, description)
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\imp.py", line 243, in load_module
    return load_dynamic(name, filename, file)
  File "D:\ProgramFiles\Anaconda\envs\tensorflow\lib\imp.py", line 343, in load_dynamic
    return _load(spec)
ImportError: DLL load failed: 找不到指定的模块。


Failed to load the native TensorFlow runtime.

See https://www.tensorflow.org/install/install_sources#common_installation_problems

for some common reasons and solutions.  Include the entire stack trace
above this error message when asking for help.
```
搜索网上的解决方法，有博客说道：
1. 错误提示为DLL load failed，说明缺少一些必须的DLL模块，可以尝试安装Microsoft Visual C++ 2015 Redistributable程序。
2. 在anaconda中重新安装tensorflow。
3. 重新配置cuda和cudnn模块。可能是之前用别的深度学习模块时，改变了cuda和cudnn的配置。检查一下电脑中的cuda和cudnn配置，改为cuda8.0和cudnn-8.0-windows10-x64-v6.0版本（或其他和tensorflow配套的版本）。
尝试上述的3种方法也许可以解决tensorflow启动报错：DLL load failed的问题。
虽然通过以上几种方法确实可以解决问题，但是方法并不直接，没有确定缺少DLL的原因。

可以通过类似于下面的方法确定缺少的DLL（通过dumpbin.exe确定依赖的DLL）：
```c
"c:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin\dumpbin.exe" /dependents C:\Users\username\AppData\Local\Programs\Python\Python36\lib\site-packages\tensorflow\python\_pywrap_tensorflow_internal.pyd
```
得到的输出类似于：
KERNEL32.dll WSOCK32.dll WS2_32.dll SHLWAPI.dll nvcuda.dll cublas64_80.dll cufft64_80.dll curand64_80.dll cudnn64_6.dll python36.dll MSVCP140.dll VCRUNTIME140.dll
然后通过搜索文件找出缺少的DLL，谷歌搜索确定DLL来自于哪里，装上对应版本的模块（可能是Microsoft Visual C++ 2015 Redistributable没装、tensorflow版本和cuda版本不一致）。

在遇到这个问题的时候，本人的原因在于cuda版本不对导致缺少DLL，通过DLL找到对应的cuda版本为10.0，最后安装的版本为tensorflow-gpu 1.13.1 + cuda 10.0 + cudnn 7.4.2（原错误版本为cuda 10.1）。

方法来源：<https://github.com/tensorflow/tensorflow/issues/7623>

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_01.png)