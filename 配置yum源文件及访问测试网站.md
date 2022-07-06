# 配置yum源
- 需要先配置一个软件仓库，阿里云的yum源
- 通过yum命令，直接去安装各种你想要的应用程序

```bash
1.#先安装一个工具，wget
yum install wget -y

2.#备份旧的yum源文件配置文件
cd /etc/yum.repos.d/
mkdir repo-bak
mv ./* ./repo.bak

3.#下载阿里云yum源
wget -O /etc/yum.repos.d/CentOS-Base.repo https://mirrors.aliyun.com/repo/Centos-vault-8.5.2111.repo

4.#下载epel源
yum install -y https://mirrors.aliyun.com/epel/epel-release-latest-8.noarch.rpm

5.#将 repo 配置中的地址替换为阿里云镜像站地址(RHEL8)
sed -i 's|^#baseurl=https://download.example/pub|baseurl=https://mirrors.aliyun.com|' /etc/yum.repos.d/epel*
sed -i 's|^metalink|#metalink|' /etc/yum.repos.d/epel*

6.#检查阿里云的yum软件仓库配置文件
ls /etc/yum.repos.d

7.#选择安装应用程序
yum install nginx -y

8.#启动nginx应用程序 管理应用程序(systemctl)
systemctl start nginx

9.#验证nginx是否正确启动(如何检查机器的进程信息)
检查进程命令 ps
ps -ef | grep "nginx"

10.#查看端口的用法，查看你linnux网络连接信息的命令
netstat -tunlp | grep "nginx"
#如果显示netstat命令未找到
yum install net-tools


```

**名词解释：**
进程信息、端口

---
*[进程信息]:   一个应用程序跑起来了，就有一个进程记录，相当于win任务管理器
*[端口]:   提供服务的一个窗口，nginx应用程序默认使用80端口提供服务

# 访问nginx服务页面
机器ip地址:80端口即可
>ip:80

如果出现无法访问界面：
![exit 0](https://img-blog.csdnimg.cn/989f3c55f72c4a1dbd4ef06fd55fa8be.png#pic_center =400x300)

- 方法一：
 
查看是否80端口被占用
> netstat -lnp|grep 80
> 
![在这里插入图片描述](https://img-blog.csdnimg.cn/ced0f5d2537e48efbc43c2b1cfbfba01.png)
例如我这里80端口被7013占用。**解决方法：**
> kill -9 7013
systemctl start nginx

- 方法二：
```
#对80端口进行防火墙配置
firewall-cmd --zone=public --add-port=80/tcp --permanent 

#重启防火墙服务
systemctl restart firewalld.service 
```
==再次到浏览器中访问页面得到类似如下即成功==

![在这里插入图片描述](https://img-blog.csdnimg.cn/dc0987ed7d2c47b78cac371ef4141eef.png)

---

>- 启动nginx服务
systemctl start nginx
>- 停止nginx服务
systemctl stop nginx

**修改网站显示内容**

```bash
#利用curl命令 发起http网络请求，并且验证对方网站信息
#查看淘宝网的信息
curl https://www.taobao.com/

#查看淘宝网的web服务器信息，它是tengine
curl -I https://www.taobao.com/
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/dd64cc5cdc574f05aa8b1bcbc75dfdbd.png#pic_center)

```bash
#查看自己的web信息，它是nginx/1.14.1
curl -I 192.168.1.11
#后面ip为本机自身ip各不相同
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/60ef02e3d2e243759352e7d2b3c9ea2f.png#pic_center)

---

**修改nginx首页**

```bash
#查看nginx安装文件，路径信息
rpm -ql nginx

#查看index相关信息
rpm -ql nginx | grep index
```
![在这里插入图片描述](https://img-blog.csdnimg.cn/94f41eaea0fa4a9bae07507fc011eefa.png#pic_center)

```bash
#修改该首页文件内容
vim /usr/share/nginx/html/index.html
```

==让网页支持中文，文件首添加以下==
> < meta charset=utf8>

---

- 浅试一下拷贝淘宝主页信息

> 进入主页邮件空白进入查看页面源代码，复制拷贝
![在这里插入图片描述](https://img-blog.csdnimg.cn/5fde280c7c2c4d5cbbae5fd3d038637b.png)

>进入linux 用vim文件编辑器编辑/usr/share/nginx/html/index.html
用dG命令将文件内容全部删除
i进入编辑模式，粘贴刚才复制的页面源代码，保存并退出
回到浏览器刷新机器页面，得到
![在这里插入图片描述](https://img-blog.csdnimg.cn/44c9d23ef25e46e49a8af9e61a83b21e.png)
- 如果没有刷新没有显示或者无法访问
可能是nginx停止服务，打开就好了
> systemctl start nginx

---

<kbd>以上为学习总结经验，期待大家发现错误相互沟通学习经验