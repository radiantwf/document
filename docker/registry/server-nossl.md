# Registry服务器端配置（无SSL）

#### 步骤
##### 1、安装Docker引擎

##### 2、下载最新版Registry Image
具体方法如下：
 
    docker pull registry:latest

##### 3、添加用户
具体方法如下（用户名：hisign 密码：hisign123）：
 
    stop registryAddUser;docker rm registryAddUser;
    docker run --entrypoint htpasswd --name=registryAddUser registry:latest -Bbn hisign hisign123  >> /opt/data/registry/auth/htpasswd
    docker sleep 3
    stop registryAddUser;docker rm registryAddUser;
如要添加多个用户，反复执行以上命令
 

##### 4、启动服务
具体方法如下（用户名：hisign 密码：hisign123）：
 
    docker stop registryServer;docker rm registryServer;
    docker run -d \
        -v /opt/data/registry/auth/:/auth/ \
        -v /opt/data/registry/images/:/images/ \
        -e STORAGE_PATH=/images \
        -e "REGISTRY_AUTH=htpasswd" \
        -e "REGISTRY_AUTH_HTPASSWD_REALM=Registry Realm" \
        -e REGISTRY_AUTH_HTPASSWD_PATH=/auth/htpasswd \
        -p 5000:5000 \
        --name=registryServer \
        registry:latest
