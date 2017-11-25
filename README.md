# shadowsocks_config
shadowsocks_config for centos7

安装必备的开发环境
## CentOS / Fedora / RHEL
yum install epel-release -y
yum install gcc gettext autoconf libtool automake make pcre-devel asciidoc xmlto udns-devel c-ares-devel libev-devel -y

# Installation of Libsodium
export LIBSODIUM_VER=1.0.15
wget https://download.libsodium.org/libsodium/releases/libsodium-$LIBSODIUM_VER.tar.gz
tar xvf libsodium-$LIBSODIUM_VER.tar.gz
pushd libsodium-$LIBSODIUM_VER
./configure --prefix=/usr && make
sudo make install
popd
sudo ldconfig

# Installation of MbedTLS
export MBEDTLS_VER=2.6.0
wget https://tls.mbed.org/download/mbedtls-$MBEDTLS_VER-gpl.tgz
tar xvf mbedtls-$MBEDTLS_VER-gpl.tgz
pushd mbedtls-$MBEDTLS_VER
make SHARED=1 CFLAGS=-fPIC
sudo make DESTDIR=/usr install
popd
sudo ldconfig

# Start building
./autogen.sh && ./configure && make
sudo make install



让shadowsocks开机启动
将shadowsocks.init.txt脚本改名为shadowsocks放到/etc/init.d/目录
sudo chmod a+x /etc/init.d/shadowsocks
sudo chkconfig --add shadowsocks
sudo chkconfig shadowsocks on

如果配合kcptun使用，可以将kcptun.init.txt做上面同样的操作

centos7可能会有防火墙设置

开放防火墙端口

永久的开放需要的端口

sudo firewall-cmd --zone=public --add-port=3000/tcp --permanent
sudo firewall-cmd --zone=public --add-port=3000/udp --permanent
sudo firewall-cmd --reload

之后检查新的防火墙规则

firewall-cmd --list-all

关闭防火墙

//临时关闭防火墙,重启后会重新自动打开

systemctl restart firewalld
//检查防火墙状态
firewall-cmd --state
firewall-cmd --list-all
//Disable firewall
systemctl disable firewalld
systemctl stop firewalld
systemctl status firewalld
//Enable firewall
systemctl enable firewalld
systemctl start firewalld
systemctl status firewalld
