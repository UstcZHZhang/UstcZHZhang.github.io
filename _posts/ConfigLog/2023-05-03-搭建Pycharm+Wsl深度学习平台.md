---
title:	"搭建Pycharm+Wsl深度学习平台"
layout: post
categories: Config_Log
---

### 1. 安装Win11

[Download Windows 11 (microsoft.com)](https://www.microsoft.com/zh-cn/software-download/windows11)

### 2. 安装WSL2和Ubuntu

[安装 WSL  Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/install)

[使用 WSL 运行 Linux GUI 应用  Microsoft Learn](https://learn.microsoft.com/zh-cn/windows/wsl/tutorials/gui-apps#install-gedit)

（因为我只是在Linux下配置一个深度学习的环境，所以没有使用Gui）

1. 安装wsl

   ```powershell
   wsl --install
   ```

2. 安装Ubuntu

   我使用的Linux发行版是Ubuntu-20.04

   ```powershell
   wsl --install -d Ubuntu-20.04
   ```

3. 迁移至非系统盘

   默认WSL装的Linux子系统在C盘，可以迁至其他位置

   1. shutdown子系统

      ```powershell
      wsl --shutdown
      ```

   2. 导出Ubuntu

      ```powershell
      wsl --export Ubuntu-20.04 D:\System\Ubuntu-20.04\ubuntu.tar
      ```

   3. 注销原有Ubuntu

      ```powershell
      wsl --unregister Ubuntu-20.04
      ```

   4. 导入

      ```powershell
      wsl --import Ubuntu-20.04 D:\System\Ubuntu-20.04\ D:\System\Ubuntu-20.04\ubuntu.tar --version 2
      ```

   5. 删除压缩包

      ```powershell
      del D:\System\Ubuntu-20.04\ubuntu.tar
      ```

   6. 重新启动Wsl

4. Ubuntu更改镜像源

   [ubuntu  镜像站使用帮助  清华大学开源软件镜像站  Tsinghua Open Source Mirror](https://link.zhihu.com/?target=https%3A//mirrors.tuna.tsinghua.edu.cn/help/ubuntu/)

   [ubuntu镜像-ubuntu下载地址-ubuntu安装教程-阿里巴巴开源镜像站](https://link.zhihu.com/?target=https%3A//developer.aliyun.com/mirror/ubuntu)

   [全球镜像https://launchpad.net/ubuntu/+archivemirrors](https://link.zhihu.com/?target=https%3A//launchpad.net/ubuntu/%2Barchivemirrors)

   [HOW TO USE](https://link.zhihu.com/?target=https%3A//github.com/microsoft/WSL/issues/2477%23issuecomment-371285353)

   我这里先做了一个备份，然后使用了清华的镜像源

   ```bash
   sudo cp  /etc/apt/sources.list /etc/apt/sources.list.bak
   sudo vim /etc/apt/sources.list
   ```

   ![image-20230503151620924](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503151620924.png)

   更新一下

   ```bash
   sudo apt-get update
   sudo apt-get upgrade
   ```

   在win11和wsl最新版（2023.5）的NVIDIA驱动下，在WSL内可以直接看到显卡信息了

   ```bash
   nvidia-smi
   ```

   ![image-20230503151825272](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503151825272.png)

### 3. 安装Cuda和cudnn

1. 安装wsl CUDA

   [CUDA Toolkit Archive  NVIDIA Developer](https://developer.nvidia.com/cuda-toolkit-archive)

   选择需要的CUDA版本（我这里选择的11.8），按下图选择，得到安装命令

   ![image-20230503153350070](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503153350070.png)

   ```bash
   wget https://developer.download.nvidia.com/compute/cuda/repos/wsl-ubuntu/x86_64/cuda-wsl-ubuntu.pin
   sudo mv cuda-wsl-ubuntu.pin /etc/apt/preferences.d/cuda-repository-pin-600
   wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
   sudo dpkg -i cuda-repo-wsl-ubuntu-11-8-local_11.8.0-1_amd64.deb
   sudo cp /var/cuda-repo-wsl-ubuntu-11-8-local/cuda-*-keyring.gpg /usr/share/keyrings/
   sudo apt-get update
   sudo apt-get -y install cuda
   ```

   安装完成后，设置环境变量

   ```bash
   vim ~/.bashrc
   ```

   在最后加入，配置环境变量

   ```bash
   export PATH=/usr/bin:$PATH
   export PATH=/usr/local/cuda-11.8/bin:$PATH
   export LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64:$LD_LIBRARY_PATH
   ```

   更新一下source

   ```bash
   source ~/.bashrc
   ```

   验证安装是否成功

   ```bash
   nvcc -V
   ```

   ![image-20230503154212366](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503154212366.png)

2. 安装cudnn

   1. 下载适合的cudnn版本

      [cuDNN Archive  NVIDIA Developer](https://developer.nvidia.com/rdp/cudnn-archive)

      ![image-20230503160956168](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503160956168.png)

   2. 安装deb

      找到下载好的deb文件，进行安装（我这里把文件复制到了Ubuntu的~目录下进行安装）

      ```bash
      cd /mnt/c/Users/zhzhang/Downloads
      cp ./cudnn-local-repo-ubuntu2004-8.9.0.131_1.0-1_amd64.deb ~/
      sudo dpkg -i ./cudnn-local-repo-ubuntu2004-8.9.0.131_1.0-1_amd64.deb
      ```

   3. 拷贝密钥

      ```bash
      cp /var/cudnn-local-repo-ubuntu2004-8.9.0.131/cudnn-local-54F08CCB-keyring.gpg /usr/share/keyrings/
      ```

      （这里最好自己手动进文件夹，版本不同文件名不同）

   4. 刷新元数据

      ```bash
      sudo apt-get update
      ```

   5. 查看支持的软件包

      ```bash
      apt-cache search cudnn
      ```

      ![image-20230503162310260](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503162310260.png)

   6. 指定刚刚检查的软件包名称以安装cudnn

      ```bash
      sudo apt -y install libcudnn8 libcudnn8-dev
      ```

   7. 检查已安装的软件包

      ```bash
      dpkg -l | grep cudnn 
      ```

      ![image-20230503162712220](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503162712220.png)

### 4. 安装Miniconda

1. 下载安装包

   ```bash
   wget -c https://repo.anaconda.com/miniconda/Miniconda3-latest-Linux-x86_64.sh
   chmod 777 Miniconda3-latest-Linux-x86_64.sh
   ```

2. 安装conda

   ```bash
   bash Miniconda3-latest-Linux-x86_64.sh
   ```

3. 设置环境变量

   ```bash
   vim ~/.bashrc
   export PATH=~/miniconda3/bin:$PATH
   ```

4. 更新镜像源

   ```bash
   vim ~/.condarc
   channels:
     - defaults
   show_channel_urls: true
   default_channels:
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
     - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
   custom_channels:
     conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
     simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
   ```

### 5. 安装Pytorch

1. 创建虚拟环境

   ```bash
   conda create -n env_pytorch python=3.9
   ```

2. 激活虚拟环境

   ```bash
   conda activate env_pytorch
   ```

3. 安装Pytorch

   [Start Locally  PyTorch](https://link.zhihu.com/?target=https%3A//pytorch.org/get-started/locally/)

   根据cuda版本选择合适的torchgpu版本

   ![image-20230503164105075](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503164105075.png)

   ```bash
   conda install pytorch torchvision torchaudio pytorch-cuda=11.8 -c pytorch -c nvidia
   ```

   可以考虑使用清华镜像加速

   ```bash
   -i https://pypi.tuna.tsinghua.edu.cn/simple some-package
   ```

4. 测试GPU

   ```python
   # torch
   import torch
   torch.cuda.is_available()
   ```

   ![image-20230503165309551](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503165309551.png)

### 6. 安装Tensorflow

1. 创建虚拟环境

   ```bash
   conda create -n env_tensorflow python=3.9
   ```

2. 激活虚拟环境

   ```bash
   conda activate env_tensorflow
   ```

3. 安装Tensorflow

   [TensorFlow (google.cn)](https://link.zhihu.com/?target=https%3A//tensorflow.google.cn/install%3Fhl%3Dzh-cn)

   ```bash
   pip install --upgrade pip
   pip install tensorflow
   ```

4. 测试GPU

   ```python
   # tensorflow
   import tensorflow as tf
   tf.config.list_physical_devices()
   ```

### 7. 安装Pycharm

[Configure an interpreter using WSL  PyCharm Documentation (jetbrains.com)](https://www.jetbrains.com/help/pycharm/using-wsl-as-a-remote-interpreter.html#configure-wsl)

1. 下载pycharm专业版

   [PyCharm: the Python IDE for Professional Developers by JetBrains](https://www.jetbrains.com/pycharm/)

2. 申请学生免费

   按照下面顺序操作

   ![image-20230503170710813](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503170710813.png)

   ![image-20230503170816282](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503170816282.png)

   ![image-20230503170914068](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503170914068.png)

3. 使用Pycharm远程连接WSL上的project

   按照下面顺序操作

   ![image-20230503174221143](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503174221143.png)

   ![image-20230503174340911](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503174340911.png)

   ![image-20230503174537223](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503174537223.png)

4. 设置Pycharm使用的虚拟环境

   ![image-20230503174815155](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503174815155.png)

   ![image-20230503175046412](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503175046412.png)

   ![image-20230503175250858](/images/ConfigLog/2023-05-03-搭建Pycharm+Wsl深度学习平台_images/image-20230503175250858.png)



主要参考：

1. [2023最新WSL搭建深度学习平台教程（适用于Docker-gpu、tensorflow-gpu、pytorch-gpu) - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/621142457)
2. [【记录】在Win11的WSL下使用GPU运行PyTorch&PyCharm编程 - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/460471318)