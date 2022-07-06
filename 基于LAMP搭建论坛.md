# 部署一个论坛disuz
- 下载论坛的压缩代码
> wget http://download.comsenz.com/DiscuzX/3.2/Discuz_X3.2_SC_UTF8.zip

- 解压缩代码包(没有zip可以先下载一个)
> 下载zip：yum install unzip -y
> 解压缩包:unzip Discuz_X3.2_SC_UTF8.zip

- 拷贝upload下代码到apache目录下，即可访问
(这里我的upload目录在/home/discuz/下)
> cp -r /home/discuz/upload/* /var/www/html

- 给与最高权限，便于测试
> chmod -R 777 /var/www/html/*

- 访问apache首页，查看是否进入论坛安装界面
