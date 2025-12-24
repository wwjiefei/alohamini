2025/12/19 

alohamini 机械臂安装

### 1.配置机械臂端口号

将微雪开发板接电源并用USB连接主机，通过下列命令找到端口：

```
cd ~/lerobot_alohamini
ls /dev/ttyACM*
```

找到之后对端口进行授权：可以用chmod的方法添加授权，但是舵机很多太麻烦，可以直接把用户加入设备用户组来快速实现。

```
(base) alohamini@alohamini:~/lerobot_alohamini$ whoami
alohamini
(base) alohamini@alohamini:~/lerobot_alohamini$ sudo usermod -a -G dialout alohamini
```

之后重启电脑。

### 2.设置sts3215电机ID

给电机设置ID：电机在出厂时默认为1号，但是总线上不能同时出现2个1号，所以要提前设置，否则会出现异常。

把电机连接到微雪开发板上。

使用alohamini的点击状态查询脚本查看电机：`python examples/debug/motors.py get_motors_states --port /dev/ttyACM0`

![image-20251219152113343](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219152113343.png)

一般正常来讲，需要让电机的初始态位于2048（图中的POS），因此需要修复。这一步使用脚本将电机进行旋转：

`python examples/debug/motors.py move_motor_to_position --id 1 --position 2048 --port /dev/ttyACM0`

![image-20251219152545506](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219152545506.png)

再次查看：

![image-20251219152910528](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219152910528.png)

虽然指定2048的位置，但是有一些误差也是正常，下面修改电机的ID：

`python examples/debug/motors.py configure_motor_id --id 1 --set_id 6 --port /dev/ttyACM0`

![image-20251219153153270](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219153153270.png)

然后串联下一个电机，查看一下串口，显示两个电机：

![image-20251219153549601](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219153549601.png)

重复修改ID的流程，把新的电机ID改成5：

![image-20251219153720965](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219153720965.png)

串联电机图：

<img src="F:\QQ聊天消息\Tencent Files\1669278467\nt_qq\nt_data\Pic\2025-12\Thumb\6510618d932acc57375c60d5991e7ec2_720.jpg" alt="6510618d932acc57375c60d5991e7ec2_720" style="zoom: 25%;" />

之后重复上述过程，把6个电机全部配置好ID。

![image-20251219154603528](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219154603528.png)

![image-20251219154617528](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219154617528.png)

### 3.组装机械臂

把配置好的电机和3D打印件组装起来。

视频教程：[3-SO100-Follower臂组装教程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1nQcFewEUC?spm_id_from=333.788.player.switch&vd_source=5fab9a6ba6876aa1bc0215ac7d8cab21)

打开 python examples/debug/motors.py get_motors_states --port /dev/ttyACM0，移动机械臂，可以看到数值变动：

![image-20251219181423363](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251219181423363.png)