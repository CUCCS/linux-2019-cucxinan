# 一、实验目的

&emsp;&emsp;&emsp;配置无人值守安装linux-unbentu操作系统iso并在Virtualbox中完成自动安装

# 二、实验环境

&emsp;&emsp;&emsp;宿主机：Mac（自带ssh服务）\
&emsp;&emsp;&emsp;虚拟机：virtualbox / ubuntu-18.04.1-server-amd64\
&emsp;&emsp;&emsp;网络配置：NAT/ host-only\
&emsp;&emsp;&emsp;附加准备：  
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;1.手动安装一个可用的 Ubuntu 系统环境，利用ifconfig -a命令查看网络接口配置信息  
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;2.启用dhcp（启用dhcp指令：sudo dhclient enp0s8 )  
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;3.开启ssh服务，宿主机（Mac）通过ssh远程连接虚拟机  
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;4.在安装好的虚拟机中开启ssh服务  
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;（如果没有ssh服务，则使用指令 sudo apt install openssh-server 安装）\
 &emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;5.查看ssh是否正常启用（查看指令：ps -e | grep ssh ）  
&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&emsp;&ensp;6.在MAC上尝试远程登录（指令'ssh cookie@192.168.68.3'），屏幕显示登录成功。

# 三、实验过程

## 1. 创建iso镜像文件
### &emsp;***步骤：***
          # 在当前正在使用的用户目录下创建一个用于挂载iso的文件目录
	        指令：mkdir loopdir

          # 挂载iso镜像文件到这个目录
            指令：sudo mount -o loop ubuntu-16.04.1-server-amd64.iso loopdir

          # 在当前用户目录下新建一个用于克隆的目录
	        指令：mkdir cd

	      # 克隆光盘文件
            指令：rsync -av copydir/ cd

          # 卸载iso镜像文件
            指令：sudo umount loopdir

## 2. 进入cd'copydir'工作目录下，设置安装引导界面
 ### &emsp;***步骤：***
          # 进入copydir工作目录下
	        指令：/cd/copydir

          # 打开文件，修改配置缩短超时等待时间，添加以下内容到该文件后强制保存退出
	        添加文件：'label autoinstall'
	                 'menu label ^Auto Install Ubuntu Server'
	                 'kernel /install/vmlinuz'
	                 'append  file=/cdrom/preseed/ubuntu-server-autoinstall.seed debian-installer
                     /locale=en_US console-setup/layoutcode=us keyboard-configuration/layoutcode=us console-setup
                     /ask_detect=false localechooser/translation/warn-light=true localechooser
                     /translation/warn-severe=true initrd=/install/initrd.gz root=/dev/ram rw quiet'
            指令：sudo vi isolinux/isolinux.cfg

          # 下载（老师提供的）定制好的ubuntu-server-autoinstall.seed
            指令：sudo wget https://github.com/c4pr1c3/LinuxSysAdmin/blob/master
                /exp/chap0x01/cd-rom/preseed/ubuntu-server-autoinstall.seed

          # 将下载后的seed移动到指定的目录下
	        指令：sudo mv ubuntu-server-autoinstall.seed /copydir/preseed/
                （此时通过'sudo chmod -R 775 '目标路径开放写权限，否则路径无法被识别）

          # 修改isolinux/isolinux.cfg，重新生成md5sum.txt
            指令：find . -type f -print0 | xargs -0 md5sum > /tmp/md5sum.txt
	             sudo mv /tmp/md5sum.txt md5sum.txt

          # 新建shell文件
	        指令：sudo vim shell
 
          # 添加以下内容到shell文件中
	        添加文件：'IMAGE=custom.iso'
	                 'BUILD=~/cd/'
	                 'mkisofs -r -V "Custom Ubuntu Install CD" \
	                 -cache-inodes \
	                 -J -l -b isolinux/isolinux.bin \
	                 -c isolinux/boot.cat -no-emul-boot \
	                 -boot-load-size 4 -boot-info-table \
	                -o $IMAGE $BUILD'
            指令：mkisofs -r -V "Custom Ubuntu Install CD"

          # 首先安装genisoimage（便于获取目标数据执行shell），再执行shell命令
	        指令：sudo apt-get install genisoimage（安装） | sudo bash shell（执行）

## 3. 下载虚拟机中生成的无人值守镜像到宿主机
### &emsp;***步骤：***
          #宿主机从虚拟机下载custom.iso镜像

          #安装成功
