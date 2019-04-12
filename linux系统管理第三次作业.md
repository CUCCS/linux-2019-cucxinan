# Systemd入门

## 实验环境

   虚拟机：ubuntu 18.04.1 server\
   宿主机：Mac
## 实验内容
### &emsp;&emsp;# 重启系统
&emsp;&emsp;&emsp;指令：sudo systemctl reboot

### &emsp;&emsp;# 关闭系统，切断电源
&emsp;&emsp;&emsp;指令：sudo systemctl poweroff

### &emsp;&emsp;# CPU停止工作
&emsp;&emsp;&emsp;指令：sudo systemctl halt

### &emsp;&emsp;# 暂停系统
&emsp;&emsp;&emsp;指令：sudo systemctl suspend

### &emsp;&emsp;# 让系统进入冬眠状态
&emsp;&emsp;&emsp;指令：sudo systemctl hibernate

### &emsp;&emsp;# 让系统进入交互式休眠状态
&emsp;&emsp;&emsp;指令：sudo systemctl hybrid-sleep 

### &emsp;&emsp;# 启动进入救援状态（单用户状态）
&emsp;&emsp;&emsp;指令：sudo systemctl rescue

## 自查清单

## 1. 如何添加一个用户并使其具备sudo执行程序的权限？ 

### &emsp;&emsp;# 添加一个new用户
&emsp;&emsp;&emsp;指令：sudo adduser new

### &emsp;&emsp;# 将new用户加入sudo组内
&emsp;&emsp;&emsp;指令：sudo adduser new sudo

## 2. 如何将一个用户添加到一个用户组？

### &emsp;&emsp;# 将new用户加入用户组内
&emsp;&emsp;&emsp;指令：sudo adduser new 组名


## 3. 如何查看当前系统的分区表和文件系统详细信息？ 


### &emsp;&emsp;# 查看当前系统分区表
&emsp;&emsp;&emsp;指令：sudo fdisk -l\
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;sudo sfdisk -l\
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;cfdisk
### &emsp;&emsp;# 查看文件系统详细信息
&emsp;&emsp;&emsp;指令：df -a

## 4. 如何实现开机自动挂载Virtualbox的共享目录分区？ 
### &emsp;&emsp;# 手动配置
### &emsp;&emsp;# 新建挂载目录
&emsp;&emsp;&emsp;指令：mkdir ~/shared

### &emsp;&emsp;# 挂载文件夹
&emsp;&emsp;&emsp;指令：sudo mount -t vboxsf codes ~/shared

### &emsp;&emsp;# 修改配置文件
&emsp;&emsp;&emsp;指令：sudo gedit /etc/fstab

### &emsp;&emsp;# 修改模块
&emsp;&emsp;&emsp;指令：sudo gedit /etc/modules

## 5. 基于LVM（逻辑分卷管理）的分区如何实现动态扩容和缩减容量？ 

### &emsp;&emsp;# 使用 LVM2 工具集

### &emsp;&emsp;# 查看逻辑卷信息
&emsp;&emsp;&emsp;指令：lvdisplay

### &emsp;&emsp;# 缩容3G
&emsp;&emsp;&emsp;指令：lvreduce --size -3g /dev/bogon-vg/root

### &emsp;&emsp;# 扩容3G
&emsp;&emsp;&emsp;指令：lvextend --size +3g /dev/bogon-vg/root

### &emsp;&emsp;# 更改大小
&emsp;&emsp;&emsp;指令：lvresize --size +3g /dev/bogon-vg/root


## 6. 如何通过systemd设置实现在网络连通时运行一个指定脚本，在网络断开时运行另一个脚本？

### &emsp;&emsp;# 连通时运行此脚本
&emsp;&emsp;&emsp;指令：ExecStartPost = 脚本x.service

### &emsp;&emsp;# 断开时运行此脚本
&emsp;&emsp;&emsp;指令：ExecStopPost = 脚本y.service


## 7. 如何通过systemd设置实现一个脚本在任何情况下被杀死之后会立即重新启动？实现杀不死？

### &emsp;&emsp;# 对Service类型的脚本 : 修改'[Service]'区块中的'Restart'字段,将其设置为always
