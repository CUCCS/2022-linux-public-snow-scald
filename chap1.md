# Chapter1: Linux基础

## 实验环境

* VM VirtualBox
* Ubuntu20.04.2-server
* 阿里云 云起实验室 零门槛云上实践平台

## 实验问题

1. 调查并记录实验环境的如下信息： 
    + 当前 Linux 发行版基本信息
    + 当前 Linux 内核版本信息
2. virtualbox安装完 Ubuntu 之后新添加的网卡如何实现系统开机自动启用和自动获取 IP？
3. 如何使用 scp 在「虚拟机和宿主机之间」、「本机和远程 Linux 系统之间」传输文件？
4. 如何配置 SSH 免密登录？

## 实验过程

### 调查并记录实验环境信息

> 要查看发行版信息，使用命令 lsb_release -a

本地ubuntu20.04以及阿里云线上计算资源结果分别如下：

![](png\ubuntu_dis.png)

![](png\centos_dis.png)

> 要查看内核版本信息，使用命令 uname -a

本地ubuntu20.04以及阿里云线上计算资源结果分别如下：

![](png\ubuntu_kernel.png)

![](png\centos_kernel.png)

### 新网卡的自动启用和获取ip

virtualbox安装linux系统时安装了两块显卡，分别设置为`NAT`与`仅主机`。可通过命令`ifconfig`来查看网卡工作情况。

![](png\ifconfig.png)

可以看到两块网卡均处于工作状态。

那么为什么会有这一项实验内容呢？查阅ubuntu官方文档得到：Ubuntu的dhch自动分配可能未添加新网卡导致该网卡不能自动启动。Ubuntu17之后，该选项配置由netplan管理。由于我没有遇到这个问题，在此不再赘述。

### 使用scp传递文件
虚拟机与远程服务器道理相同，因此仅展示服务器与本地间传输即可。

使用scp传输文件的命令为：

    \\从本地传到服务器
    scp [本地路径] [服务器用户名]@[服务器ip地址]:[服务器端路径]
    
    \\从服务器传到本地
    scp [服务器用户名]@[服务器ip地址]:[服务器端路径] [本地路径]

若需传输文件夹的话，加参数 `-r`，即 recursively.  

例如，在本地和服务器分别建立测试文件`LtoS.txt`和`StoL.txt`。  
本地目录为`C:\tempworkspace\LtoS.txt`，文件内容为`"local to server"`。  
服务器端目录为`\home\cuc\StoL.txt`文件内容为`"server to local"`

使用命令 

    scp C:\tempworkspace\LtoS.txt cuc@192.168.56.101:\home\cuc
    scp cuc@192.168.56.101:\home\cuc\StoL.txt C:\tempworkspace

结果如图：

![](png\doctrans.png)

在服务器和本地对应目录下查看，发现文件已传输成功。

![](png\server.png)

![](png\local.png)

### ssh免密登录
主要流程是通过ssh-keygen生成公私钥对，再通过ssh-copy-id命令将公钥传输至服务器端。由于我之前已生成过rsa密钥，故这里只演示如何上传。

![](png\withoutpwd.png)

可以看到，重新使用ssh连接服务器，已可以成功免密登录。

## 参考文献

[Ubuntu documentation about network-configuration](https://ubuntu.com/server/docs/network-configuration)

《Linux for Developers》.William Rothwell.Addison-Wesley
	
《Using And Administering Linux: Volume 1: Zero To SysAdmin: Getting Started》David Both. Apress 2020.






