# Docker离线安装（CentOS7）
#### 快速链接
[离线包下载地址](https://yum.dockerproject.org/repo/main/centos/7/Packages/)
 
#### 步骤
##### 1、拷贝离线安装包
离线安装包包含：
 
libcgroup-(verson).rpm
 
libcgroup-devel-(verson).rpm
 
libcgroup-parm-(verson).rpm
 
libcgroup-tools-(verson).rpm
 
docker-engine-(verson).rpm
 
docker-engine-selinux-(verson).rpm
 

##### 2、安装
安装方法如下：
 
    rpm -ivh ./libcgroup-*
    mkdir -p /var/lib/docker
    rpm -ivh ./docker-engine-selinux-1.12.3-1.el7.centos.noarch.rpm
    rpm -ivh ./docker-engine-1.12.3-1.el7.centos.x86_64.rpm

##### 3、重启Docker服务
    service docker restart
