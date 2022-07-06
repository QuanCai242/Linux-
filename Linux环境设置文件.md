### Linux环境设置文件
>Linux环境变量按==作用域==可分为**系统环境变量**和**用户环境变量**。
### 永久有效
#### 一.系统环境中设置文件
系统环境变量==对该系统中的所有用户都有效==。

**方法**：登录环境设置文件：/etc/profile

<kbd>用**vim/gedit**在文件/etc/profile文件中增加变量，该变量将会对Linux下所有用户**永久有效**。</kbd>

export:设置或显示环境变量。
: export [-fnp][变量名称]=[变量设置值]

source:从当前shell会话中的文件读取和执行命令。
: source 文件名（可用 ==.== 代替 ==source==）
```bash
//打开文件(vim或者gedit都可)
vim/gedit /etc/profile
//任意处添加自定义PPSP变量
export PPSP=/root:/etc/alsa
//保存并返回 
//执行一遍文件，否则更新文件得在下次重进用户时才生效
source /etc/profile
//查看变量值
echo $PPSP
```

**结果**：
![note_one](https://img-blog.csdnimg.cn/1ac8fab55b3b44ef936bb483f5c98bbe.png#pic_center)


---

#### 二.用户环境中设置文件
用户环境变量只==对特定的用户有效==。

**方法**：登录环境设置文件：$ HOME/.bash_profile
非登录环境设置文件：$ HOME/.bashrc

<kbd>用**vim/gedit**在用户目录($HOME/~)下打开.bash_profile文件增减变量，该变量仅在当前用户中**永久有效**。</kbd>

```bash
//打开文件
vim/gedit ~/.bash_profile
//任意处添加自定义ASD变量
export ASD=/etc/alsa:/root
//保存并返回 
//执行一遍文件，否则更新文件得在下次重进用户时才生效
source ~/.bash_profile
//查看变量值
echo $ASD
```
**结果**：
![note_two](https://img-blog.csdnimg.cn/04143708be48461bbbc7d79cf6f76fe3.png#pic_center)

---

### 临时有效
> 使用export命令直接定义变量，该变量仅在当前shell有效，关闭shell之后再次打开则需要重新定义变量。

```bash
//格式
export 变量名=变量值
HOME=/root/bash
echo $HOME
export GOUP=/etc/alsa
echo $GOUP
```
**结果**：![在这里插入图片描述](https://img-blog.csdnimg.cn/9d65d2bb0d97432ca3e55790bd68c941.png#pic_center)

---

### 常见环境变量

> - **PATH**:指定命令的搜索路径
> <kbd>PATH声明用法：
> PATH=$PATH:< PATH1 >:< PATH2 >:< PATH3 >...< PATHn >
> export PATH
> echo $PATH
> </kbd>
>  - **HOME**:指定用户的主工作目录(即用户登录到Linux系统时默认的目录)。
> - **HISTSIZE**:保存历史命令记录的条数。
>  - **LOGNAME**:当前用户登录名。
>  - **HOSTNAME**:主机名称。诸多应用程序如果要用到主机名，通常从这个环境变量中取得。
>  - **SHELL**:当前用户用的是那种shell。
>  - **LANG/LANGUGE**:和语言相关的环境变量，使用多种语言的用户可以修改此环境变量。
>  - **MAIL**:当前用户的邮件存放目录。

<kbd>**注**:根据Linux版本不同，变量名称可能有所不同。</kbd>

---

### 查看与删除
- 查看环境变量的方法（我已知）如下：

```php
//以PWD为例 PWD=/etc/alsa
$PWD
cat $PWD
echo $PWD
printenv $PWD
set |grep $PWD
env |grep $PWD
```
**输出**：
![$PWD](https://img-blog.csdnimg.cn/d6e7bbdaaf454b0ab8fdad3e98f51a94.png#pic_center=30x30)

---
- 删除环境变量的方法：
使用unset命令，删除环境变量
```php
$PWD
unset PWD
$PWD
```

**结果**：![0](https://img-blog.csdnimg.cn/4a1b047c62af4648a0e0a76d908364f9.png#pic_center)

---
### 变量只读设置
- 使用readonly命令设置只读变量。
>如果使用了readonly命令的话，变量就不可以被修改或清除了。

![00](https://img-blog.csdnimg.cn/0fb0707b36db4c66b72a5342b905f3e7.png#pic_center)

