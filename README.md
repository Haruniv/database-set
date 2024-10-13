# database-set
wget https://gist.githubusercontent.com/lesstif/d72b0d46e58bc7dfcfbed0c910833bc2/raw/51d1e20f92357c4d320c313ec535aa1a14c39191/change-rocklinux-mirror.sh

chmod +x change-rocklinux-mirror.sh

sudo ./change-rocklinux-mirror.sh

sudo yum repolist baseos -v 

sync && echo 3 >> /proc/sys/vm/drop_caches


# install pre-requisites
yum install automake \
binutils bison \
gcc gcc-c++ \
gettext libtool \
make openssl \
openssl openssl-devel \
zlib zlib-devel \
cmake \
gcc-toolset-10-gcc gcc-toolset-10-gcc-c++ gcc-toolset-10-binutils gcc-toolset-10-annobin \
libaio libaio-devel \
perl ncurses-devel \
git redhat-rpm-config rpm-build rpm-sign \
cyrus-sasl cyrus-sasl-devel \
curl curl-devel \
libtirpc-devel \
libudev-devel


shell> dnf install epel-release
shell> dnf config-manager --set-enabled powertools
shell> dnf install rpcgen libtirpc-devel

dnf groupinstall "Development Tools"
dnf install cmake gcc gcc-c++ boost-devel openssl-devel protobuf-devel libaio-devel libevent-devel mysql-devel


sync && echo 3 >> /proc/sys/vm/drop_caches

# download install package
shell> wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-8.4.2.tar.gz

curl -O -L https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-8.4.2.tar.gz


# Preconfiguration setup
shell> groupadd mysql
useradd -r -g mysql -s /bin/false mysql

// useradd 시에 로그인이 되지 않도록 -r -s /bin/false 설정 합니다.
  -r, --system                  create a system account
  -s, --shell SHELL             login shell of the new account

  # source-build specific instructions
shell> tar zxvf mysql-$version.tar.gz
shell> cd mysql $VERSION
shell> mkdir build
shell> cd build
shell> cmake .. \
-DCMAKE_INSTALL_PREFIX=/usr/local/mysql \
-DSYSCONFDIR=/etc \
-DDOWNLOAD_BOOST=1 \
-DWITH_BOOST=/usr/local/src/boost \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_EXAMPLE_STORAGE_ENGINE=1 \
-DWITH_FEDERATED_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DENABLED_PROFILING=1 \
-DWITH_CURL=system \
-DOPTIMIZER_TRACE=1 \
-DWITH_INNODB_MEMCACHED=1 \
-DWITH_SSL=system \
-DWITH_ZLIB=bundled

shell> make
shell> make install

shell> cd /etc
shell> touch my.cnf
shell> chown root:root my.cnf  
shell> chmod 644 my.cnf

shell> vi /etc/my.cnf
[mysqld]
datadir=/usr/local/mysql/data
socket=/tmp/mysql.sock
port=3306
log-error=/usr/local/mysql/data/localhost.localdomain.err
user=mysql
secure_file_priv=/usr/local/mysql/mysql-files
local_infile=OFF

lower_case_table_names = 1

# Postinstallation setup
shell> cd /usr/local/mysql
shell> mkdir mysql-files
shell> chown mysql:mysql mysql-files
shell> chmod 750 mysql-files

shell> cd /usr/local/mysql
shell> mkdir data
shell> chmod 750 data
shell> chown mysql:mysql datadir

shell> cd /usr/local/mysql
shell> bin/mysqld --defaults-file=/etc/my.cnf --initialize --user=mysql

bin/mysqld_safe --defaults-file=/etc/my.cnf --user=mysql &

shell> bin/mysql_ssl_rsa_setup
shell> bin/mysqld_safe --user=mysql &

shell> bin/mysqladmin version

shell> mysql -u root -p
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY 'root-password';

shell> vi ~/.bash_profile
PATH=${PATH}:/usr/local/mysql/bin

shell> cp mysql.server /etc/init.d/mysql
shell> chmod +x /etc/init.d/mysql
shell> chkconfig --add mysql
shell> chkconfig --level 345 mysql on

#mysql 8.4.2 version download url

https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-8.4.2-linux-glibc2.28-x86_64.tar


https://dev.mysql.com/get/Downloads/MySQL-8.4/mysql-8.4.2.tar.gz


https://myinfrabox.tistory.com/253

https://data-architect.notion.site/my-cnf-c937227cd1cb493895e030f0d877f66a
