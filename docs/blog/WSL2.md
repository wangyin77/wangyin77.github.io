# WSL2 踩坑全记录

## 1.安装WSL

进入insider 空白要开信息采集，开启灰色需要进计算机管理给权限（还挺麻烦的，网上有教程,例如[这个](https://blog.csdn.net/weixin_41542958/article/details/107577542)）

需要系统版本BUILD > 20145吧，大概是这个数，据说202xx的也有问题，就选最新的dev通道吧。

Store里装Ubuntu 18.04

进去之后创建用户名密码

创建root密码

```bash
sudo passwd root
```

更改软件源

```bash
sudo nano /etc/apt/sources.list
```

`Alt + T` 全选剪切

注意：需要复制多行文本时需要用Windows Terminal打开！Windows Terminal通过Store安装。

添加[清华源](https://mirror.tuna.tsinghua.edu.cn/help/ubuntu/)，注意版本18.04

```bash
# 默认注释了源码镜像以提高 apt update 速度，如有需要可自行取消注释
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-updates main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-backports main restricted universe multiverse
deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-security main restricted universe multiverse

# 预发布软件源，不建议启用
# deb https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
# deb-src https://mirrors.tuna.tsinghua.edu.cn/ubuntu/ bionic-proposed main restricted universe multiverse
```

`Ctrl + X` 保存退出

更新包，系统初始状态是空包，一定要更新

```bash
sudo apt update && sudo apt update -y
sudo apt -y install dist-upgrade
# 清理缓存
sudo apt -y clean && sudo apt -y autoclean && sudo apt -y autoremove
```

连不上网，惊不惊喜。因为WSL2目前版本吧有很多BUG，连不上网的问题还没修好，只能手动解决，具体方式如下：

首先删除 /etc/resolv.conf，这是个软连接，直接改重启就没了

```bash
sudo rm /etc/resolv.conf
```

然后再生成一个 `/etc/resolv.conf`

```bash
sudo touch /etc/resolv.conf
```

然后在这个文件里写入自定义的DNS服务器，因为默认的不好使

```bash
sudo nano /etc/resolv.conf
```

写入内容,可以用别的DNS：

```
nameserver 114.114.114.114
```

再创建一个/etc/wsl.conf，阻止启动时创建`/etc/resolv.conf`

```bash
sudo touch /etc/wsl.conf
sudo nano /etc/wsl.conf
```

写入内容：

```
[network] 
generateResolvConf = false 
```

继续更新包

```bash
sudo apt update && sudo apt update -y
# 清理缓存
sudo apt -y clean && sudo apt -y autoclean && sudo apt -y autoremove
```

## 2. 安装Miniconda 和 Jupyter-lab

接着安装一下conda，这里先装个Miniconda吧，首先下载

```bash
wget -c https://mirrors.tuna.tsinghua.edu.cn/anaconda/miniconda/Miniconda3-latest-Linux-x86_64.sh
```

然后最好给个权限，不然可能安装的会有点问题，777就是啥权限都有

```bash
sudo chmod 777 ~/Miniconda3-latest-Linux-x86_64.sh
```

然后开始安装，最好还是给个sudo权限

```bash
sudo bash Miniconda3-latest-Linux-x86_64.sh 
```

很多人说吧，不要自动初始化，但是咱全新的安装和别的软件也冲突不了，就直接第三步选yes了

接着装个JupyterLab

```bash
conda install -c conda-forge jupyterlab
```

然后发现，

```
conda: command not found
```

可能需要重启下，重启就是把终端关了再打开，很简单，此时能看到`（base）`了

还需要添加一下安装源

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/conda-forge
```

此时输入命令，还是装不了

```
Collecting package metadata (current_repodata.json): failed
```

这时候应该是权限出了问题，得给`miniconda3`还有`.conda`这个文件夹权限,`wy`这里填自己的用户名

```bash
sudo chown -R wy miniconda3/
sudo chown -R wy ~/.conda
```

好了这就开始安装了，继续执行

```bash
conda install -c conda-forge jupyterlab
```

下的还挺慢，感觉channel不是太好

然后打开jupyter-lab，直接打

```bash
jupyter lab
```

此时吧，会给一个地址，同时他会试图打开一个浏览器，这时候就乱码了，可能是我的win是英文版的问题。按`Ctrl + C`是能看到地址的，反正就不能自动打开windows 里的浏览器。

于是，设置一下。`Ctrl + Z`结束`jupyter-lab`进程

首先，生成默认配置文件

```bash
jupyter notebook --generate-config
```

修改这个文件

```bash
nano ~/.jupyter/jupyter_notebook_config.py
```

`Ctrl + W` 搜索 `c.NotebookApp.use_redirect_file`这个配置选项，把它改成，并取消注释

`c.NotebookApp.use_redirect_file = False`

之后修改`~/.bashrc`

```bash
nano ~/.bashrc
```

添加浏览器指向，例如`"C:\Program Files\Google\Chrome\Application\chrome.exe"`写成`'/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'`

```bash
export BROWSER='/mnt/c/Program Files/Google/Chrome/Application/chrome.exe'
```

刷新bash

```bash
source ~/.bashrc
```

此时用`jupyter lab`命令就能直接打开windows内部的浏览器了

## 3.安装CUDA

接下来装一下CUDA

首先要有Nvidia的显卡，然后在windows里装上最新的驱动（得是最新的，目前还是预览版，需要注册Nvidia账户，在这里下载）

然后进入WSL2，下载WSL-Ubuntu的CUDA，可以在这个网站找到

```bash
wget https://developer.download.nvidia.com/compute/cuda/11.1.0/local_installers/cuda_11.1.0_455.23.05_linux.run
sudo sh cuda_11.1.0_455.23.05_linux.run
```

这一步可以先搭个梯子把那个run文件下下来挺大的3GB+，然后拷进去，下次要是重装就不用重复下了。

拷进去之后安装前先给个权限，这里我也拷在~下了

在这之前需要安装几个需要的软件（系统里自带的啥也没有。。）

```bash
sudo apt-get install -y gcc g++ make
sudo chmod 777 ~/cuda_11.1.0_455.23.05_linux.run
sudo sh cuda_11.1.0_455.23.05_linux.run
```

需要等待一段时间，可能比较卡，此时内存占用巨大，基本用满了（14.3GB/15.9GB），可能在解压吧。出错的话查看对应的log文件。

进入之后输入`accept`，然后选择安装即可，注意如果出现了driver的选项，需要把driver的`×`去掉。

```
===========
= Summary =
===========

Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-11.1/
Samples:  Installed in /home/wy/, but missing recommended libraries

Please make sure that
 -   PATH includes /usr/local/cuda-11.1/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-11.1/lib64, or, add /usr/local/cuda-11.1/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.1/bin
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least .00 is required for CUDA 11.1 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver

Logfile is /var/log/cuda-installer.log===========
= Summary =
===========

Driver:   Not Selected
Toolkit:  Installed in /usr/local/cuda-11.1/
Samples:  Installed in /home/wy/, but missing recommended libraries

Please make sure that
 -   PATH includes /usr/local/cuda-11.1/bin
 -   LD_LIBRARY_PATH includes /usr/local/cuda-11.1/lib64, or, add /usr/local/cuda-11.1/lib64 to /etc/ld.so.conf and run ldconfig as root

To uninstall the CUDA Toolkit, run cuda-uninstaller in /usr/local/cuda-11.1/bin
***WARNING: Incomplete installation! This installation did not install the CUDA Driver. A driver of version at least .00 is required for CUDA 11.1 functionality to work.
To install the driver using this installer, run the following command, replacing <CudaInstaller> with the name of this run file:
    sudo <CudaInstaller>.run --silent --driver

Logfile is /var/log/cuda-installer.log
```

可以看到一个Summary，他说他装完了，但是环境需要你配。具体来说呢，可以参考[官方文档](https://docs.nvidia.com/cuda/cuda-installation-guide-linux/index.html#post-installation-actions)

这里版本号，我装的是11.1的，需要修改一下

环境变量可以添加在很多地方，这里加在~/.bashrc里

```bash
nano ~/.bashrc
```

在最后加如下：

```bash
export PATH=/usr/local/cuda-11.1/bin${PATH:+:${PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-11.2/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
```

刷新一下bash

```bash
source ~/.bashrc
```

运行一下

```bash
nvcc --version
```

可以看到CUDA装好了

还可以进一步验证，执行如下操作

```bash
cd ./NVIDIA_CUDA-11.1_Samples/1_Utilities/deviceQuery
make
./deviceQuery
```

很尴尬，这里报错了代码35，表示没找到GPU，可能跟我用的P106有关系。