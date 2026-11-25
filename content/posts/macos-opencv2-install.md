---
title: "Macbook OS X 11.6 安装 opencv2 记录"
date: 2017-12-05
categories: [技术]
tags: [OpenCV, macOS, 编译]
draft: false
---

安装有两种方法：
**一、** 通过brew安装。
**二、** 官网下载源码安装。

**一、** 通过brew安装
```
brew install opencv是安装opencv2
brew install opencv3是安装opencv3
```
安装过程中会自动安装很多可能需要的依赖包。
安装完成后安装路径为/usr/local/Cellar/opencv/2.4.X.X/。

**二、** 官网下载源码安装（需要先通过brew安装cmake）
**1、** 进入官网https://opencv.org/releases.html，下载页面中的source文件到本地，如放在Document文件夹中。

![配图1](/images/p6_1.png)

**2、** 对下载压缩包进行解压，打开终端，cd到Document中的oepncv-2.4.13.4文件夹下。进行如下操作
```
mkdir release （新建release文件夹）
cd ./release
cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local -D BUILD_PYTHON_SUPPORT=ON -D BUILD_EXAMPLES=ON ..
```
(或者cmake -D CMAKE_BUILD_TYPE=RELEASE -D CMAKE_INSTALL_PREFIX=/usr/local ..）【推荐后者】
然后输入make开始编译。
最后输入sudo make install进行安装。
至此。openCV在mac上算是安装完毕。

**3、** Xcode的配置
打开Xcode，新建一个command line工程。语言选择c++。（command line在create a new Xcode project中OS X中Application中）

![配图2](/images/p6_2.png)

在左边选中新建的工程，右边选中Target（上图中右边白色窗口的左上角，没有红色框出，在下图中用红色框已框出），
然后选择build setting。在search paths中的Header Search Paths和Library Search Paths中添加相关路径（/usr/local/include和/usr/local/lib），如上图所示。

![配图3](/images/p6_3.png)

然后选择build Phases。在Link Binary With Libraries中添加动态库文件，动态库文件存在的地方是：/Users/zhangjianlian/Documents/opencv-2.4.13.4/release/lib。

**4、** 其他
在终端中打开mac自带的python（2.7.10），输入import cv，报错。错误如下：
```
Traceback (most recent call last):
File "<stdin>", line 1, in <module>
File "/usr/local/lib/python2.7/site-packages/cv.py", line 1, in <module>
from cv2.cv import *
ImportError: dlopen(/usr/local/lib/python2.7/site-packages/cv2.so, 2): Library not loaded: libopencv_contrib.2.4.dylib
Referenced from: /usr/local/lib/python2.7/site-packages/cv2.so
Reason: unsafe use of relative rpath libopencv_contrib.2.4.dylib in /usr/local/lib/python2.7/site-packages/cv2.so with restricted binary
```
解决方案：
通过以下命令把位于下载的opencv文件夹路径下的cv2.so link 到所使用的python的site-package文件夹路径下。
（可能还需要把/Users/zhangjianlian/Documents/opencv-2.4.13.4/modules/python/src2/cv.py同样 link 到相同路径下写成命令的形式
```
ln -s -f  /Users/zhangjianlian/Documents/opencv-2.4.13.4/modules/python/src2/cv.py  /usr/local/lib/python2.7/site-packages/cv.py）
ln -s -f  /Users/zhangjianlian/Documents/opencv-2.4.13.4/release/lib/cv2.so   /usr/local/lib/python2.7/site-packages/cv2.so
```

**5、** 其他2
在解决4中的错误时，还做过一项操作，做完后4中的错误依然存在，不知道该操作是否有用。
cd到根目录后在.bash_profile和.profile中添加了和python相关的路径。（具体记不清楚了 再找找。。。）
——————————————————————
5的补充：
在cd到根目录后，通过open .bash_profile打开.bash_profile文件

![配图4](/images/p6_4.png)

以及通过open .profile打开.profile文件

![配图5](/images/p6_5.png)

将上述红色方框内语句删除（即之前解决4中错误时添加的相关操作）
否则会造成mac terminal中无法打开python3，只能使用python2。
编辑完成上述文件之后需要重新source一下。
source完之后重启电脑才能生效。
——————————————————————

Reference
安装相关：
[http://blog.csdn.net/yanzi1225627/article/details/42113661](http://blog.csdn.net/yanzi1225627/article/details/42113661)
[http://blog.csdn.net/mai00dou/article/details/48102413](http://blog.csdn.net/mai00dou/article/details/48102413)
[http://bbs.feng.com/read-htm-tid-5583579.html](http://bbs.feng.com/read-htm-tid-5583579.html)
解决unsafe use...with restricted binary问题：
[How to install python opencv on mac OS 10.11](https://stackoverflow.com/questions/32942571/how-to-install-python-opencv-on-mac-os-10-11)（通过该方案解决的问题）
[python opencv error import cv2 importerror](https://stackoverflow.com/questions/33260490/python-opencv-error-import-cv2-importerror-dlopen-after-update-of-os-x-el-capit)
[segmentfault 相关问题](https://segmentfault.com/q/1010000005751879)
