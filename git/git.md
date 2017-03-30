#Git

### 命令行简介
#### Git global setup
```
git config --global user.name "name"
git config --global user.email "user@email.com"
```
 
#### Create a new repository
```
git clone http(https)://*************************.git
cd test
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master
```
 
#### Existing folder
```
cd existing_folder
git init
git remote add origin http(https)://*************************.git
git add .
git commit
git push -u origin master
```
 
#### Existing Git repository
```
cd existing_repo
git remote rm origin
git remote add origin http(https)://*************************.git
git push -u origin --all
git push -u origin --tags
```
 
#### 快速链接
[客户端下载](https://git-scm.com/downloads)

[GitHub](https://github.com/)
 
[Hisign企业内部GitLab](http://gitlab.hisign.top:6003)