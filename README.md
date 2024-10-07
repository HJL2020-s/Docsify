# **笔记**
## **STM32**
### **裸机开发**
待补充
### **freeRTOS**
待补充
## **树莓派**
待补充
## **RK356X**
待补充

# **安装Docker**
## apt换源
### 备份原有源，或者直接注释
```bash
sudo cp /etc/apt/sources.list /etc/apt/sources.list.bak
```
### 编辑现有源
```
sudo nano /etc/apt/sources.list
```
### 阿里云源
```
deb http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-updates main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-backports main restricted universe multiverse
deb-src http://mirrors.aliyun.com/ubuntu/ jammy-security main restricted universe multiverse

```
### 更新系统
```
sudo apt update 
sudo apt upgrade -y
```
### 安装相应依赖
```
sudo apt install apt-transport-https ca-certificates curl software-properties-common -y
```
## 常规方式安装Docker
### 添加Docker官方的GPG密钥
```
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

``` 
### 添加Docker软件源
```
echo "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
```
### 更新APT包索引
```
sudo apt update
```
### 安装Docker
```
sudo apt install docker-ce docker-ce-cli containerd.io -y

```
## 安装Docker失败时可以考虑使用国内镜像
```bash
curl -fsSL https://mirrors.aliyun.com/docker-ce/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io
```
# **Docker换源**
编辑 Docker 的配置文件 `/etc/docker/daemon.json`通常这个文件是需要自己创建的：
```
sudo nano /etc/docker/daemon.json
```
添加内容
```
{
  "registry-mirrors": [
"可以去华为云中复制"
  ]
}
```
重启Docker服务
```
sudo systemctl daemon-reload
sudo systemctl restart docker
```
# **Docker拉取leanote**
```
docker pull axboy/leanote
```
**这个镜像据说自带Mongo，但是实际使用发现还是需要再启动一个Mongo镜像**
## 创建文件夹
```
mkdir -p /data/leanote/{db,conf,files,upload}
cd /data/leanote
```
# **Docker拉取运行Mongo镜像**
```
docker run -d \
  --name leanote-mongo \
  -v leanote-data:/data/db \
  mongo:3.2
```
`-d` 表示在后台运行容器。
`--name leanote-mongo` 指定容器名称为 `leanote-mongo`。 
`-v leanote-data:/data/db` 将数据保存在 Docker 卷中以确保持久化。
# **启动Leanote镜像**
启动 Leanote 容器，并将其连接到 MongoDB：
```
docker run -d \
  --name leanote \
  --link leanote-mongo:mongo \
  -p 9000:9000 \
  axboy/leanote
```
 `-d` 表示在后台运行容器。 
 `--name leanote` 指定容器名称为 `leanote`。 
 `--link leanote-mongo:mongo` 将 Leanote 容器与 MongoDB 容器连接，MongoDB 容器名为 `mongo`。 
 `-p 9000:9000` 将宿主机的 `9000` 端口映射到容器内的 `9000` 端口。





