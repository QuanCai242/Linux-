
# 部署apache
- 清空防火墙规则
> iptables -F

```bash
#关闭防火墙
systemctl stop firewalld
systemctl disable firewalld
getenforce

#停止以及把nginx应用程序卸载(这一步可略)
yum remove nginx -y

#下载apache
yum install httpd
#启动apache
systemctl start httpd
```
- 如果出现报错：
可能是nginx没有停止服务
![在这里插入图片描述](https://img-blog.csdnimg.cn/0841c0b6e83449b6af61b5e38d836536.png)

```bash
#停止nginx服务
systemctl stop nginx

#重新启动apache
systemctl start httpd
```


- 回到浏览器，输入本机  ==ip:80== 进入如下页面即为顺利


![在这里插入图片描述](https://img-blog.csdnimg.cn/1c92110106444c84a532269e2c45164c.png)

---

# 部署mysql
- 安装
>yum install mariadb-server mariadb -y

- 启动
> systemctl start mariadb

- 验证mysql ，默认端口port为3306
> netstat -tunlp | grep mysql

- 使用，访问
> mysql -uroot -p
> （默认没有密码，直接回车即可）

**进入如下界面证明成功：**

![在这里插入图片描述](https://img-blog.csdnimg.cn/067baf766350485583cc07d767e83c8e.png#pic_center)

---

- 显示目前sql已安装的库
`show databases;`

```bash
MariaDB [(none)]> 
MariaDB [(none)]> show databases;
+--------------------+
| Database           |
+--------------------+
| information_schema |
| mysql              |
| performance_schema |
+--------------------+
3 rows in set (0.001 sec)
```

- 进入到mysql
`use mysql;`
```bash
MariaDB [(none)]> use mysql;
Reading table information for completion of table and column names
You can turn off this feature to get a quicker startup with -A

Database changed
MariaDB [mysql]>
```

- 查看数据表
`show tables;`

- 查询user表的信息(user表在mysql文件夹下)
`select user,password,host from user;`
```bash
MariaDB [mysql]> select user,password,host from user;
+------+----------+-----------------------+
| user | password | host                  |
+------+----------+-----------------------+
| root |          | localhost             |
| root |          | localhost.localdomain |
| root |          | 127.0.0.1             |
| root |          | ::1                   |
+------+----------+-----------------------+
4 rows in set (0.001 sec)
```

- 退出mysql
`exit`

```bash
MariaDB [mysql]> exit
Bye
```

---

# 部署PHP结合apache
- 解决php安装的依赖开发环境
```php
yum install -y zlib-devel libxml2-devel libjpeg-devel libjpeg-turbo-devel libiconv-devel freetype-devel libpng-devel gd-devel libcurl-devel libxslt-devel libtool-ltdl-devel pcre pcre-devel apr apr-devel zlib-devel gcc make
```
- 安装php以及php连接mysql数据库的驱动
`yum install php php-fpm php-mysql  -y`

**如果出现报错则换成**

`yum search php-mysql`

![在这里插入图片描述](https://img-blog.csdnimg.cn/68abdd0cc03f4a8eae43d285348fb3c5.png#pic_center)
> php不需要额外修改，但是需要修改apache配置文件让支持php
-php程序和apache结合工作

- 编辑apache配置文件
<kbd>vim /etc/httpd/conf/httpd.conf</kbd>

- 进行配置文件修改
 在文件中找到``Document Root``(大概在120行左右 显示行号:`set nu`)
编辑如下信息：
```bash
DocumentRoot "/var/www/html"
TypesConfig /etc/mime.types
        AddType application/x-httpd-php .php
        AddType application/x-httpd-php-source .phps
        DirectoryIndex index.php index.html
```
- 编辑php脚本，看apache能否正常运行
脚本放置位置：
`vim /var/www/html/index.php`

**输入：**
```bash
<meta cherset=utf8>
大家下午好～
<?php
phpinfo();
?>
```
- 保存退出重启apach服务和php进程
`systemctl start php-fpm`
`systemctl restart httpd`

- 访问页面
>回到浏览器
输入本机  ==ip:80== 进入如下页面即为顺利
![在这里插入图片描述](https://img-blog.csdnimg.cn/bdd13297dd194569b50452d3f08984fa.png)
