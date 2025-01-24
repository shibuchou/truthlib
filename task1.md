# 已完成Linux环境及bochs环境配置
配置Linux为vmware配置ubunutu版本22.04
# 配置bochs环境过程如下：
## 安装vim命令：
sudo apt install vim
## 安装依赖及工具：
sudo apt install build-essential
sudo apt-get install libghc-x11-dev
sudo apt-get install xorg-dev
## 安装bochs:
下载地址:https://altushost-swe.dl.sourceforge.net/project/bochs/bochs/2.6.8/bochs-2.6.8.tar.gz?viasf=1
解压:tar -zxvf bochs-2.6.8.tar.gz
## 配置bochs的config:
./configure --prefix=/home/shibuchou/truth/bochs --enable-debugger --enable-disasm --enable-iodebug --enable-x86-debugger --with-x --with-x11 LDFLAGS='-pthread'
### 编译:make
### 安装:make install
## 进入bochs目录创建bochsrc.disk配置文件:
touch bochsrc.disk
### 并使用vim进行编辑:
sudo vim bochsrc.disk
## 配置如下:
#第一步，首先设置 Bochs 在运行过程中能够使用的内存，本例为 32MB。  
#关键字为：megs  
megs: 32

#第二步，设置对应真实机器的 BIOS 和 VGA BIOS。  
#对应两个关键字为：romimage 和 vgaromimage  
romimage: file=/home/shibuchou/truth/bochs/share/bochs/BIOS-bochs-latest  
vgaromimage: file=/home/shibuchou/truth/bochs/share/bochs/VGABIOS-lgpl-latest  

#第三步，设置 Bochs 所使用的磁盘，软盘的关键字为 floppy。  
#若只有一个软盘，则使用 floppya 即可，若有多个，则为 floppya，floppyb…  
#floppya: 1_44=a.img, status=inserted

#第四步，选择启动盘符。  
#boot: floppy  
#默认从软盘启动，将其注释  
boot: disk

#改为从硬盘启动。我们的任何代码都将直接写在硬盘上，所以不会再有读写软盘的操作。  
#第五步，设置日志文件的输出。  
log: bochs.out

#第六步，开启或关闭某些功能。

#下面是关闭鼠标，并打开键盘。  
mouse: enabled=0  
keyboard: keymap=/home/shibuchou/truth/bochs/share/bochs/keymaps/x11-pc-us.map  

#硬盘设置  
ata0: enabled=1, ioaddr1=0x1f0, ioaddr2=0x3f0, irq=14
ata0-master: type=disk, path="hd60M.img", mode=flat,cylinders=121,heads=16,spt=63  

#下面的是增加的 bochs 对 gdb 的支持，这样 gdb 便可以远程连接到此机器的 1234 端口调试了

#gdbstub: enabled=1, port=1234, text_base=0, data_base=0, bss_base=0

################### 配置文件结束 #####################
## 此时及配置完成进行查看:
bin/bochs -f bochsrc.disk  
![运行结果](https://github.com/shibuchou/truthlib/blob/main/4ff0de770f48f41dff9038f807df5d2.png)
## 创建启动磁盘:
bin/bximage  
依次输入:  
1  
hd  
flat  
60  
hd60M.img  
