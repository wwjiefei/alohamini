2025/12/18

alohamini数据采集与训练的准备工作

### 1.需要做的准备

​	树莓派 + AlohaMini环境

​	PC + GPU（可选租服务器或本地PC训练）

​	Hugging Face CLI账号（用于保存数据集和模型）

### 2.大致流程

 1. 树莓派端（机器人端）

    需要安装好Ubuntu和alohamini的环境，能够正常访问Hugging Face

    需要安装并控制机械臂、相机驱动

    功能：数据采集（相机 + 机械臂状态 + 动作命令）

    ​	    在线推理（加载训练好的模型执行控制）

    ​	    低层控制（串口 / GPIO / I2C）

 2. 配置机械臂和相机

    配置机械臂端口编号、配置摄像机端口号、远程作校准与测试

    [liyiteng/lerobot_alohamini：AlohaMini 机器人的软件支持](https://github.com/liyiteng/lerobot_alohamini) 上面有具体的操作和调试流程

 3. 采集数据

     在 HuggingFace 上获取并配置密钥，之后运行脚本，在git上有操作流程

    也可以回放数据集、数据可视化

 4. 本地培训或远程培训

​	需要 CUDA + cuDNN 对应显卡驱动

​	开发者给出的训练教程视频：[6-lerobot配置相机与模型训练_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1HXw6e2EMo?spm_id_from=333.788.videopod.sections&vd_source=5fab9a6ba6876aa1bc0215ac7d8cab21)