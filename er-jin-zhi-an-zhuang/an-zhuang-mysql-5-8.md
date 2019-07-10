### 安装MySQL 5.8

添加安装源

> cp -f /opt/bigops/install/yum.repos.d/\* /etc/yum.repos.d/

设置安装源，打开对应版本的源

> vi /etc/yum.repos.d/mysql-community.repo

安装MySQL

> yum -y install mysql-community-server mysql-community-client mysql-community-devel mysql-community-libs-compat

启动数据库

centos 6启动命令

> chown -R mysql:mysql /var/lib/mysql
>
> service mysqld start
>
> 如果启动失败，有可能/var/lib/mysql/有以前的残留文件，需要删除

centos 7启动命令

> systemctl start  mysqld.service

查找MySQL初始化的登录密码

> egrep 'temporary password' /var/log/mysqld.log

登录MySQL

> mysql -uroot -p

MySQL 5.8取消密码复杂度及更新密码

> set global validate\_password.policy=0;
>
> set global validate\_password.mixed\_case\_count=0;
>
> set global validate\_password.number\_count=0;
>
> set global validate\_password.special\_char\_count=0;
>
> set global validate\_password.length=6;
>
> \#修改过期规则
>
> ALTER USER 'root'@'localhost' IDENTIFIED BY 'your\_password' PASSWORD EXPIRE NEVER;
>
> \#更新密码
>
> ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql\_native\_password BY 'your\_password';
>
> flush privileges;

优化MySQL 5.8

> cp -f /opt/bigops/install/lnmp\_conf/my-5.8.cnf /etc/my.cnf
>
> 修改datadir=/var/lib/mysql为你的数据存储目录
>
> 修改innodb\_buffer\_pool\_size=3G为你的内存的60%

最后重启MySQL

> service mysqld restart

注意：非MySQL 5.7和5.8版本请参考/opt/bigops/install/lnmp\_conf/my-5.7.cnf进行对应优化
