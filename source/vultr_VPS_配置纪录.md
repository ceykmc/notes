# vultr VPS 配置纪录

---

### 安装[锐速破解版](https://github.com/91yun/serverspeeder)

### 安装 mosh

* debian 服务端安装 [mosh](https://mosh.mit.edu/)

	```
	sudo apt-get install mosh
	```
* mac 客户端安装 mosh

	```
	brew install mobile-shell
	```
	
### 增加普通权限的用户
```
adduser lijun
passwd lijun
```

### 增加 SSH 公钥

将已有的公钥文件 id\_rsa.pub，拷贝到 VPS 上 /root/.ssh 路径下，并将 id\_rsa.pub 重命名成 authorized\_keys

已有的 rd\_rsa 文件放置在用户目录 .ssh 文件下

### 开启 shadowsocks 服务

* 安装 python-pip

	```
	sudo apt-get install python-pip
	```
* 安装 shadowsocks

	```
	pip install shadowsocks
	```
	
* 新建 shadowsocks.json 文件，放置于 /etc 目录下

	```
	{
        "server": "45.32.28.87",
        "server_port": 13171, 
        "local_address": "127.0.0.1", 
        "local_port": 1080, 
        "password": "wwwfox", 
        "timeout": 300, 
        "method": "aes-256-cfb", 
        "fast_open": false
	}
	```
* 将 shadowsocks 服务添加到开机启动里面，编辑 /etc/rc.local 文件，添加如下代码：

```
/usr/local/bin/ssserver -c /etc/shadowsocks.json
```

### 安装 Hexo

* 安装 Git

	```
	apt-get install git
	```
	
* 安装 [Node.js](https://www.vultr.com/docs/installing-node-js-and-express)
* 安装 Hexo

	```
	npm install -g hexo-cli
	```
* 安装 Rsync

	```
	npm install hexo-deployer-rsync --save
	```
	修改 _config.yml 文件中的 deploy 如下：
	
	```
	deploy:
     type: rsync
     host: 104.224.170.13
     user: root
     root: /www/hexo
     port: 29876
     delete: true
     verbose: true
     ignore_errors: false

	```
	
### 安装 Nginx

```
apt-get install nginx
```
	
	
	
	
	
	