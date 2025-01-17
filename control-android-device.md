#### 控制触摸屏失效的Android设备

一台Android版本很老的设备，它的触摸屏已经不能使用了，在大学时期学习Android开发时使用它作为物理机调试，课程上完之后就没有使用它了，一直存放在箱子里，时间久了，触摸屏里面的化学物质变质，金属线被锈蚀导致触摸没有反馈。现在我想尝试使用鼠标通过蓝牙控制HUAWEI C8812E设备，以前尝试通过shell命令去开启蓝牙，但是没有成功。

在学习Android的过程中，有时候需要修改文件访问权限，因此这台设备很早就被我获取了root权限，我可以通过电脑的控制台键入shell命令去访问这台设备。我寻找了很久开启bluetooth的命令，但是这台设备太老了，命令似乎不太一样，我又寻找关于蓝牙的配置文件，试图通过修改配置文件开启蓝牙。经过一个多小时的尝试仍然没有结果。无意间搜索到模拟输入的命令，这好像也是一个突破口。于是我按照这个思路去尝试启动`Settings` 系统软件，并通过模拟输入成功开启了蓝牙。我兴奋地使用电脑连接设备蓝牙，但是提示设备不支持鼠标键盘通过蓝牙控制。既然它可以通过shell命令去控制，那也可以通过物理外设去控制Android设备。我通过使用python运行了一个发送shell命令的脚本，发现这是可行的方法。接下来我拿出了Raspberry Pi Pico去使用按键发送shell命令给手机，观察它是否真的可行，由于我是使用的是MicroPython，它是一个精简过的python库，常常运行在嵌入式设备中，所以有很多功能不能够使用的，尤其是Raspberry Pi Pico是MCU，它没有操作系统，所以os模块中一些函数在设备中无法执行。 :-)



<div><center><img src="control-android-device/shell.jpg" style="zoom:80%;"></center></div><br>



<details>
<summary>备忘录:查看应用程序包名</summary>

```shell
adb shell pm list package
```

</details>



<details>
<summary>备忘录:启动应用程序</summary>

```shell
adb shell am start PACKAGE NAME
```

</details>



<details>
    <summary>备忘录:模拟输入</summary>

```shell
adb shell input keyevent 19
```

</details>


<details>
    <summary>备忘录:发送控制命令脚本</summary>

```python
import os
os.popen("adb shell input keyevent 20")
```

</details>

<br>

| KEY NAME | KEY VALUE |
| -------- | --------- |
| 方向上键 | 19        |
| 方向下键 | 20        |
| 方向左键 | 21        |
| 方向右键 | 22        |
| 确定键   | 66        |
| 返回键   | 4         |
| Home键   | 3         |
| Menu键   | 82        |
| 导航键   | 23        |

> 碎屏手机开启USB共享网络（连接WIFI, 共享给电脑）

<div><center><img src="control-android-device/usb network connection.jpg" style="zoom:80%;"></center></div><br>