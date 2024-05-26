#周报
##租VPS
上vmiss官方网站购买vps，选择centos9，本人用的是电信，选择的线路是CN2。之后获得了Main IP和Root pass，尝试登录：win+r之后输入cmd，进入“命令提示符”，输入
```
ssh root@xxx.xxx.xxx.xxx
```
然后提示输入密码，即可登录对应的vps。
租域名
上易名网站注册域名，我注册的是zhzh118.com，进行实名认证后，即可购买zhzh118.com并使用。

##在Windows本地上可以实现ssh免密登录
1.生成ssh密钥：在vps服务器终端上输入命令:
```
ssh-keygen -t rsa -b 4096 -C "1184464111@qq.com"
```
2.在.ssh目录下创建authorized_keys文件，并把ssh目录的权限设置为700，authorized_keys文件的权限设置为600：打开vps终端输入
```
cd ~/.ssh，
chmod 700 ~/.ssh，touch authorized_keys
chmod 600 authorized_keys
```
3.生成好的公钥添加到authorized_keys文件中
配置如下：
```
cat ~/.ssh/id_rsa_pub >> ~/.ssh/authorized_keys
cat authorized_keys检查公钥是否复制成功
```
4.将私钥复制到本地文件夹中，本地.ssh目录下，创建rsa.private.key文件
配置：
```
vim ~/.ssh/id_rsa
```
可以查看文件内容，复制私钥到rsa.private.key文件中，（确保MD5值要一样），可使用scp传文件。
scp username@remote_host:/path/to/remote/file /path/to/local/directory
5.在本地.ssh目录中，创建config文件，在里面写：
```
Host vmiss-118
User root
HostName 38.47.116.50
ServerAliveInterval 60
PreferredAuthentications publickey
IdentityFile "C:\Users\Zhang Peng\.ssh\rsa.private.key"
```
6.之后就可以在PowerShell终端上免密登录
```
ssh vmiss-118
```
##注册cloudflare,将购买的域名在cloudflare上做解析
    登录到你的Cloudflare账户。
在仪表盘上，点击“Add a Site”（添加站点）。
点击“添加站点”，类型选为A，名称为域名，内容为注册的ip地址，代理状态为仅DNS。
##Caddy配置
在ssh服务器上安装caddy,配置如下：
```
yum install yum-plug-copr
Yum copr enable @caddy/caddy
yum install caddy
```
启动caddy,配置如下：
```
sudo systemctl daemon-reload
sudo systemctl enable caddy
sudo systemctl start caddy
```
将SSL/TLS配置，勾选为完全（严格）
查看2019端口是否被占用
```
Lsof -i tcp:2019
```
释放2019端口，kill对应的进程
```
sudo systemctl stop 服务名称
```
重新启动caddy
```
systemctl start caddy
```
下载文件caddy-dns/cloudflare,实现ssl证书可以被cloudflare获取
```
wget https://caddyserver.com/api/download?os=linux&arch=amd64&version=2.7.6
```
将下载的二进制包覆盖
```
whereis caddy /usr/bin/caddy
```
配置文件路径一般为/etc/caddy/Caddyfile,将cloudflare复制下来的token加入到tls配置。
内容为:
```
zhzh118.com:443 {
        root * /usr/share/caddy
        encode gziip
        file_server
        tls {
                dns cloudflare mdnPNno_v8zy0tSyVWJHnlO-K2ICj_W8R2kA2lk6
resolvers 1.1.1.1
        }
}
```