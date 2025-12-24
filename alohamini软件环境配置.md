2025/12/17 

alohamini软件环境配置

环境：树莓派5，Ubuntu24.04

git出处 [liyiteng/lerobot_alohamini: Software support for AlohaMini robot.](https://github.com/liyiteng/lerobot_alohamini)

1. 安装ubuntu（[（树莓派Raspberry Pi 5系列文章）一、安装ubuntu24.04操作系统_树莓派5安装ubuntu-CSDN博客](https://blog.csdn.net/guojingyue123/article/details/135914906)）：

   安装前准备：sd卡（16G以上），读卡器，屏幕和micro-HDMImini线

   将sd卡插入读卡器，再将读卡器接入Windows电脑上。从树莓派官网下载镜像：ubuntu-24.04.3-preinstalled-desktop-arm64+raspi.img.xz。该镜像为桌面版，适配树莓派5。

   此外，下载并安装树莓派官方烧录软件（[Raspberry Pi software – Raspberry Pi](https://www.raspberrypi.com/software/)），在镜像下载完成后打开烧录软件并选择读卡器和镜像，之后进行镜像的下载和校验。检验完成后将sd卡拔出并插进树莓派。成功的话可以看到树莓派的显示屏上出现Ubuntu安装界面。按需要选择即可安装成功。

2. ssh服务设置

   在树莓派上打开终端，安装ssh服务。指令：`sudo apt-get install openssh-server`

   安装好ssh服务端后，检查树莓派和PC主机是否在同一网段。确认之后进行ssh连接,格式为：ssh 用户名@需要连接的设备的IP 

   如：`ssh alohamini@192.168.0.123`

   为了更方便的开发，可以选择在vscode进行连接。需要修改本地config：

   ![image-20251217152914950](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251217152914950.png)

   添加：

   ![image-20251217152933096](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251217152933096.png)

   之后就可以直接在vscode中连接ssh：

   ![image-20251217152807984](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251217152807984.png)

3. 换源（可选）

   打开sources.list：`sudo nano /etc/apt/sources.list`

   将阿里云镜像复制进文件：

   ```
   `deb https://mirrors.aliyun.com/ubuntu-ports/ noble main restricted universe multiverse`
   `deb-src https://mirrors.aliyun.com/ubuntu-ports/ noble main restricted universe multiverse`
   
   `deb https://mirrors.aliyun.com/ubuntu-ports/ noble-security main restricted universe multiverse`
   `deb-src https://mirrors.aliyun.com/ubuntu-ports/ noble-security main restricted universe multiverse`
   
   `deb https://mirrors.aliyun.com/ubuntu-ports/ noble-updates main restricted universe multiverse`
   `deb-src https://mirrors.aliyun.com/ubuntu-ports/ noble-updates main restricted universe multiverse`
   
   `deb https://mirrors.aliyun.com/ubuntu-ports/ noble-backports main restricted universe multiverse`
   `deb-src https://mirrors.aliyun.com/ubuntu-ports/ noble-backports main restricted universe multiverse`
   ```

   更新：

   ```
   sudo apt-get update
   sudo apt-get upgrade
   ```

   

4. 安装配置alohamini环境（[liyiteng/lerobot_alohamini: Software support for AlohaMini robot.](https://github.com/liyiteng/lerobot_alohamini)）

   测试网络：

   ```
   curl https://www.google.com
   curl https://huggingface.co
   ```

   clone项目：

   ```
   cd ~
   git clone https://github.com/liyiteng/lerobot_alohamini.git
   ```

   安装conda环境：相较于git的教程，修改了miniconda的下载方式。这是因为树莓派为arm架构，需要下载适配树莓派的miniconda，而原版教程使用了x86架构。具体指令如下：

   ````
   ```
   `mkdir -p ~/miniconda3`
   ```
   ```
   wget -4 https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-aarch64.sh -O ~/miniconda3/miniconda.sh
   ```
   `bash ~/miniconda3/miniconda.sh -b -u -p ~/miniconda3`
   
   `rm ~/miniconda3/miniconda.sh`
   
   `~/miniconda3/bin/conda init bash`
   
   `source ~/.bashrc`
   ````

   创建conda虚拟环境并初始化conda：

   ```
   conda create -y -n lerobot_alohamini python=3.10
   conda activate lerobot_alohamini
   ```

   安装环境依赖：

   注意在安装依赖之前，需要检查树莓派是否安装过编译工具如gcc cmake等。如果没有的话，在安装依赖并编译的阶段会出现报错，因此建议提前安装好：

   ```
   sudo apt update
   sudo apt install build-essential
   sudo apt install cmake
   ```

   安装好后，在实际操作时还遇到了下载速度过慢导致的一系列依赖下载超时问题。可以通过科学上网等方式解决。但是实际上通过对比发现大概率是因为树莓派过热导致的降频，使得CPU解压速度变慢等问题。最终给树莓派降温就解决了。

   检查好一切之后，可以开始安装依赖：

   ```
   cd ~/lerobot_alohamini
   pip install -e .[all]
   conda install ffmpeg=7.1.1 -c conda-forge
   ```

   ![image-20251217154635565](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251217154635565.png)

   ![image-20251217155017084](C:\Users\PC\AppData\Roaming\Typora\typora-user-images\image-20251217155017084.png)

   出现上面的结果，说明依赖全部安装完成。

参考：[1-Lerobot_AlohaMini环境配置_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1GQreYhEy1/?spm_id_from=333.1387.collection.video_card.click&vd_source=5fab9a6ba6876aa1bc0215ac7d8cab21)