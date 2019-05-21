
---

title: travis-ci自动部署到vps

date: 2019-03-01 16:02:10 +0800

tags: []

---
<a name="78698512"></a>
### 一、环境：
- vps环境：ubuntu

<a name="1428a7ff"></a>
### 二、流程：

<a name="6e8b1f3d"></a>
#### 1. 安装travis命令行工具：
```
sudo apt-get install ruby-dev  
# 安装travis命令行工具，gem指令需要先安装ruby
sudo gem install travis
```

<a name="cffd1de7"></a>
#### 2. 给用户sudo权限：
```
adduser hadoop

chmod 740 /etc/sudoers

sudo vim /etc/sudoers

# User privilege specification 在root行后面增加
hadoop    ALL=(ALL:ALL) ALL

# 保存退出后，修改回文件权限
chmod 440 /etc/sudoers
```

![image.png](https://cdn.nlark.com/yuque/0/2019/png/155457/1551427597144-b800e8d2-e834-44c6-b9b0-81035808b257.png#align=left&display=inline&height=445&name=image.png&originHeight=490&originWidth=858&size=40492&status=done&width=780)

<a name="1aceaf37"></a>
#### 3. 配置ssh
```
# 生成ssh密钥对 注意密码要为空，Travis自动过程中才不会被输入密码步骤卡住，生成后目录 /home/hadoop/.ssh/id_rsa
ssh-keygen -t rsa
# 设置.ssh目录为700
chmod 700 ~/.ssh/
# 设置.ssh目录下的文件为600
chmod 600 ~/.ssh/*
# 切换到.ssh/目录
cd .ssh/
# 将公钥内容添加到authorized_keys
cat id_rsa.pub >> authorized_keys
```

<a name="ca658900"></a>
#### 4. 配置travis

```
# 在home目录下拉取Source仓库
cd /home
git clone 你的仓库.git 
# cd到仓库根目录
# 登录github帐号
travis login --auto

# 在项目下新建.travis.yml
vi .travis.yml

# 生成加密公钥文件id_rsa.enc并自动增加解密行到.travis.yml文件；-r 指向GitHub的Source仓库
travis encrypt-file ~/.ssh/id_rsa --add -r Ghostdar/项目名称
```

<a name="8697adee"></a>
#### 5. 修改.travis.yml配置

```
language: node_js
node_js:
- '8'
branchs:
  only:
  - master
addons:
  ssh_known_hosts:
  - 服务器IP
before_install:
- openssl aes-256-cbc -K $encrypted_2c31c2abd686_key -iv $encrypted_2c31c2abd686_iv -in id_rsa.enc -out ~/.ssh/id_rsa -d

install:
- npm install

script:
- npm run build

after_success:
- chmod 600 ~/.ssh/id_rsa
- ssh hadoop@服务器IP -o StrictHostKeyChecking=no 'cd ~/项目文件夹 && git pull && npm install && npm run build'   #使用ssh连接服务器
```
<a name="d41d8cd9"></a>
#### 
<a name="036dc766"></a>
### 三、需要注意的点：

- 确保git项目是通过ssh拉取的。
- 确保github添加了ssh-key。

