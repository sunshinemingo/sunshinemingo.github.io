---
layout:     post
title:      "MATLAB 2016b安装libsvm"
subtitle:   "不要出错哦"
date:       2019-10-21
author:     "Haoyue"
header-img: "img/post-bg-js-module.jpg"

---

## MATLAB 2016b安装libsvm

配置：win10+64位+matlab 2016b+VS2017+libsvm-3.23
之前在MATLAB 2016b上安装了编译器，以为这次安装libsvm只需要按照网上的步骤即可，却没想到遇到了很多问题。
关于MATLAB安装MinGW-w64 C/C++编译器的教程有很多，在这里我就不重复了，就说一说我遇到的问题吧。
在命令行中输入mex -setup的时候，显示如下：

![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_16.png)

此时如果使用make进行编译的话是会报错的（如果报错提示你没有权限访问，那需要以管理员的身份运行MATLAB）。
没有合适的编译器进行编译，那么如何找到合适的编译器呢？在MATLAB 2016之后的版本中，MATLAB安装目录\bin\win64\mexopts目录中有各种编译器的配置文件，MATLAB就是利用这些xml文件和编译器建立关联：
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_02.png)
之后使用mex -setup -v来进行编译器和SDK的搜索，还是只找到了MinGW64 Compiler ©’。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_03.png)
这时候需要自己去找相关编译器的XML文件，将相应文件添加到\bin\win64\mexopts目录下（如果安装条件跟我一样的话，推荐大家去看这篇博客<https://blog.csdn.net/weixin_41923961/article/details/80209047>，可以找到下图中的两个XML文件）
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_04.png)
再次使用mex -setup -v进行编译器和SDK的搜索，
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_05.png)
之后就可以按照提示选择Microsoft Visual C++ 2017 ©进行编译了。
首先将MATLAB的工作目录切换到\MATLAB\R2016b\toolbox\libsvm-3.23\matlab，然后设置路径。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_06.png)
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_07.png)
之后在命令行中输入make进行编译。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_08.png)
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_09.png)
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_10.png)
编译完成后，可以看到新编译出4个文件libsvmread.mexw64，libsvmwrite.mexw64，svmtrain.mexw64，svmpredict.mexw64。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_11.png)
接下来测试是否安装成功。测试代码如下：
```
[heart_scale_label,heart_scale_inst] =libsvmread('heart_scale');
model = svmtrain(heart_scale_label, heart_scale_inst, '-c 1 -g 0.07');
[predict_label, accuracy, dec_values] = svmpredict(heart_scale_label, heart_scale_inst, model);
```
但是出现错误：
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_12.png)
解决方法是：
1.将heart_scale数据放入当前目录下；
2.在设置路径中可以把libsvm-3.23\matlab和libsvm-3.23\windows添加进来，或者置顶
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_13.png)
之后再次进行测试，成功。
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_14.png)
![img](https://github.com/sunshinemingo/sunshinemingo.github.io/raw/master/img/image_md/image_15.png)
至此，libsvm安装成功。