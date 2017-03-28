# Registry客户端配置（无SSL）

#### 步骤
##### 1、客户端修改
###### centos7修改方式
>具体方法如下：
> 
    vi /usr/lib/systemd/system/docker.service
>找到ExecStart=/usr/bin/dockerd这一行，后加入 --selinux-enabled --insecure-registry=211.157.146.6:5000，如下所示：
> 
    ExecStart=/usr/bin/dockerd --selinux-enabled --insecure-registry 211.157.146.6:5000

###### centos6.5修改方式
>具体方法如下：
> 
    vi /etc/init.d/docker
> 
> 
> 
    $exec -d $other_args &>> $logfile &
> 
>=>
> 
    $exec --insecure-registry=211.157.146.6:5000 -d $other_args &>> $logfile &

##### 2、重启Docker引擎服务
>具体方法如下：
> 
    systemctl daemon-reload;systemctl restart docker;

##### 3、登陆Registry服务
>具体方法如下（用户名 hisign 密码 hisign123）：
> 
    docker login 211.157.146.6:5000

##### 4、Registry操作
###### 客户端提交
    docker tag image_name[:tag] 211.157.146.6:5000/image_name[:tag]
    docker push 211.157.146.6:5000/image_name

###### 客户端查找Image
    curl -XGET http://211.157.146.6:5000/v2/_catalog

###### 客户端查找Tag（当前Image的Tag）
    curl -XGET http://211.157.146.6:5000/v2/image_name/tags/list
