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
