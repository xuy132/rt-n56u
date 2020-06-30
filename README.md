### 固件说明 ###
* 默认登陆IP:192.168.2.1 
* 默认用户名/密码:admin/admin
* 默认wifi密码:1234567890
* 集成/取消新增插件请修改此文件: trunk/build_firmware_modify

- 网件的闪存布局要求factory在ubi/firmware之后, 因此额外加入了NETGEAR_LAYOUT、FACTORY_OFFSET及RWFS_OFFSET选项。
>- NETGEAR_LAYOUT表明布局为config及factory在firmware之后。
>- FACTORY_OFFSET直接指定factory的起始位置, 跳过reserved0分区。
>- RWFS_OFFSET直接指定RWFS的起始位置, 跳过reserved1分区。

目前无法直接创建reserved0与reserved1分区, 否则开机时会提示factory尺寸溢出。

- 已适配除官方适配及chongshengB分支外的网件套娃机型: 
>- R6220, MT7603+MT7612
>- HWID为CHJ的机型(NETGEAR-CHJ), MT7603+MT7615: R6260/R6350/R6850
>- HWID为BZV的机型(NETGEAR-BZV), MT7615+MT7615: R6700v2/R6800/R6900v2/R7200/R7450/AC2100/AC2400

***

### 编译说明 ###

* 安装依赖包
```shell
# Debian/Ubuntu
sudo apt update
sudo apt install unzip libtool-bin curl cmake gperf gawk flex bison nano xxd fakeroot \
cpio git python-docutils gettext automake autopoint texinfo build-essential help2man \
pkg-config zlib1g-dev libgmp3-dev libmpc-dev libmpfr-dev libncurses5-dev libltdl-dev wget

# CentOS 7
sudo yum update
sudo yum install ncurses-* flex byacc bison zlib-* texinfo gmp-* mpfr-* gettext \
libtool* libmpc-* gettext-* python-docutils nano help2man fakeroot
sudo yum groupinstall "Development Tools"

# CentOS 8
sudo yum update
sudo yum install ncurses-* flex byacc bison zlib-* gmp-* mpfr-* gettext \
libtool* libmpc-* gettext-* nano fakeroot
sudo yum groupinstall "Development Tools"
# CentOS 8不能直接通过yum安装texinfo, help2man, python-docutils。请去官网下载发行的安装包编译安装
# 以texinfo为例
# cd /usr/local/src
# sudo wget http://ftp.gnu.org/gnu/texinfo/texinfo-6.7.tar.gz
# sudo tar zxvf texinfo-6.7.tar.gz
# cd texinfo-6.7
# sudo ./configure
# sudo make
# sudo make install

# Archlinux/Manjaro
sudo pacman -Syu --needed git base-devel cmake gperf ncurses libmpc gmp python-docutils \
vim rpcsvc-proto fakeroot

```
* 克隆源码
```shell
git clone --depth=1 https://github.com/chongshengB/rt-n56u.git /opt/rt-n56u
```
* 准备工具链
```shell
cd /opt/rt-n56u/toolchain-mipsel

# （推荐）使用脚本下载预编译的工具链：
sh dl_toolchain.sh

# 或者, 也可以从源码编译工具链, 这需要一些时间：
# Manjaro/ArchLinux 用户请使用gcc-8
# sudo pacman -S gcc8
# sudo ln -sf /usr/bin/gcc-8 /usr/local/bin/gcc
# sudo ln -sf /usr/bin/g++-8 /usr/local/bin/g++
./clean_toolchain
./build_toolchain

```
* (可选) 修改机型配置文件
```shell
nano /opt/rt-n56u/trunk/configs/templates/PSG1218.config
```
* 清理代码树并开始编译
```shell
cd /opt/rt-n56u/trunk
./clear_tree
fakeroot ./build_firmware_modify PSG1218
# 脚本第一个参数为路由型号, 在trunk/configs/templates/中
# 编译好的固件在trunk/images里
```

***

### 请参阅 ###
- https://www.jianshu.com/p/cb51fb0fb2ac
- https://www.jianshu.com/p/6b8403cdea46

### 特别说明 ###
* hanwckf源码：https://github.com/hanwckf/rt-n56u
* lean源码: https://github.com/coolsnowwolf/lede
* 汉化字典来自：https://github.com/gorden5566/padavan
* hanwckf更新日志：https://www.jianshu.com/p/d76a63a12eae

