# Autoware 安装流程

### 1 源码安装

1.1 设置软件源

设置中选择阿里云apt源，命令行增加清华ROS源

```bash
sudo sh -c '. /etc/lsb-release && echo "deb http://mirrors.tuna.tsinghua.edu.cn/ros/ubuntu/ $DISTRIB_CODENAME main" > /etc/apt/sources.list.d/ros-latest.list'
```

1.2 设置ROS仓库密钥

```bash
sudo apt-key adv --keyserver keyserver.ubuntu.com --recv-keys F42ED6FBAB17C654
```

1.3 安装依赖

```bash
sudo apt update
sudo apt install -y python-catkin-pkg python-rosdep2 ros-melodic-catkin
sudo apt install -y python3-pip python3-colcon-common-extensions python3-setuptools python3-vcstool
pip3 install -U setuptools
```

注：以上安装的ros不完全，可能导致编译失败，可以额外安装ros的完全版

```bash
sudo apt-get install ros-melodic-desktop-full
echo "source /opt/ros/melodic/setup.bash" >> ~/.bashrc
source ~/.bashrc
sudo apt install python-rosdep python-rosinstall python-rosinstall-generator python-wstool build-essential
sudo apt install python-rosdep
```

升级eigen到3.3.7，可以手动解压到eigen下编译

```bash
cd && wget http://bitbucket.org/eigen/eigen/get/3.3.7.tar.gz
mkdir eigen && tar --strip-components=1 -xzvf 3.3.7.tar.gz -C eigen
cd eigen && mkdir build && cd build && cmake .. && make
sudo make install
```

1.4 安装CUDA

确认已装好显卡驱动(没有就在装CUDA时选择安装驱动)，选择CUDA版本10.0

下载run文件，https://developer.nvidia.com/cuda-10.0-download-archive

```bash
sudo sh cuda_10.0.130_410.48_linux.run
sudo sh cuda_10.0.130.1_linux.run
```

1.5 添加环境变量

修改~/.bashrc，末尾添加

```bash
export PATH=/usr/local/cuda-10.0/bin${PATH:+:$PATH}}
export LD_LIBRARY_PATH=/usr/local/cuda-10.0/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}
source /opt/ros/melodic/setup.bash
```

注：安装时会提示缺少CUDNN和TensorRT

安装CUDNN7.6.5，TensorRT7.0.0.11

cudnn直接解压到CUDA安装目录（CUDNN）

```bash
sudo cp cuda/include/cudnn.h /usr/local/cuda/include/
 sudo cp cuda/lib64/libcudnn* /usr/local/cuda/lib64/
 sudo chmod a+r /usr/local/cuda/include/cudnn.h
 sudo chmod a+r /usr/local/cuda/lib64/libcudnn*
```

TensorRT不要下载deb安装包，因为CUDA不是deb装的

```bash
tar xzcf TensorRT-7.0.0.11.Ubuntu-18.04.x86_64-gnu.cuda-10.0.cudnn7.6.tar.gz 
// 注意解压的目录要替换
export LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/home/wy/tensorrt/TensorRT-7.0.0.11/lib
sudo pip3 install tensorrt-7.0.0.11-cp36-none-linux_x86_64.whl 
sudo pip3 install uff-0.6.5-py2.py3-none-any.whl 
sudo pip3 install graphsurgeon-0.4.1-py2.py3-none-any.whl 
sudo cp -r ./lib/* /usr/lib
sudo cp -r ./include/* /usr/include
```

1.6 下载源码

以下操作会出错，可能1.14.0版本有什么冲突，未知。

报错信息是`test-pure_pursuit`重复编译了。

原因是checkout的分支不对，直接执行正确操作即可。

```bash
proxychains git clone https://github.com/Autoware-AI/autoware.ai.git
```

切换到1.14.0版本

```bash
cd autoware.ai
git checkout 1.14.0
proxychains vcs import src < autoware.ai.repos
```

正确操作：

```bash
mkdir -p autoware.ai/src
cd autoware.ai
wget -O autoware.ai.repos "https://gitlab.com/autowarefoundation/autoware.ai/autoware/raw/1.14.0/autoware.ai.repos?inline=false"
vcs import src < autoware.ai.repos
```

1.7 安装依赖

```bash
cd src
catkin_init_workspace
cd ..
proxychains rosdep init
proxychains rosdep update
proxychains rosdep install -y --from-paths src --ignore-src --rosdistro melodic
```

1.8 编译

```bash
 AUTOWARE_COMPILE_WITH_CUDA=1 colcon build --cmake-args -DCMAKE_BUILD_TYPE=Release
```

1.9 运行Autoware

```bash
source ~/autoware.ai/install/setup.bash
roslaunch runtime_manager runtime_manager.launch
```



### 2 Docker 安装

2.1 安装docker engine

```bash
curl -fsSL https://get.docker.com -o get-docker.sh
sudo sh get-docker.sh
```

2.2 将当前用户加入docker用户组

```bash
sudo usermod -aG docker <your-user>
newgrp docker
```

2.3 安装nvidia-docker2

添加密钥

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID) \
   && curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | sudo apt-key add - \
   && curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | sudo tee /etc/apt/sources.list.d/nvidia-docker.list
```

安装

```bash
sudo apt-get update
sudo apt-get install -y nvidia-docker2
sudo systemctl restart docker
```

2.4 拉取Autoware镜像 

```bash
git clone https://gitlab.com/autowarefoundation/autoware.ai/docker.git
cd docker/generic
./run.sh
```

`run.sh`中默认拉取的是`latest-melodic-cuda`分支，不需要修改

