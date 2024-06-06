---
date: 2024-06-03
title: Linux 常用命令纪录
tags:
  - Linux
description: 摘要：本文主要介绍Linux上比较常用的命令及用法。
---

# Linux 
#  背景  
最近在使用Linux的时候用到很多命令，熟练掌握这些会让Linux的操作事半功倍，在这里做一些主要的纪录。  

# 概览
ssh - 用于远程登录到其他主机。  
scp - 用于在不同主机之间安全地复制文件。  
cd - 用于改变当前工作目录。  
pwd - 用于显示当前工作目录的完整路径。  
mv - 用于移动或重命名文件和目录。  
chmod - 用于管理文件和目录的权限。  
passwd - 用于修改用户密码。  
usermod - 用于修改用户账户的属性。  
df - 用于显示文件系统的磁盘空间使用情况。  
netstat - 用于显示网络连接、路由表和网络接口信息。  
vi - 用于编辑文本文件。  
chgrp - 用于修改文件或目录的所属组。  
chown - 用于修改文件或目录的所有者。  
du - 用于估计文件或目录的磁盘使用空间。  
ps - 用于显示当前进程的状态。  
top - 用于实时显示系统中运行的进程及其资源使用情况。  
grep - 用于在文件中搜索指定的字符串。  
cat - 用于连接文件并在标准输出上显示。  
less - 用于分页查看文件内容。  
tail - 用于显示文件的末尾部分。  
ip - 用于显示或管理路由、网络设备、接口和隧道。  
ifconfig - 用于配置网络接口。  
ping - 用于测试与另一台主机的网络连接。  
traceroute - 用于追踪数据包到达目标主机所经过的路由。  
crontab - 用于创建、编辑或删除定时任务。  
tar - 用于归档文件。  
gzip - 用于压缩或解压缩文件。  
systemctl - 用于管理systemd系统和服务。  
service - 用于运行System V init脚本。  
firewall-cmd - 用于管理firewalld防火墙。  
iptables - 用于配置Linux内核防火墙。  
lsblk - 用于列出所有可用的块设备。  
hostnamectl - 用于查询或更改系统主机名。  
initialize-tsm - 用于初始化IBM Tivoli Storage Manager (TSM)。  
cp - 用于复制文件和目录。  
export - 用于设置环境变量。  
openssl - 用于生成和管理密钥、证书等密码学相关的内容。  
yum - 用于从yum存储库安装、更新、删除软件包。  
pip - 用于安装和管理Python的软件包。  
useradd/adduser - 用于创建新的Linux用户。  
echo - 用于在控制台输出字符串,也常用于将字符串重定向到文件中。  
ntpdate/chronyd - 用于配置和同步NTP时间服务。  
ulimit - 用于控制进程可以使用的系统资源,如打开的文件数量、可用内存大小等。  

以下脚本，从https://github.com/jaywcjlove/linux-command  
这个库的command文件夹提取命令的说明，特此感谢，

这是运行脚本的目录结构，1.md就是上面命令的集合：
```
your_directory/
│
├── 1.md
├── command/
│   ├── ssh.md
│   ├── scp.md
│   ├── cd.md
│   └── ...其他命令文件...
└── merge_script.py
```

```
import os
import re

# 1. 读取1.md文件内容，并提取所有命令
def extract_commands(file_path):
    with open(file_path, 'r', encoding='utf-8') as file:
        content = file.read()
        
    # 提取命令正则表达式，假设命令是单个单词
    commands = re.findall(r'(\w+) - 用于', content)
    return commands

# 2. 从command文件夹中找到对应的文件并合并
def merge_command_files(commands, command_folder):
    merged_content = []
    
    for cmd in commands:
        cmd_file_path = os.path.join(command_folder, f'{cmd}.md')
        
        if os.path.isfile(cmd_file_path):
            with open(cmd_file_path, 'r', encoding='utf-8') as cmd_file:
                merged_content.append(cmd_file.read())
        else:
            print(f"Warning: {cmd_file_path} not found")
    
    return merged_content

# 3. 写入合并后的文件内容到一个新的md文件
def write_merged_file(output_file, merged_content):
    with open(output_file, 'w', encoding='utf-8') as output:
        for content in merged_content:
            output.write(content + '\n')

# 主函数
def main():
    # 文件路径
    source_file = '1.md'
    command_folder = 'command'
    output_file = 'merged_commands.md'
    
    # 提取命令
    commands = extract_commands(source_file)
    
    # 合并命令文件内容
    merged_content = merge_command_files(commands, command_folder)
    
    # 写入合并后的文件
    write_merged_file(output_file, merged_content)
    
    print(f"Merged file created: {output_file}")

if __name__ == '__main__':
    main()
```

# 详细说明

ssh
===

openssh套件中的客户端连接工具

## 补充说明

**ssh命令** 是openssh套件中的客户端连接工具，可以给予ssh加密协议实现安全的远程登录服务器。

### 语法

```shell
ssh(选项)(参数)
```

### 选项

```shell
-1：强制使用ssh协议版本1；
-2：强制使用ssh协议版本2；
-4：强制使用IPv4地址；
-6：强制使用IPv6地址；
-A：开启认证代理连接转发功能；
-a：关闭认证代理连接转发功能；
-b：使用本机指定地址作为对应连接的源ip地址；
-C：请求压缩所有数据；
-F：指定ssh指令的配置文件；
-f：后台执行ssh指令；
-g：允许远程主机连接主机的转发端口；
-i：指定身份(私钥)文件；
-l：指定连接远程服务器登录用户名；
-N：不执行远程指令；
-o：指定配置选项；
-p：指定远程服务器上的端口；
-q：静默模式；
-X：开启X11转发功能；
-x：关闭X11转发功能；
-y：开启信任X11转发功能。
```

### 参数

* 远程主机：指定要连接的远程ssh服务器；
* 指令：要在远程ssh服务器上执行的指令。

### 实例

```shell
# ssh 用户名@远程服务器地址
ssh user1@172.24.210.101
# 指定端口
ssh -p 2211 root@140.206.185.170

# ssh 大家族
ssh -p 22 user@ip  # 默认用户名为当前用户名，默认端口为 22
ssh-keygen # 为当前用户生成 ssh 公钥 + 私钥
ssh-keygen -f keyfile -i -m key_format -e -m key_format # key_format: RFC4716/SSH2(default) PKCS8 PEM
ssh-copy-id user@ip:port # 将当前用户的公钥复制到需要 ssh 的服务器的 ~/.ssh/authorized_keys，之后可以免密登录
```

连接远程服务器

```shell
ssh username@remote_host
```

连接远程服务器并指定端口

```shell
ssh -p port username@remote_host
```

使用密钥文件连接远程服务器

```shell
ssh -i path/to/private_key username@remote_host
```

在本地执行远程命令

```shell
ssh username@remote_host "command"
```

在本地复制文件到远程服务器

```shell
scp path/to/local_file username@remote_host:/path/to/remote_directory
```

在远程服务器复制文件到本地

```shell
scp username@remote_host:/path/to/remote_file path/to/local_directory
```

在本地端口转发到远程服务器

```shell
ssh -L local_port:remote_host:remote_port username@remote_host
```

在远程服务器端口转发到本地

```shell
ssh -R remote_port:local_host:local_port username@remote_host
```




scp
===

加密的方式在本地主机和远程主机之间复制文件

## 补充说明

**scp命令** 用于在Linux下进行远程拷贝文件的命令，和它类似的命令有cp，不过cp只是在本机进行拷贝不能跨服务器，而且scp传输是加密的。可能会稍微影响一下速度。当你服务器硬盘变为只读read only system时，用scp可以帮你把文件移出来。另外，scp还非常不占资源，不会提高多少系统负荷，在这一点上，rsync就远远不及它了。虽然 rsync比scp会快一点，但当小文件众多的情况下，rsync会导致硬盘I/O非常高，而scp基本不影响系统正常使用。

###  语法

```shell
scp(选项)(参数)
```

###  选项

```shell
-1：使用ssh协议版本1；
-2：使用ssh协议版本2；
-4：使用ipv4；
-6：使用ipv6；
-B：以批处理模式运行；
-C：使用压缩；
-F：指定ssh配置文件；
-i：identity_file 从指定文件中读取传输时使用的密钥文件（例如亚马逊云pem），此参数直接传递给ssh；
-l：指定宽带限制；
-o：指定使用的ssh选项；
-P：指定远程主机的端口号；
-p：保留文件的最后修改时间，最后访问时间和权限模式；
-q：不显示复制进度；
-r：以递归方式复制。
```

###  参数

* 源文件：指定要复制的源文件。
* 目标文件：目标文件。格式为`user@host：filename`（文件名为目标文件的名称）。

###  实例

从远程复制到本地的scp命令与上面的命令雷同，只要将从本地复制到远程的命令后面2个参数互换顺序就行了。

 **从远程机器复制文件到本地目录** 

```shell
scp root@10.10.10.10:/opt/soft/nginx-0.5.38.tar.gz /opt/soft/
```

从10.10.10.10机器上的`/opt/soft/`的目录中下载nginx-0.5.38.tar.gz 文件到本地`/opt/soft/`目录中。

**从亚马逊云复制OpenVPN到本地目录** 

```shell
scp -i amazon.pem ubuntu@10.10.10.10:/usr/local/openvpn_as/etc/exe/openvpn-connect-2.1.3.110.dmg openvpn-connect-2.1.3.110.dmg
```
从10.10.10.10机器上下载openvpn安装文件到本地当前目录来。

 **从远程机器复制到本地** 

```shell
scp -r root@10.10.10.10:/opt/soft/mongodb /opt/soft/
```

从10.10.10.10机器上的`/opt/soft/`中下载mongodb目录到本地的`/opt/soft/`目录来。

 **上传本地文件到远程机器指定目录** 

```shell
scp /opt/soft/nginx-0.5.38.tar.gz root@10.10.10.10:/opt/soft/scptest
# 指定端口 2222
scp -rp -P 2222 /opt/soft/nginx-0.5.38.tar.gz root@10.10.10.10:/opt/soft/scptest
```

复制本地`/opt/soft/`目录下的文件nginx-0.5.38.tar.gz到远程机器10.10.10.10的`opt/soft/scptest`目录。

 **上传本地目录到远程机器指定目录** 

```shell
scp -r /opt/soft/mongodb root@10.10.10.10:/opt/soft/scptest
```

上传本地目录`/opt/soft/mongodb`到远程机器10.10.10.10上`/opt/soft/scptest`的目录中去。




cd
===

切换用户当前工作目录。

## 概要

```shell
cd [-L|[-P [-e]]] [dir]
```

## 主要用途

- 切换工作目录至`dir`。其中`dir`的表示法可以是绝对路径或相对路径。
- 若参数`dir`省略，则默认为使用者的shell变量`HOME`。
- 如果`dir`指定为`~`时表示为使用者的shell变量`HOME`，`.`表示当前目录，`..`表示当前目录的上一级目录。
- 环境变量`CDPATH`是由冒号分割的一到多个目录，你可以将常去的目录的上一级加入到`CDPATH`以便方便访问它们；如果`dir`以`/`开头那么`CDPATH`不会被使用。
- 当`shopt`选项`cdable_vars`打开时，如果`dir`在`CDPATH`及当前目录下均不存在，那么会把它当作变量，读取它的值作为要进入的目录。

## 参数

dir（可选）：指定要切换到的目录。

## 选项

```shell
-L （默认值）如果要切换到的目标目录是一个符号连接，那么切换到符号连接的目录。
-P 如果要切换到的目标目录是一个符号连接，那么切换到它指向的物理位置目录。
-  当前工作目录将被切换到环境变量OLDPWD所表示的目录，也就是前一个工作目录。
```

## 返回值

返回状态为成功除非无法进入指定的目录。

## 例子

```shell
cd    # 进入用户主目录；
cd /  # 进入根目录
cd ~  # 进入用户主目录；
cd ..  # 返回上级目录（若当前目录为“/“，则执行完后还在“/"；".."为上级目录的意思）；
cd ../..  # 返回上两级目录；
cd !$  # 把上个命令的参数作为cd参数使用。
```

关于切换到上一个工作目录的说明

```shell
cd -
# 命令会首先显示要切换到的目标目录，然后再进入。
cd ${OLDPWD}
# 命令会直接切换到上一个工作目录。
```

关于`CDPATH`

```shell
# 设置桌面文件夹作为CDPATH的值。
CDPATH='~/Desktop'
# 假设我们接下来要演示涉及到的路径~和~/Desktop下没有test3文件夹，现在新建它们。
mkdir ~/test3
mkdir ~/Desktop/test3
# 进入~目录。
cd ~
# 进入test3目录。
cd test3
# 执行后显示~/Desktop/test3并进入该目录，而不是~目录的test3目录。
# 如果CDPATH存在值，那么优先在CDPATH中查找并进入第一个匹配成功的，如果全部失败那么最后尝试当前目录。
```

关于`cdable_vars`

```shell
# 打开选项。
shopt -s cdable_vars
# 假设当前路径以及CDPATH没有名为new_var的目录。
new_var='~/Desktop'
# 尝试进入。
cd new_var
# 关闭选项。
shopt -u cdable_vars
```

### 注意

1. 该命令是bash内建命令，相关的帮助信息请查看`help`命令。

2. 建议您在编写脚本的过程中如有必要使用`cd`命令时，请增加必要的注释以用于提醒阅读者当前工作目录，以免出现诸如`找不到文件`这类问题的发生。



pwd
===

显示当前工作目录的绝对路径。

## 补充说明

pwd（英文全拼：print working directory） 命令用于显示用户当前所在的工作目录（以绝对路径显示）。

## 内建命令

#### 概要

```shell
pwd [-LP]
```

#### 选项

```shell
-L （默认值）打印环境变量"$PWD"的值，可能为符号链接。
-P 打印当前工作目录的物理位置。
```

#### 返回值

返回状态为成功除非给出了非法选项或是当前目录无法读取。

#### 注意

1. 该命令是bash内建命令，相关的帮助信息请查看`help`命令。


## 外部命令

#### 概要

```shell
pwd [OPTION]...
```

#### 主要用途

- 显示当前工作目录。


#### 选项

```shell
-L, --logical 打印环境变量"$PWD"的值，可能为符号链接。
-P, --physical （默认值）打印当前工作目录的物理位置。
--help 显示帮助信息并退出。
--version 显示版本信息并退出。
```

#### 返回值

返回状态为成功除非给出了非法选项或是当前目录无法读取。

#### 注意

1. 该命令是`GNU coreutils`包中的命令，相关的帮助信息请查看`man pwd`或`info coreutils 'pwd invocation'`。
2. 启动或关闭内建命令请查看`enable`命令，关于同名优先级的问题请查看`builtin`命令的例子部分的相关讨论。
3. 在不禁用内建且当前环境没有定义`pwd`函数的情况下，使用`/usr/bin/pwd`指向`coreutils`的`pwd`，使用`pwd`指向bash内建的`pwd`。


## 例子

查看当前所在路径

```shell
[root@localhost var]# pwd
/var
```

显示软连接文件最终指向的文件路径

```shell
[root@localhost ~]# cd /var/   # 进入/var目录，该目录下有个 mail 软连接文件
[root@localhost var]# ls -al
total 164
...
lrwxrwxrwx  1 root root   10 Oct 17  2015 mail -> spool/mail

[root@localhost var]# cd mail/   # 进入 mail 目录，mail 为连接文件。
[root@localhost mail]# pwd       # 默认，使用连接文件，直接显示连接文件全路径。
/var/mail
```

使用 `-P` 参数，显示的不是逻辑路径，而是连接(软连接)文件最终指向的文件

```shell
[root@localhost mail]# pwd -P    
/var/spool/mail
```

mv
===

用来对文件或目录重新命名

## 补充说明

**mv命令** 用来对文件或目录重新命名，或者将文件从一个目录移到另一个目录中。source表示源文件或目录，target表示目标文件或目录。如果将一个文件移到一个已经存在的目标文件中，则目标文件的内容将被覆盖。

mv命令可以用来将源文件移至一个目标文件中，或将一组文件移至一个目标目录中。源文件被移至目标文件有两种不同的结果：

1.  如果目标文件是到某一目录文件的路径，源文件会被移到此目录下，且文件名不变。
2.  如果目标文件不是目录文件，则源文件名（只能有一个）会变为此目标文件名，并覆盖己存在的同名文件。如果源文件和目标文件在同一个目录下，mv的作用就是改文件名。当目标文件是目录文件时，源文件或目录参数可以有多个，则所有的源文件都会被移至目标文件中。所有移到该目录下的文件都将保留以前的文件名。

注意事项：mv与cp的结果不同，mv好像文件“搬家”，文件个数并未增加。而cp对文件进行复制，文件个数增加了。

###  语法 

```shell
mv(选项)(参数)
```

###  选项 

```shell
--backup=<备份模式>：若需覆盖文件，则覆盖前先行备份；
-b：当文件存在时，覆盖前，为其创建一个备份；
-f：若目标文件或目录与现有的文件或目录重复，则直接覆盖现有的文件或目录；
-i：交互式操作，覆盖前先行询问用户，如果源文件与目标文件或目标目录中的文件同名，则询问用户是否覆盖目标文件。用户输入”y”，表示将覆盖目标文件；输入”n”，表示取消对源文件的移动。这样可以避免误将文件覆盖。
--strip-trailing-slashes：删除源文件中的斜杠“/”；
-S<后缀>：为备份文件指定后缀，而不使用默认的后缀；
--target-directory=<目录>：指定源文件要移动到目标目录；
-u：当源文件比目标文件新或者目标文件不存在时，才执行移动操作。
```

###  参数 

*   源文件：源文件列表。
*   目标文件：如果“目标文件”是文件名则在移动文件的同时，将其改名为“目标文件”；如果“目标文件”是目录名则将源文件移动到“目标文件”下。

###  实例 

将目录`/usr/men`中的所有文件移到当前目录（用`.`表示）中：

```shell
mv /usr/men/* .
```

移动文件

```shell
mv file_1.txt /home/office/
```

移动多个文件

```shell
mv file_2.txt file_3.txt file_4.txt /home/office/
mv *.txt /home/office/
```

移动目录

```shell
mv directory_1/ /home/office/
```

重命名文件或目录

```shell
mv file_1.txt file_2.txt # 将文件file_1.txt改名为file_2.txt
```

重命名目录

```shell
mv directory_1/ directory_2/
```

打印移动信息

```shell
mv -v *.txt /home/office
```

提示是否覆盖文件

```shell
mv -i file_1.txt /home/office
```

源文件比目标文件新时才执行更新

```shell
mv -uv *.txt /home/office
```

不要覆盖任何已存在的文件

```shell
mv -vn *.txt /home/office
```

复制时创建备份

```shell
mv -bv *.txt /home/office
```

无条件覆盖已经存在的文件

```shell
mv -f *.txt /home/office
```



chmod
===

用来变更文件或目录的权限

## 概要

```shell
chmod [OPTION]... MODE[,MODE]... FILE...
chmod [OPTION]... OCTAL-MODE FILE...
chmod [OPTION]... --reference=RFILE FILE...
```

## 主要用途

- 通过符号组合的方式更改目标文件或目录的权限。
- 通过八进制数的方式更改目标文件或目录的权限。
- 通过参考文件的权限来更改目标文件或目录的权限。

## 参数

mode：八进制数或符号组合。

file：指定要更改权限的一到多个文件。

## 选项 

```shell
-c, --changes：当文件的权限更改时输出操作信息。
--no-preserve-root：不将'/'特殊化处理，默认选项。
--preserve-root：不能在根目录下递归操作。
-f, --silent, --quiet：抑制多数错误消息的输出。
-v, --verbose：无论文件是否更改了权限，一律输出操作信息。
--reference=RFILE：使用参考文件或参考目录RFILE的权限来设置目标文件或目录的权限。
-R, --recursive：对目录以及目录下的文件递归执行更改权限操作。
--help：显示帮助信息并退出。
--version：显示版本信息并退出。
```

## 返回值

返回状态为成功除非给出了非法选项或非法参数。

## 例子 

> 参考`man chmod`文档的`DESCRIPTION`段落得知：
> - `u`符号代表当前用户。
> - `g`符号代表和当前用户在同一个组的用户，以下简称组用户。
> - `o`符号代表其他用户。
> - `a`符号代表所有用户。
> - `r`符号代表读权限以及八进制数`4`。
> - `w`符号代表写权限以及八进制数`2`。
> - `x`符号代表执行权限以及八进制数`1`。
> - `X`符号代表如果目标文件是可执行文件或目录，可给其设置可执行权限。
> - `s`符号代表设置权限suid和sgid，使用权限组合`u+s`设定文件的用户的ID位，`g+s`设置组用户ID位。
> - `t`符号代表只有目录或文件的所有者才可以删除目录下的文件。
> - `+`符号代表添加目标用户相应的权限。
> - `-`符号代表删除目标用户相应的权限。
> - `=`符号代表添加目标用户相应的权限，删除未提到的权限。

```shell
linux文件的用户权限说明：

# 查看当前目录（包含隐藏文件）的长格式。
ls -la
  -rw-r--r--   1 user  staff   651 Oct 12 12:53 .gitmodules

# 第1位如果是d则代表目录，是-则代表普通文件。
# 更多详情请参阅info coreutils 'ls invocation'（ls命令的info文档）的'-l'选项部分。
# 第2到4位代表当前用户的权限。
# 第5到7位代表组用户的权限。
# 第8到10位代表其他用户的权限。
```

```shell
# 添加组用户的写权限。
chmod g+w ./test.log
# 删除其他用户的所有权限。
chmod o= ./test.log
# 使得所有用户都没有写权限。
chmod a-w ./test.log
# 当前用户具有所有权限，组用户有读写权限，其他用户只有读权限。
chmod u=rwx, g=rw, o=r ./test.log
# 等价的八进制数表示：
chmod 764 ./test.log
# 将目录以及目录下的文件都设置为所有用户拥有读写权限。
# 注意，使用'-R'选项一定要保留当前用户的执行和读取权限，否则会报错！
chmod -R a=rw ./testdir/
# 根据其他文件的权限设置文件权限。
chmod --reference=./1.log  ./test.log
```

### 注意

1. 该命令是`GNU coreutils`包中的命令，相关的帮助信息请查看`man chmod`或`info coreutils 'chmod invocation'`。

2. 符号连接的权限无法变更，如果用户对符号连接修改权限，其改变会作用在被连接的原始文件。

3. 使用`-R`选项一定要保留当前用户的执行和读取权限，否则会报错！



passwd
===

用于让用户可以更改自己的密码

## 补充说明

**passwd命令** 用于设置用户的认证信息，包括用户密码、密码过期时间等。系统管理者则能用它管理系统用户的密码。只有管理者可以指定用户名称，一般用户只能变更自己的密码。

###  语法

```shell
passwd(选项)(参数)
```

###  选项

```shell
-d：删除密码，仅有系统管理者才能使用；
-f：强制执行；
-k：设置只有在密码过期失效后，方能更新；
-l：锁住密码；
-s：列出密码的相关信息，仅有系统管理者才能使用；
-u：解开已上锁的帐号。
```

###  参数

用户名：需要设置密码的用户名。

###  知识扩展

与用户、组账户信息相关的文件

存放用户信息：

```shell
/etc/passwd
/etc/shadow
```

存放组信息：

```shell
/etc/group
/etc/gshadow
```

用户信息文件分析（每项用`:`隔开）

```shell
例如：jack:X:503:504:::/home/jack/:/bin/bash
jack　　# 用户名
X　　# 口令、密码
503　　# 用户id（0代表root、普通新建用户从500开始）
504　　# 所在组
:　　# 描述
/home/jack/　　# 用户主目录
/bin/bash　　# 用户缺省Shell
```

组信息文件分析

```shell
例如：jack:$!$:???:13801:0:99999:7:*:*:
jack　　# 组名
$!$　　# 被加密的口令
13801　　# 创建日期与今天相隔的天数
0　　# 口令最短位数
99999　　# 用户口令
7　　# 到7天时提醒
*　　# 禁用天数
*　　# 过期天数
```

###  实例

如果是普通用户执行passwd只能修改自己的密码。如果新建用户后，要为新用户创建密码，则用passwd用户名，注意要以root用户的权限来创建。

```shell
[root@localhost ~]# passwd linuxde     # 更改或创建linuxde用户的密码；
Changing password for user linuxde.
New UNIX password:           # 请输入新密码；
Retype new UNIX password:    # 再输入一次；
passwd: all authentication tokens updated successfully.  # 成功；
```

普通用户如果想更改自己的密码，直接运行passwd即可，比如当前操作的用户是linuxde。

```shell
[linuxde@localhost ~]$ passwd
Changing password for user linuxde.  # 更改linuxde用户的密码；
(current) UNIX password:    # 请输入当前密码；
New UNIX password:          # 请输入新密码；
Retype new UNIX password:   # 确认新密码；
passwd: all authentication tokens updated successfully.  # 更改成功；
```

比如我们让某个用户不能修改密码，可以用`-l`选项来锁定：

```shell
[root@localhost ~]# passwd -l linuxde     # 锁定用户linuxde不能更改密码；
Locking password for user linuxde.
passwd: Success            # 锁定成功；

[linuxde@localhost ~]# su linuxde    # 通过su切换到linuxde用户；
[linuxde@localhost ~]$ passwd       # linuxde来更改密码；
Changing password for user linuxde.
Changing password for linuxde
(current) UNIX password:           # 输入linuxde的当前密码；
passwd: Authentication token manipulation error      # 失败，不能更改密码；
```

再来一例：

```shell
[root@localhost ~]# passwd -d linuxde   # 清除linuxde用户密码；
Removing password for user linuxde.
passwd: Success                          # 清除成功；

[root@localhost ~]# passwd -S linuxde     # 查询linuxde用户密码状态；
Empty password.                          # 空密码，也就是没有密码；
```

注意：当我们清除一个用户的密码时，登录时就无需密码，这一点要加以注意。



usermod
===

用于修改用户的基本信息

## 补充说明

**usermod命令** 用于修改用户的基本信息。usermod 命令不允许你改变正在线上的使用者帐号名称。当 usermod 命令用来改变user id，必须确认这名user没在电脑上执行任何程序。你需手动更改使用者的 crontab 档。也需手动更改使用者的 at 工作档。采用 NIS server 须在server上更动相关的NIS设定。

### 语法

```shell
usermod(选项)(参数)
```

### 选项

```shell
-c<备注>：修改用户帐号的备注文字；
-d<登入目录>：修改用户登入时的目录，只是修改/etc/passwd中用户的家目录配置信息，不会自动创建新的家目录，通常和-m一起使用；
-m<移动用户家目录>:移动用户家目录到新的位置，不能单独使用，一般与-d一起使用。
-e<有效期限>：修改帐号的有效期限；
-f<缓冲天数>：修改在密码过期后多少天即关闭该帐号；
-g<群组>：修改用户所属的群组；
-G<群组>；修改用户所属的附加群组；
-l<帐号名称>：修改用户帐号名称；
-L：锁定用户密码，使密码无效；
-s<shell>：修改用户登入后所使用的shell；
-u<uid>：修改用户ID；
-U:解除密码锁定。
```

### 参数

登录名：指定要修改信息的用户登录名。

### 实例

将 newuser2 添加到组 staff 中：

```shell
usermod -G staff newuser2
```

修改newuser的用户名为newuser1：

```shell
usermod -l newuser1 newuser
```

锁定账号newuser1：

```shell
usermod -L newuser1
```

解除对newuser1的锁定：

```shell
usermod -U newuser1
```

增加用户到用户组中:

```shell
apk add shadow # 安装 shadow 包, usermod 命令包含在 usermod 中
usermod -aG group user # 添加用户到用户组中
```

`-a` 参数表示附加，只和 `-G` 参数一同使用，表示将用户增加到组中。

修改用户家目录：
```
[root@node-1 ~]# useradd lutixiaya
[root@node-1 ~]# ls /home
lutixiaya
[root@node-1 ~]# usermod -md /data/new_home lutixiaya
[root@node-1 ~]# ls /home/
[root@node-1 ~]# ls /data/
new_home
```



df
===

显示磁盘的相关信息

## 补充说明

**df命令** 用于显示磁盘分区上的可使用的磁盘空间。默认显示单位为KB。可以利用该命令来获取硬盘被占用了多少空间，目前还剩下多少空间等信息。

###  语法 

```shell
df(选项)(参数)
```

###  选项 

```shell
-a或--all：包含全部的文件系统；
--block-size=<区块大小>：以指定的区块大小来显示区块数目；
-h或--human-readable：以可读性较高的方式来显示信息；
-H或--si：与-h参数相同，但在计算时是以1000 Bytes为换算单位而非1024 Bytes；
-i或--inodes：显示inode的信息；
-k或--kilobytes：指定区块大小为1024字节；
-l或--local：仅显示本地端的文件系统；
-m或--megabytes：指定区块大小为1048576字节；
--no-sync：在取得磁盘使用信息前，不要执行sync指令，此为预设值；
-P或--portability：使用POSIX的输出格式；
--sync：在取得磁盘使用信息前，先执行sync指令；
-t<文件系统类型>或--type=<文件系统类型>：仅显示指定文件系统类型的磁盘信息；
-T或--print-type：显示文件系统的类型；
-x<文件系统类型>或--exclude-type=<文件系统类型>：不要显示指定文件系统类型的磁盘信息；
--help：显示帮助；
--version：显示版本信息。
```

###  参数 

文件：指定文件系统上的文件。

### 大小格式

显示值以 `--block-size` 和 `DF_BLOCK_SIZE`，`BLOCK_SIZE` 和 `BLOCKSIZE` 环境变量中的第一个可用 `SIZE` 为单位。 否则，单位默认为 `1024` 个字节（如果设置 `POSIXLY_CORRECT`，则为`512`）。

SIZE是一个整数和可选单位（例如：10M是10 * 1024 * 1024）。 单位是K，M，G，T，P，E，Z，Y（1024的幂）或KB，MB，...（1000的幂）。

###  实例 

查看系统磁盘设备，默认是KB为单位：

```shell
[root@LinServ-1 ~]# df
文件系统               1K-块        已用     可用 已用% 挂载点
/dev/sda2            146294492  28244432 110498708  21% /
/dev/sda1              1019208     62360    904240   7% /boot
tmpfs                  1032204         0   1032204   0% /dev/shm
/dev/sdb1            2884284108 218826068 2518944764   8% /data1
```

使用`-h`选项以KB以上的单位来显示，可读性高：

```shell
[root@LinServ-1 ~]# df -h
文件系统              容量  已用 可用 已用% 挂载点
/dev/sda2             140G   27G  106G  21% /
/dev/sda1             996M   61M  884M   7% /boot
tmpfs                1009M     0 1009M   0% /dev/shm
/dev/sdb1             2.7T  209G  2.4T   8% /data1
```

查看全部文件系统：

```shell
[root@LinServ-1 ~]# df -a
文件系统               1K-块        已用     可用 已用% 挂载点
/dev/sda2            146294492  28244432 110498708  21% /
proc                         0         0         0   -  /proc
sysfs                        0         0         0   -  /sys
devpts                       0         0         0   -  /dev/pts
/dev/sda1              1019208     62360    904240   7% /boot
tmpfs                  1032204         0   1032204   0% /dev/shm
/dev/sdb1            2884284108 218826068 2518944764   8% /data1
none                         0         0         0   -  /proc/sys/fs/binfmt_misc
```

显示 `public` 目录中的可用空间量，如以下输出中所示：

```shell
df public
# Filesystem     1K-blocks     Used Available Use% Mounted on
# /dev/loop0      18761008 15246924   2554392  86% /d Avail
```




netstat
===

查看Linux中网络系统状态信息

## 补充说明

**netstat命令** 用来打印Linux中网络系统的状态信息，可让你得知整个Linux系统的网络情况。

###  语法 

```shell
netstat(选项)
```

###  选项 

```shell
-a或--all：显示所有连线中的Socket；
-A<网络类型>或--<网络类型>：列出该网络类型连线中的相关地址；
-c或--continuous：持续列出网络状态；
-C或--cache：显示路由器配置的快取信息；
-e或--extend：显示网络其他相关信息；
-F或--fib：显示FIB；
-g或--groups：显示多重广播功能群组组员名单；
-h或--help：在线帮助；
-i或--interfaces：显示网络界面信息表单；
-l或--listening：显示监控中的服务器的Socket；
-M或--masquerade：显示伪装的网络连线；
-n或--numeric：直接使用ip地址，而不通过域名服务器；
-N或--netlink或--symbolic：显示网络硬件外围设备的符号连接名称；
-o或--timers：显示计时器；
-p或--programs：显示正在使用Socket的程序识别码和程序名称；
-r或--route：显示Routing Table；
-s或--statistice：显示网络工作信息统计表；
-t或--tcp：显示TCP传输协议的连线状况；
-u或--udp：显示UDP传输协议的连线状况；
-v或--verbose：显示指令执行过程；
-V或--version：显示版本信息；
-w或--raw：显示RAW传输协议的连线状况；
-x或--unix：此参数的效果和指定"-A unix"参数相同；
--ip或--inet：此参数的效果和指定"-A inet"参数相同。
```

###  实例 

 **列出所有端口 (包括监听和未监听的)** 

```shell
netstat -a     #列出所有端口
netstat -at    #列出所有tcp端口
netstat -au    #列出所有udp端口                             
```

 **列出所有处于监听状态的 Sockets** 

```shell
netstat -l        #只显示监听端口
netstat -lt       #只列出所有监听 tcp 端口
netstat -lu       #只列出所有监听 udp 端口
netstat -lx       #只列出所有监听 UNIX 端口
```

 **显示每个协议的统计信息** 

```shell
netstat -s   显示所有端口的统计信息
netstat -st   显示TCP端口的统计信息
netstat -su   显示UDP端口的统计信息

​```shell

 **在netstat输出中显示 PID 和进程名称** 

​```shell
netstat -pt
```

`netstat -p`可以与其它开关一起使用，就可以添加“PID/进程名称”到netstat输出中，这样debugging的时候可以很方便的发现特定端口运行的程序。

 **在netstat输出中不显示主机，端口和用户名(host, port or user)** 

当你不想让主机，端口和用户名显示，使用`netstat -n`。将会使用数字代替那些名称。同样可以加速输出，因为不用进行比对查询。

```shell
netstat -an
```

如果只是不想让这三个名称中的一个被显示，使用以下命令:

```shell
netsat -a --numeric-ports
netsat -a --numeric-hosts
netsat -a --numeric-users
```

 **持续输出netstat信息** 

```shell
netstat -c   #每隔一秒输出网络信息
```

 **显示系统不支持的地址族(Address Families)** 

```shell
netstat --verbose
```

在输出的末尾，会有如下的信息：

```shell
netstat: no support for `AF IPX' on this system.
netstat: no support for `AF AX25' on this system.
netstat: no support for `AF X25' on this system.
netstat: no support for `AF NETROM' on this system.
```

 **显示核心路由信息** 

```shell
netstat -r
```

使用`netstat -rn`显示数字格式，不查询主机名称。

 **找出程序运行的端口** 

并不是所有的进程都能找到，没有权限的会不显示，使用 root 权限查看所有的信息。

```shell
netstat -ap | grep ssh
```

找出运行在指定端口的进程：

```shell
netstat -an | grep ':80'
```

 **通过端口找进程ID**

```bash
netstat -anp|grep 8081 | grep LISTEN|awk '{printf $7}'|cut -d/ -f1
```

 **显示网络接口列表** 

```shell
netstat -i
```

显示详细信息，像是ifconfig使用`netstat -ie`。

 **IP和TCP分析** 

查看连接某服务端口最多的的IP地址：

```shell
netstat -ntu | grep :80 | awk '{print $5}' | cut -d: -f1 | awk '{++ip[$1]} END {for(i in ip) print ip[i],"\t",i}' | sort -nr
```

TCP各种状态列表：

```shell
netstat -nt | grep -e 127.0.0.1 -e 0.0.0.0 -e ::: -v | awk '/^tcp/ {++state[$NF]} END {for(i in state) print i,"\t",state[i]}'
```

查看phpcgi进程数，如果接近预设值，说明不够用，需要增加：

```shell
netstat -anpo | grep "php-cgi" | wc -l
```

## 扩展知识 

### 网络连接状态详解

**共有12中可能的状态**，前面11种是按照TCP连接建立的三次握手和TCP连接断开的四次挥手过程来描述的：

1. LISTEN：首先服务端需要打开一个socket进行监听，状态为 LISTEN，侦听来自远方TCP端口的连接请求 ；

2. SYN_SENT：客户端通过应用程序调用connect进行active open，于是客户端tcp发送一个SYN以请求建立一个连接，之后状态置为 SYN_SENT，在发送连接请求后等待匹配的连接请求；

3. SYN_RECV：服务端应发出ACK确认客户端的 SYN，同时自己向客户端发送一个SYN，之后状态置为，在收到和发送一个连接请求后等待对连接请求的确认；

4. ESTABLISHED：代表一个打开的连接，双方可以进行或已经在数据交互了， 代表一个打开的连接，数据可以传送给用户；

5. FIN_WAIT1：主动关闭(active close)端应用程序调用close，于是其TCP发出FIN请求主动关闭连接，之后进入FIN_WAIT1状态， 等待远程TCP的连接中断请求，或先前的连接中断请求的确认；

6. CLOSE_WAIT：被动关闭(passive close)端TCP接到FIN后，就发出ACK以回应FIN请求(它的接收也作为文件结束符传递给上层应用程序)，并进入CLOSE_WAIT， 等待从本地用户发来的连接中断请求；

7. FIN_WAIT2：主动关闭端接到ACK后，就进入了 FIN-WAIT-2，从远程TCP等待连接中断请求；

8. LAST_ACK：被动关闭端一段时间后，接收到文件结束符的应用程 序将调用CLOSE关闭连接，这导致它的TCP也发送一个 FIN,等待对方的ACK.就进入了LAST-ACK，等待原来发向远程TCP的连接中断请求的确认；

9. TIME_WAIT:在主动关闭端接收到FIN后，TCP 就发送ACK包，并进入TIME-WAIT状态，等待足够的时间以确保远程TCP接收到连接中断请求的确认；

10. CLOSING: 比较少见，等待远程TCP对连接中断的确认；

11. CLOSED: 被动关闭端在接受到ACK包后，就进入了closed的状态，连接结束，没有任何连接状态；

12. UNKNOWN：未知的Socket状态；

**常见标志位**

* SYN: (同步序列编号,Synchronize Sequence Numbers)该标志仅在三次握手建立TCP连接时有效。表示一个新的TCP连接请求。

* ACK: (确认编号,Acknowledgement Number)是对TCP请求的确认标志,同时提示对端系统已经成功接收所有数据。

* FIN: (结束标志,FINish)用来结束一个TCP回话.但对应端口仍处于开放状态,准备接收后续数据。



vi
===

功能强大的纯文本编辑器

## 补充说明

**vi命令** 是UNIX操作系统和类UNIX操作系统中最通用的全屏幕纯文本编辑器。Linux中的vi编辑器叫vim，它是vi的增强版（vi Improved），与vi编辑器完全兼容，而且实现了很多增强功能。

vi编辑器支持编辑模式和命令模式，编辑模式下可以完成文本的编辑功能，命令模式下可以完成对文件的操作命令，要正确使用vi编辑器就必须熟练掌握着两种模式的切换。默认情况下，打开vi编辑器后自动进入命令模式。从编辑模式切换到命令模式使用“esc”键，从命令模式切换到编辑模式使用“A”、“a”、“O”、“o”、“I”、“i”键。

vi编辑器提供了丰富的内置命令，有些内置命令使用键盘组合键即可完成，有些内置命令则需要以冒号“：”开头输入。常用内置命令如下：

```shell
Ctrl+u：向文件首翻半屏；
Ctrl+d：向文件尾翻半屏；
Ctrl+f：向文件尾翻一屏；
Ctrl+b：向文件首翻一屏；
Esc：从编辑模式切换到命令模式；
ZZ：命令模式下保存当前文件所做的修改后退出vi；
:行号：光标跳转到指定行的行首；
:$：光标跳转到最后一行的行首；
x或X：删除一个字符，x删除光标后的，而X删除光标前的；
D：删除从当前光标到光标所在行尾的全部字符；
dd：删除光标行正行内容；
ndd：删除当前行及其后n-1行；
nyy：将当前行及其下n行的内容保存到寄存器？中，其中？为一个字母，n为一个数字；
p：粘贴文本操作，用于将缓存区的内容粘贴到当前光标所在位置的下方；
P：粘贴文本操作，用于将缓存区的内容粘贴到当前光标所在位置的上方；
/字符串：文本查找操作，用于从当前光标所在位置开始向文件尾部查找指定字符串的内容，查找的字符串会被加亮显示；
？字符串：文本查找操作，用于从当前光标所在位置开始向文件头部查找指定字符串的内容，查找的字符串会被加亮显示；
a，bs/F/T：替换文本操作，用于在第a行到第b行之间，将F字符串换成T字符串。其中，“s/”表示进行替换操作；
a：在当前字符后添加文本；
A：在行末添加文本；
i：在当前字符前插入文本；
I：在行首插入文本；
o：在当前行后面插入一空行；
O：在当前行前面插入一空行；
:wq：在命令模式下，执行存盘退出操作；
:w：在命令模式下，执行存盘操作；
:w！：在命令模式下，执行强制存盘操作；
:q：在命令模式下，执行退出vi操作；
:q！：在命令模式下，执行强制退出vi操作；
:e文件名：在命令模式下，打开并编辑指定名称的文件；
:n：在命令模式下，如果同时打开多个文件，则继续编辑下一个文件；
:f：在命令模式下，用于显示当前的文件名、光标所在行的行号以及显示比例；
:set number：在命令模式下，用于在最左端显示行号；
:set nonumber：在命令模式下，用于在最左端不显示行号；
```

###  语法

```shell
vi(选项)(参数)
```

###  选项

```shell
+<行号>：从指定行号的行开始显示文本内容；
-b：以二进制模式打开文件，用于编辑二进制文件和可执行文件；
-c<指令>：在完成对第一个文件编辑任务后，执行给出的指令；
-d：以diff模式打开文件，当多个文件编辑时，显示文件差异部分；
-l：使用lisp模式，打开“lisp”和“showmatch”；
-m：取消写文件功能，重设“write”选项；
-M：关闭修改功能；
-n：不实用缓存功能；
-o<文件数目>：指定同时打开指定数目的文件；
-R：以只读方式打开文件；
-s：安静模式，不显示指令的任何错误信息。
```

###  参数

文件列表：指定要编辑的文件列表。多个文件之间使用空格分隔开。

## 知识扩展  

vi编辑器有三种工作方式：命令方式、输入方式和ex转义方式。通过相应的命令或操作，在这三种工作方式之间可以进行转换。

**命令方式** 

在Shell提示符后输入命令vi，进入vi编辑器，并处于vi的命令方式。此时，从键盘上输入的任何字符都被作为编辑命令来解释，例如，a(append）表示附加命令，i(insert）表示插入命令，x表示删除字符命令等。如果输入的字符不是vi的合法命令，则机器发出“报警声”，光标不移动。另外，在命令方式下输入的字符（即vi命令）并不在屏幕上显示出来，例如，输入i，屏幕上并无变化，但通过执行i命令，编辑器的工作方式却发生变化：由命令方式变为输入方式。

**输入方式** 

通过输入vi的插入命令（i）、附加命令（a）、打开命令（o）、替换命令（s）、修改命令(c）或取代命令（r）可以从命令方式进入输入方式。在输入方式下，从键盘上输入的所有字符都被插入到正在编辑的缓冲区中，被当做该文件的正文。进入输入方式后，输入的可见字符都在屏幕上显示出来，而编辑命令不再起作用，仅作为普通字母出现。例如，在命令方式下输入字母i，进到输入方式，然后再输入i，就在屏幕上相应光标处添加一个字母i。

由输入方式回到命令方式的办法是按下Esc键。如果已在命令方式下，那么按下Esc键就会发出“嘟嘟”声。为了确保用户想执行的vi命令是在命令方式下输入的，不妨多按几下Esc键，听到嘟声后再输入命令。

**ex转义方式** 

vi和ex编辑器的功能是相同的，二者的主要区别是用户界面。在vi中，命令通常是单个字母，如a,x,r等。而在ex中，命令是以Enter；键结束的命令行。vi有一个专门的“转义”命令，可访问很多面向行的ex命令。为使用ex转义方式，可输入一个冒号（:）。作为ex命令提示符，冒号出现在状态行（通常在屏幕最下一行）。按下中断键（通常是Del键），可终止正在执行的命令。多数文件管理命令都是在ex转义方式下执行的（例如，读取文件，把编辑缓冲区的内容写到文件中等）。转义命令执行后，自动回到命令方式。例如：

```shell
:1,$s/I/i/g 按Enter键
```

则从文件第一行至文件末尾（$）将大写I全部替换成小写i。vi编辑器的三种工作方式之间的转换如图所示。

!vi




chgrp
===

用来变更文件或目录的所属群组

## 补充说明

**chgrp命令** 用来改变文件或目录所属的用户组。该命令用来改变指定文件所属的用户组。其中，组名可以是用户组的id，也可以是用户组的组名。文件名可以 是由空格分开的要改变属组的文件列表，也可以是由通配符描述的文件集合。如果用户不是该文件的文件主或超级用户(root)，则不能改变该文件的组。

在UNIX系统家族里，文件或目录权限的掌控以拥有者及所属群组来管理。您可以使用chgrp指令去变更文件与目录的所属群组，设置方式采用群组名称或群组识别码皆可。

###  语法 

```shell
chgrp [选项][组群][文件|目录]
```

###  选项 

```shell
-R 递归式地改变指定目录及其下的所有子目录和文件的所属的组
-c或——changes：效果类似“-v”参数，但仅回报更改的部分；
-f或--quiet或——silent：不显示错误信息；
-h或--no-dereference：只对符号连接的文件作修改，而不是该其他任何相关文件；
-H如果命令行参数是一个通到目录的符号链接，则遍历符号链接
-R或——recursive：递归处理，将指令目录下的所有文件及子目录一并处理；
-L遍历每一个遇到的通到目录的符号链接
-P不遍历任何符号链接（默认）
-v或——verbose：显示指令执行过程；
--reference=<参考文件或目录>：把指定文件或目录的所属群组全部设成和参考文件或目录的所属群组相同；
```

###  参数 

*   组：指定新工作名称；
*   文件：指定要改变所属组的文件列表。多个文件或者目录之间使用空格隔开。

###  实例 

将`/usr/meng`及其子目录下的所有文件的用户组改为mengxin

```shell
chgrp -R mengxin /usr/meng
```

更改文件ah的组群所有者为 `newuser`

```shell
[root@rhel ~]# chgrp newuser ah
```




chown
===

用来变更文件或目录的拥有者或所属群组

## 补充说明

**chown命令** 改变某个文件或目录的所有者和所属的组，该命令可以向某个用户授权，使该用户变成指定文件的所有者或者改变文件所属的组。用户可以是用户或者是用户D，用户组可以是组名或组id。文件名可以使由空格分开的文件列表，在文件名中可以包含通配符。

只有文件主和超级用户才可以使用该命令。

###  语法

```shell
chown(选项)(参数)
```

###  选项

```shell
-c或——changes：效果类似“-v”参数，但仅回报更改的部分；
-f或--quite或——silent：不显示错误信息；
-h或--no-dereference：只对符号连接的文件作修改，而不更改其他任何相关文件；
-R或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；
-v或——version：显示指令执行过程；
--dereference：效果和“-h”参数相同；
--help：在线帮助；
--reference=<参考文件或目录>：把指定文件或目录的拥有者与所属群组全部设成和参考文件或目录的拥有者与所属群组相同；
--version：显示版本信息。
```

###  参数

用户：组：指定所有者和所属工作组。当省略“：组”，仅改变文件所有者；  
文件：指定要改变所有者和工作组的文件列表。支持多个文件和目标，支持shell通配符。

###  实例

将目录`/usr/meng`及其下面的所有文件、子目录的文件主改成 liu：

```shell
chown -R liu /usr/meng
```



du
===

显示每个文件和目录的磁盘使用空间

## 补充说明

**du命令** 也是查看使用空间的，但是与df命令不同的是Linux du命令是对文件和目录磁盘使用的空间的查看，还是和df命令有一些区别的。

### 语法

```shell
du [选项][文件]
```

### 选项

```shell
-a, --all                              显示目录中个别文件的大小。
-B, --block-size=大小                  使用指定字节数的块
-b, --bytes                            显示目录或文件大小时，以byte为单位。
-c, --total                            除了显示个别目录或文件的大小外，同时也显示所有目录或文件的总和。
-D, --dereference-args                 显示指定符号链接的源文件大小。
-d, --max-depth=N                      限制文件夹深度
-H, --si                               与-h参数相同，但是K，M，G是以1000为换算单位。
-h, --human-readable                   以K，M，G为单位，提高信息的可读性。
-k, --kilobytes                        以KB(1024bytes)为单位输出。
-l, --count-links                      重复计算硬件链接的文件。
-m, --megabytes                        以MB为单位输出。
-L<符号链接>, --dereference<符号链接>  显示选项中所指定符号链接的源文件大小。
-P, --no-dereference                   不跟随任何符号链接(默认)
-0, --null                             将每个空行视作0 字节而非换行符
-S, --separate-dirs                    显示个别目录的大小时，并不含其子目录的大小。
-s, --summarize                        仅显示总计，只列出最后加总的值。
-x, --one-file-xystem                  以一开始处理时的文件系统为准，若遇上其它不同的文件系统目录则略过。
-X<文件>, --exclude-from=<文件>        在<文件>指定目录或文件。
--apparent-size                        显示表面用量，而并非是磁盘用量；虽然表面用量通常会小一些，但有时它会因为稀疏文件间的"洞"、内部碎片、非直接引用的块等原因而变大。
--files0-from=F                        计算文件F中以NUL结尾的文件名对应占用的磁盘空间如果F的值是"-"，则从标准输入读入文件名
--exclude=<目录或文件>                 略过指定的目录或文件。
--max-depth=N                          显示目录总计(与--all 一起使用计算文件)当N为指定数值时计算深度为N，等于0时等同--summarize
--si                                   类似-h，但在计算时使用1000 为基底而非1024
--time                                 显示目录或该目录子目录下所有文件的最后修改时间
--time=WORD                            显示WORD时间，而非修改时间：atime，access，use，ctime 或status
--time-style=样式                      按照指定样式显示时间(样式解释规则同"date"命令)：full-iso，long-iso，iso，+FORMAT
--help                                 显示此帮助信息并退出
--version                              显示版本信息并退出
```

### 实例

文件从大到小排序
```
ubuntu@VM-0-14-ubuntu:~/git-work/linux-command$ du -sh * |sort -rh
2.9M    command
1.9M    assets
148K    template
72K     package-lock.json
52K     dist
28K     build
16K     README.md
4.0K    renovate.json
4.0K    package.json
4.0K    LICENSE
```

只显示当前目录下子目录的大小。

```shell
ubuntu@VM-0-14-ubuntu:~/git-work/linux-command$ du -sh ./*/
1.9M    ./assets/
28K     ./build/
2.9M    ./command/
52K     ./dist/
148K    ./template/
```

查看指定目录下文件所占的空间：

```shell
ubuntu@VM-0-14-ubuntu:~/git-work/linux-command/assets$ du ./*
144     ./alfred.png
452     ./chrome-extensions.gif
4       ./dash-icon.png
1312    ./Linux.gif
16      ./qr.png
```

只显示总和的大小:

```shell
ubuntu@VM-0-14-ubuntu:~/git-work/linux-command/assets$ du -s .
1932    .
```

显示总和的大小且易读:

```shell
ubuntu@VM-0-14-ubuntu:~/git-work/linux-command/assets$ du -sh .
1.9M    .
```



ps
===

报告当前系统的进程状态

## 补充说明

**ps命令** 用于报告当前系统的进程状态。可以搭配kill指令随时中断、删除不必要的程序。ps命令是最基本同时也是非常强大的进程查看命令，使用该命令可以确定有哪些进程正在运行和运行的状态、进程是否结束、进程有没有僵死、哪些进程占用了过多的资源等等，总之大部分信息都是可以通过执行该命令得到的。

### 语法

```shell
ps(选项)
```

### 选项

```shell
-a：显示所有终端机下执行的程序，除了阶段作业领导者之外。
a：显示现行终端机下的所有程序，包括其他用户的程序。
-A：显示所有程序。
-c：显示CLS和PRI栏位。
c：列出程序时，显示每个程序真正的指令名称，而不包含路径，选项或常驻服务的标示。
-C<指令名称>：指定执行指令的名称，并列出该指令的程序的状况。
-d：显示所有程序，但不包括阶段作业领导者的程序。
-e：此选项的效果和指定"A"选项相同。
e：列出程序时，显示每个程序所使用的环境变量。
-f：显示UID,PPIP,C与STIME栏位。
f：用ASCII字符显示树状结构，表达程序间的相互关系。
-g<群组名称>：此选项的效果和指定"-G"选项相同，当亦能使用阶段作业领导者的名称来指定。
g：显示现行终端机下的所有程序，包括群组领导者的程序。
-G<群组识别码>：列出属于该群组的程序的状况，也可使用群组名称来指定。
h：不显示标题列。
-H：显示树状结构，表示程序间的相互关系。
-j或j：采用工作控制的格式显示程序状况。
-l或l：采用详细的格式来显示程序状况。
L：列出栏位的相关信息。
-m或m：显示所有的执行绪。
n：以数字来表示USER和WCHAN栏位。
-N：显示所有的程序，除了执行ps指令终端机下的程序之外。
-p<程序识别码>：指定程序识别码，并列出该程序的状况。
p<程序识别码>：此选项的效果和指定"-p"选项相同，只在列表格式方面稍有差异。
r：只列出现行终端机正在执行中的程序。
-s<阶段作业>：指定阶段作业的程序识别码，并列出隶属该阶段作业的程序的状况。
s：采用程序信号的格式显示程序状况。
S：列出程序时，包括已中断的子程序资料。
-t<终端机编号>：指定终端机编号，并列出属于该终端机的程序的状况。
t<终端机编号>：此选项的效果和指定"-t"选项相同，只在列表格式方面稍有差异。
-T：显示现行终端机下的所有程序。
-u<用户识别码>：此选项的效果和指定"-U"选项相同。
u：以用户为主的格式来显示程序状况。
-U<用户识别码>：列出属于该用户的程序的状况，也可使用用户名称来指定。
U<用户名称>：列出属于该用户的程序的状况。
v：采用虚拟内存的格式显示程序状况。
-V或V：显示版本信息。
-w或w：采用宽阔的格式来显示程序状况。　
x：显示所有程序，不以终端机来区分。
X：采用旧式的Linux i386登陆格式显示程序状况。
-y：配合选项"-l"使用时，不显示F(flag)栏位，并以RSS栏位取代ADDR栏位　。
-<程序识别码>：此选项的效果和指定"p"选项相同。
--cols<每列字符数>：设置每列的最大字符数。
--columns<每列字符数>：此选项的效果和指定"--cols"选项相同。
--cumulative：此选项的效果和指定"S"选项相同。
--deselect：此选项的效果和指定"-N"选项相同。
--forest：此选项的效果和指定"f"选项相同。
--headers：重复显示标题列。
--help：在线帮助。
--info：显示排错信息。
--lines<显示列数>：设置显示画面的列数。
--no-headers：此选项的效果和指定"h"选项相同，只在列表格式方面稍有差异。
--group<群组名称>：此选项的效果和指定"-G"选项相同。
--Group<群组识别码>：此选项的效果和指定"-G"选项相同。
--pid<程序识别码>：此选项的效果和指定"-p"选项相同。
--rows<显示列数>：此选项的效果和指定"--lines"选项相同。
--sid<阶段作业>：此选项的效果和指定"-s"选项相同。
--tty<终端机编号>：此选项的效果和指定"-t"选项相同。
--user<用户名称>：此选项的效果和指定"-U"选项相同。
--User<用户识别码>：此选项的效果和指定"-U"选项相同。
--version：此选项的效果和指定"-V"选项相同。
--widty<每列字符数>：此选项的效果和指定"-cols"选项相同。
```

由于ps命令能够支持的系统类型相当的多，所以选项多的离谱！

### 实例

```shell
ps axo pid,comm,pcpu # 查看进程的PID、名称以及CPU 占用率
ps aux | sort -rnk 4 # 按内存资源的使用量对进程进行排序
ps aux | sort -nk 3  # 按 CPU 资源的使用量对进程进行排序
ps -A # 显示所有进程信息
ps -u root # 显示指定用户信息
ps -efL # 查看线程数
ps -e -o "%C : %p :%z : %a"|sort -k5 -nr # 查看进程并按内存使用大小排列
ps -ef # 显示所有进程信息，连同命令行
ps -ef | grep ssh # ps 与grep 常用组合用法，查找特定进程
ps -C nginx # 通过名字或命令搜索进程
ps aux --sort=-pcpu,+pmem # CPU或者内存进行排序,-降序，+升序
ps -f --forest -C nginx # 用树的风格显示进程的层次关系
ps -o pid,uname,comm -C nginx # 显示一个父进程的子进程
ps -e -o pid,uname=USERNAME,pcpu=CPU_USAGE,pmem,comm # 重定义标签
ps -e -o pid,comm,etime # 显示进程运行的时间
ps -aux | grep named # 查看named进程详细信息
ps -o command -p 91730 | sed -n 2p # 通过进程id获取服务名称
```

将目前属于您自己这次登入的 PID 与相关信息列示出来

```shell
ps -l
#  UID   PID  PPID        F CPU PRI NI       SZ    RSS WCHAN     S             ADDR TTY           TIME CMD
#  501   566   559     4006   0  31  0  4317620    228 -      Ss                  0 ttys001    0:00.05 /App...cOS/iTerm2 --server /usr/bin/login -fpl kenny /Ap...s/MacOS/iTerm2 --launch_shel
#  501   592   577     4006   0  31  0  4297048     52 -      S                   0 ttys001    0:00.63 -zsh
```

- F 代表这个程序的旗标 (flag)， 4 代表使用者为 super user
- S 代表这个程序的状态 (STAT)，关于各 STAT 的意义将在内文介绍
- UID 程序被该 UID 所拥有
- PID 就是这个程序的 ID ！
- PPID 则是其上级父程序的ID
- C CPU 使用的资源百分比
- PRI 这个是 Priority (优先执行序) 的缩写，详细后面介绍
- NI 这个是 Nice 值，在下一小节我们会持续介绍
- ADDR 这个是 kernel function，指出该程序在内存的那个部分。如果是个 running的程序，一般就是 "-"
- SZ 使用掉的内存大小
- WCHAN 目前这个程序是否正在运作当中，若为 - 表示正在运作
- TTY 登入者的终端机位置
- TIME 使用掉的 CPU 时间。
- CMD 所下达的指令为何

> 在预设的情况下， `ps` 仅会列出与目前所在的 `bash shell` 有关的 `PID` 而已，所以， 当我使用 `ps -l` 的时候，只有三个 PID。

列出目前所有的正在内存当中的程序

```shell
ps aux

# USER               PID  %CPU %MEM      VSZ    RSS   TT  STAT STARTED      TIME COMMAND
# kenny             6155  21.3  1.7  7969944 284912   ??  S    二03下午 199:14.14 /Appl...OS/WeChat
# kenny              559  20.4  0.8  4963740 138176   ??  S    二03下午  33:28.27 /Appl...S/iTerm2
# _windowserver      187  18.0  0.6  7005748  95884   ??  Ss   二03下午 288:44.97 /Syst...Light.WindowServer -daemon
# kenny             1408  10.7  2.1  5838592 347348   ??  S    二03下午 138:51.63 /Appl...nts/MacOS/Google Chrome
# kenny              327   5.8  0.5  5771984  79452   ??  S    二03下午   2:51.58 /Syst...pp/Contents/MacOS/Finder
```

- USER：该 process 属于那个使用者账号的
- PID ：该 process 的号码
- %CPU：该 process 使用掉的 CPU 资源百分比
- %MEM：该 process 所占用的物理内存百分比
- VSZ ：该 process 使用掉的虚拟内存量 (Kbytes)
- RSS ：该 process 占用的固定的内存量 (Kbytes)
- TTY ：该 process 是在那个终端机上面运作，若与终端机无关，则显示 ?，另外， tty1-tty6 是本机上面的登入者程序，若为 pts/0 等等的，则表示为由网络连接进主机的程序。
- STAT：该程序目前的状态，主要的状态有
- R ：该程序目前正在运作，或者是可被运作
- S ：该程序目前正在睡眠当中 (可说是 idle 状态)，但可被某些讯号 (signal) 唤醒。
- T ：该程序目前正在侦测或者是停止了
- Z ：该程序应该已经终止，但是其父程序却无法正常的终止他，造成 zombie (疆尸) 程序的状态
- START：该 process 被触发启动的时间
- TIME ：该 process 实际使用 CPU 运作的时间
- COMMAND：该程序的实际指令

列出类似程序树的程序显示

```shell
ps -axjf

# USER               PID  PPID  PGID   SESS JOBC STAT   TT       TIME COMMAND            UID   C STIME   TTY
# root                 1     0     1      0    0 Ss     ??   10:51.90 /sbin/launchd        0   0 二03下午 ??
# root                50     1    50      0    0 Ss     ??    0:10.07 /usr/sbin/syslog     0   0 二03下午 ??
# root                51     1    51      0    0 Ss     ??    0:29.90 /usr/libexec/Use     0   0 二03下午 ??
```

找出与 cron 与 syslog 这两个服务有关的 PID 号码

```shell
ps aux | egrep '(cron|syslog)'

# root                50   0.0  0.0  4305532   1284   ??  Ss   二03下午   0:10.08 /usr/sbin/syslogd
# kenny            90167   0.0  0.0  4258468    184 s007  R+    9:23下午   0:00.00 egrep (cron|syslog)
```

把所有进程显示出来，并输出到ps001.txt文件

```shell
ps -aux > ps001.txt
```

输出指定的字段



top
===

显示或管理执行中的程序

## 补充说明

**top命令** 可以实时动态地查看系统的整体运行情况，是一个综合了多方信息监测系统性能和运行信息的实用工具。通过top命令所提供的互动式界面，用热键可以管理。

###  语法

```shell
top(选项)
```

###  选项

```shell
-b：以批处理模式操作；
-c：显示完整的治命令；
-d：屏幕刷新间隔时间；
-I：忽略失效过程；
-s：保密模式；
-S：累积模式；
-i<时间>：设置间隔时间；
-u<用户名>：指定用户名；
-p<进程号>：指定进程；
-n<次数>：循环显示的次数；
-H：所有线程占用资源情况。
```

###  top交互命令

在top命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了-s选项， 其中一些命令可能会被屏蔽。

```shell
h：显示帮助画面，给出一些简短的命令总结说明；
k：终止一个进程；
i：忽略闲置和僵死进程，这是一个开关式命令；
q：退出程序；
r：重新安排一个进程的优先级别；
S：切换到累计模式；
s：改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成ms。输入0值则系统将不断刷新，默认值是5s；
f或者F：从当前显示中添加或者删除项目；
o或者O：改变显示项目的顺序；
l：切换显示平均负载和启动时间信息；
m：切换显示内存信息；
t：切换显示进程和CPU状态信息；
c：切换显示命令名称和完整命令行；
M：根据驻留内存大小进行排序；
P：根据CPU使用百分比大小进行排序；
T：根据时间/累计时间进行排序；
w：将当前设置写入~/.toprc文件中。
```

###  实例

```shell
top - 09:44:56 up 16 days, 21:23,  1 user,  load average: 9.59, 4.75, 1.92
Tasks: 145 total,   2 running, 143 sleeping,   0 stopped,   0 zombie
Cpu(s): 99.8%us,  0.1%sy,  0.0%ni,  0.2%id,  0.0%wa,  0.0%hi,  0.0%si,  0.0%st
Mem:   4147888k total,  2493092k used,  1654796k free,   158188k buffers
Swap:  5144568k total,       56k used,  5144512k free,  2013180k cached
```

 **解释：** 

*  top - 09:44:56[当前系统时间],
*  16 days[系统已经运行了16天],
*  1 user[个用户当前登录],
*  load average: 9.59, 4.75, 1.92[系统负载，即任务队列的平均长度]
*  Tasks: 145 total[总进程数],
*  2 running[正在运行的进程数],
*  143 sleeping[睡眠的进程数],
*  0 stopped[停止的进程数],
*  0 zombie[冻结进程数],
*  Cpu(s): 99.8%us[用户空间占用CPU百分比],
*  0.1%sy[内核空间占用CPU百分比],
*  0.0%ni[用户进程空间内改变过优先级的进程占用CPU百分比],
*  0.2%id[空闲CPU百分比], 0.0%wa[等待输入输出的CPU时间百分比],
*  0.0%hi[],
*  0.0%st[],
*  Mem: 4147888k total[物理内存总量],
*  2493092k used[使用的物理内存总量],
*  1654796k free[空闲内存总量],
*  158188k buffers[用作内核缓存的内存量]
*  Swap:  5144568k total[交换区总量],
*  56k used[使用的交换区总量],
*  5144512k free[空闲交换区总量],
*  2013180k cached[缓冲的交换区总量],



grep
===

强大的文本搜索工具

## 补充说明

**grep** （global search regular expression(RE) and print out the line，全面搜索正则表达式并把行打印出来）是一种强大的文本搜索工具，它能使用正则表达式搜索文本，并把匹配的行打印出来。用于过滤/搜索的特定字符。可使用正则表达式能配合多种命令使用，使用上十分灵活。

###  选项 

```shell
-a --text  # 不要忽略二进制数据。
-A <显示行数>   --after-context=<显示行数>   # 除了显示符合范本样式的那一行之外，并显示该行之后的内容。
-b --byte-offset                           # 在显示符合范本样式的那一行之外，并显示该行之前的内容。
-B<显示行数>   --before-context=<显示行数>   # 除了显示符合样式的那一行之外，并显示该行之前的内容。
-c --count    # 计算符合范本样式的列数。
-C<显示行数> --context=<显示行数>或-<显示行数> # 除了显示符合范本样式的那一列之外，并显示该列之前后的内容。
-d<进行动作> --directories=<动作>  # 当指定要查找的是目录而非文件时，必须使用这项参数，否则grep命令将回报信息并停止动作。
-e<范本样式> --regexp=<范本样式>   # 指定字符串作为查找文件内容的范本样式。
-E --extended-regexp             # 将范本样式为延伸的普通表示法来使用，意味着使用能使用扩展正则表达式。
-f<范本文件> --file=<规则文件>     # 指定范本文件，其内容有一个或多个范本样式，让grep查找符合范本条件的文件内容，格式为每一列的范本样式。
-F --fixed-regexp   # 将范本样式视为固定字符串的列表。
-G --basic-regexp   # 将范本样式视为普通的表示法来使用。
-h --no-filename    # 在显示符合范本样式的那一列之前，不标示该列所属的文件名称。
-H --with-filename  # 在显示符合范本样式的那一列之前，标示该列的文件名称。
-i --ignore-case    # 忽略字符大小写的差别。
-l --file-with-matches   # 列出文件内容符合指定的范本样式的文件名称。
-L --files-without-match # 列出文件内容不符合指定的范本样式的文件名称。
-n --line-number         # 在显示符合范本样式的那一列之前，标示出该列的编号。
-P --perl-regexp         # PATTERN 是一个 Perl 正则表达式
-q --quiet或--silent     # 不显示任何信息。
-R/-r  --recursive       # 此参数的效果和指定“-d recurse”参数相同。
-s --no-messages  # 不显示错误信息。
-v --revert-match # 反转查找。
-V --version      # 显示版本信息。   
-w --word-regexp  # 只显示全字符合的列。
-x --line-regexp  # 只显示全列符合的列。
-y # 此参数效果跟“-i”相同。
-o # 只输出文件中匹配到的部分。
-m <num> --max-count=<num> # 找到num行结果后停止查找，用来限制匹配行数
```

### 规则表达式

```shell
^    # 锚定行的开始 如：'^grep'匹配所有以grep开头的行。    
$    # 锚定行的结束 如：'grep$' 匹配所有以grep结尾的行。
.    # 匹配一个非换行符的字符 如：'gr.p'匹配gr后接一个任意字符，然后是p。    
*    # 匹配零个或多个先前字符 如：'*grep'匹配所有一个或多个空格后紧跟grep的行。    
.*   # 一起用代表任意字符。   
[]   # 匹配一个指定范围内的字符，如'[Gg]rep'匹配Grep和grep。    
[^]  # 匹配一个不在指定范围内的字符，如：'[^A-Z]rep' 匹配不包含 A-Z 中的字母开头，紧跟 rep 的行
\(..\)  # 标记匹配字符，如'\(love\)'，love被标记为1。    
\<      # 锚定单词的开始，如:'\<grep'匹配包含以grep开头的单词的行。    
\>      # 锚定单词的结束，如'grep\>'匹配包含以grep结尾的单词的行。    
x\{m\}  # 重复字符x，m次，如：'0\{5\}'匹配包含5个o的行。    
x\{m,\}   # 重复字符x,至少m次，如：'o\{5,\}'匹配至少有5个o的行。    
x\{m,n\}  # 重复字符x，至少m次，不多于n次，如：'o\{5,10\}'匹配5--10个o的行。   
\w    # 匹配文字和数字字符，也就是[A-Za-z0-9]，如：'G\w*p'匹配以G后跟零个或多个文字或数字字符，然后是p。   
\W    # \w的反置形式，匹配一个或多个非单词字符，如点号句号等。   
\b    # 单词锁定符，如: '\bgrep\b'只匹配grep。  
```

## grep命令常见用法  

在文件中搜索一个单词，命令会返回一个包含 **“match_pattern”** 的文本行：

```shell
grep match_pattern file_name
grep "match_pattern" file_name
```

在多个文件中查找：

```shell
grep "match_pattern" file_1 file_2 file_3 ...
```

输出除之外的所有行  **-v**  选项：

```shell
grep -v "match_pattern" file_name
```

标记匹配颜色  **--color=auto**  选项：

```shell
grep "match_pattern" file_name --color=auto
```

使用正则表达式  **-E**  选项：

```shell
grep -E "[1-9]+"
# 或
egrep "[1-9]+"
```
使用正则表达式  **-P**  选项：

```shell
grep -P "(\d{3}\-){2}\d{4}" file_name
```


只输出文件中匹配到的部分  **-o**  选项：

```shell
echo this is a test line. | grep -o -E "[a-z]+\."
line.

echo this is a test line. | egrep -o "[a-z]+\."
line.
```

统计文件或者文本中包含匹配字符串的行数  **-c**  选项：

```shell
grep -c "text" file_name
```

搜索命令行历史记录中 输入过 `git` 命令的记录：

```shell
history | grep git
```

输出包含匹配字符串的行数  **-n**  选项：

```shell
grep "text" -n file_name
# 或
cat file_name | grep "text" -n

#多个文件
grep "text" -n file_1 file_2
```

打印样式匹配所位于的字符或字节偏移：

```shell
echo gun is not unix | grep -b -o "not"
7:not
#一行中字符串的字符偏移是从该行的第一个字符开始计算，起始值为0。选项  **-b -o**  一般总是配合使用。
```

搜索多个文件并查找匹配文本在哪些文件中：

```shell
grep -l "text" file1 file2 file3...
```

###  grep递归搜索文件 

在多级目录中对文本进行递归搜索：

```shell
grep "text" . -r -n
# .表示当前目录。
```

忽略匹配样式中的字符大小写：

```shell
echo "hello world" | grep -i "HELLO"
# hello
```

选项  **-e**  制动多个匹配样式：

```shell
echo this is a text line | grep -e "is" -e "line" -o
is
is
line

#也可以使用 **-f** 选项来匹配多个样式，在样式文件中逐行写出需要匹配的字符。
cat patfile
aaa
bbb

echo aaa bbb ccc ddd eee | grep -f patfile -o
```

在grep搜索结果中包括或者排除指定文件：

```shell
# 只在目录中所有的.php和.html文件中递归搜索字符"main()"
grep "main()" . -r --include *.{php,html}

# 在搜索结果中排除所有README文件
grep "main()" . -r --exclude "README"

# 在搜索结果中排除filelist文件列表里的文件
grep "main()" . -r --exclude-from filelist

```

使用0值字节后缀的grep与xargs：

```shell
# 测试文件：
echo "aaa" > file1
echo "bbb" > file2
echo "aaa" > file3

grep "aaa" file* -lZ | xargs -0 rm

# 执行后会删除file1和file3，grep输出用-Z选项来指定以0值字节作为终结符文件名（\0），xargs -0 读取输入并用0值字节终结符分隔文件名，然后删除匹配文件，-Z通常和-l结合使用。
```

grep静默输出：

```shell
grep -q "test" filename
# 不会输出任何信息，如果命令运行成功返回0，失败则返回非0值。一般用于条件测试。
```

打印出匹配文本之前或者之后的行：

```shell
# 显示匹配某个结果之后的3行，使用 -A 选项：
seq 10 | grep "5" -A 3
5
6
7
8

# 显示匹配某个结果之前的3行，使用 -B 选项：
seq 10 | grep "5" -B 3
2
3
4
5

# 显示匹配某个结果的前三行和后三行，使用 -C 选项：
seq 10 | grep "5" -C 3
2
3
4
5
6
7
8

# 如果匹配结果有多个，会用“--”作为各匹配结果之间的分隔符：
echo -e "a\nb\nc\na\nb\nc" | grep a -A 1
a
b
--
a
b
```




cat
===

连接多个文件并打印到标准输出。

## 概要

```shell
cat [OPTION]... [FILE]...
```

## 主要用途

- 显示文件内容，如果没有文件或文件为`-`则读取标准输入。
- 将多个文件的内容进行连接并打印到标准输出。
- 显示文件内容中的不可见字符（控制字符、换行符、制表符等）。

## 参数

FILE（可选）：要处理的文件，可以为一或多个。

## 选项 

```shell
长选项与短选项等价

-A, --show-all           等价于"-vET"组合选项。
-b, --number-nonblank    只对非空行编号，从1开始编号，覆盖"-n"选项。
-e                       等价于"-vE"组合选项。
-E, --show-ends          在每行的结尾显示'$'字符。
-n, --number             对所有行编号，从1开始编号。
-s, --squeeze-blank      压缩连续的空行到一行。
-t                       等价于"-vT"组合选项。
-T, --show-tabs          使用"^I"表示TAB（制表符）。
-u                       POSIX兼容性选项，无意义。
-v, --show-nonprinting   使用"^"和"M-"符号显示控制字符，除了LFD（line feed，即换行符'\n'）和TAB（制表符）。

--help                   显示帮助信息并退出。
--version                显示版本信息并退出。
```

## 返回值

返回状态为成功除非给出了非法选项或非法参数。

## 例子 

```shell
# 合并显示多个文件
cat ./1.log ./2.log ./3.log
# 显示文件中的非打印字符、tab、换行符
cat -A test.log
# 压缩文件的空行
cat -s test.log
# 显示文件并在所有行开头附加行号
cat -n test.log
# 显示文件并在所有非空行开头附加行号
cat -b test.log
# 将标准输入的内容和文件内容一并显示
echo '######' |cat - test.log
```

### 注意

1. 该命令是`GNU coreutils`包中的命令，相关的帮助信息请查看`man -s 1 cat`或`info coreutils 'cat invocation'`。
2. 当使用`cat`命令查看**体积较大的文件**时，文本在屏幕上迅速闪过（滚屏），用户往往看不清所显示的内容，为了控制滚屏，可以按`Ctrl+s`键停止滚屏；按`Ctrl+q`键恢复滚屏；按`Ctrl+c`（中断）键可以终止该命令的执行，返回Shell提示符状态。
3. 建议您查看**体积较大的文件**时使用`less`、`more`命令或`emacs`、`vi`等文本编辑器。

### 参考链接

1. [Question about LFD key](https://superuser.com/questions/328054/is-there-an-lfd-key-on-my-keyboard)



less
===

分屏上下翻页浏览文件内容

## 补充说明

**less命令** 的作用与more十分相似，都可以用来浏览文字档案的内容，不同的是less命令允许用户向前或向后浏览文件，而more命令只能向前浏览。用less命令显示文件时，用PageUp键向上翻页，用PageDown键向下翻页。要退出less程序，应按Q键。

###  语法 

```shell
less(选项)(参数)
```

###  选项 

```shell
-e：文件内容显示完毕后，自动退出；
-f：强制显示文件；
-g：不加亮显示搜索到的所有关键词，仅显示当前显示的关键字，以提高显示速度；
-l：搜索时忽略大小写的差异；
-N：每一行行首显示行号；
-s：将连续多个空行压缩成一行显示；
-S：在单行显示较长的内容，而不换行显示；
-x<数字>：将TAB字符显示为指定个数的空格字符。
-r：能够显示设置的颜色。
```

###  参数 

文件：指定要分屏显示内容的文件。

## 实例

```shell
sudo less /var/log/shadowsocks.log

/字符串：向下搜索"字符串"的功能
?字符串：向上搜索"字符串"的功能
n：继续向后搜索
N：向前搜索
b: 向后翻一页
d: 向后翻半页
u: 向前滚动半页
y: 向前滚动一行
Q: 退出less 命令
空格键: 滚动一页
回车键: 滚动一行
[pagedown]： 向下翻动一页
[pageup]： 向上翻动一页
G: 移动到最后一行
g: 移动到第一行

```



tail
===

在屏幕上显示指定文件的末尾若干行

## 补充说明

**tail命令** 用于输入文件中的尾部内容。
- 默认在屏幕上显示指定文件的末尾10行。
- 处理多个文件时会在各个文件之前附加含有文件名的行。
- 如果没有指定文件或者文件名为`-`，则读取标准输入。
- 如果表示字节或行数的`NUM`值之前有一个`+`号，则从文件开头的第`NUM`项开始显示，而不是显示文件的最后`NUM`项。
- `NUM`值后面可以有后缀：
  - `b`  : 512
  - `kB` : 1000
  - `k ` : 1024
  - `MB` : 1000 * 1000
  - `M ` : 1024 * 1024
  - `GB` : 1000 * 1000 * 1000
  - `G ` : 1024 * 1024 * 1024
  - `T`、`P`、`E`、`Z`、`Y`等以此类推。

### 语法

```shell
tail (选项) (参数)
```

### 选项

```shell
-c, --bytes=NUM                 输出文件尾部的NUM（NUM为整数）个字节内容。
-f, --follow[={name|descript}]  显示文件最新追加的内容。“name”表示以文件名的方式监视文件的变化。
-F                              与 “--follow=name --retry” 功能相同。
-n, --line=NUM                  输出文件的尾部NUM（NUM位数字）行内容。
--pid=<进程号>                  与“-f”选项连用，当指定的进程号的进程终止后，自动退出tail命令。
-q, --quiet, --silent           当有多个文件参数时，不输出各个文件名。
--retry                         即是在tail命令启动时，文件不可访问或者文件稍后变得不可访问，都始终尝试打开文件。使用此选项时需要与选项“--follow=name”连用。
-s, --sleep-interal=<秒数>      与“-f”选项连用，指定监视文件变化时间隔的秒数。
-v, --verbose                   当有多个文件参数时，总是输出各个文件名。
--help                          显示指令的帮助信息。
--version                       显示指令的版本信息。
```

### 参数

文件列表：指定要显示尾部内容的文件列表。

### 实例

```shell
tail file #（显示文件file的最后10行）
tail -n +20 file #（显示文件file的内容，从第20行至文件末尾）
tail -c 10 file #（显示文件file的最后10个字节）

tail -25 mail.log # 显示 mail.log 最后的 25 行
tail -f mail.log # 等同于--follow=descriptor，根据文件描述符进行追踪，当文件改名或被删除，追踪停止
tail -F mail.log # 等同于--follow=name --retry，根据文件名进行追踪，并保持重试，即该文件被删除或改名后，如果再次创建相同的文件名，会继续追踪
```





ip
===

网络配置工具

## 补充说明

**ip命令** 用来显示或操纵Linux主机的路由、网络设备、策略路由和隧道，是Linux下较新的功能强大的网络配置工具。

###  语法 

```shell
ip(选项)(对象)
Usage: ip [ OPTIONS ] OBJECT { COMMAND | help }
       ip [ -force ] -batch filename
```

###  对象 

```shell
OBJECT := { link | address | addrlabel | route | rule | neigh | ntable |
       tunnel | tuntap | maddress | mroute | mrule | monitor | xfrm |
       netns | l2tp | macsec | tcp_metrics | token }
       
-V：显示指令版本信息；
-s：输出更详细的信息；
-f：强制使用指定的协议族；
-4：指定使用的网络层协议是IPv4协议；
-6：指定使用的网络层协议是IPv6协议；
-0：输出信息每条记录输出一行，即使内容较多也不换行显示；
-r：显示主机时，不使用IP地址，而使用主机的域名。
```

###  选项

```shell
OPTIONS := { -V[ersion] | -s[tatistics] | -d[etails] | -r[esolve] |
        -h[uman-readable] | -iec |
        -f[amily] { inet | inet6 | ipx | dnet | bridge | link } |
        -4 | -6 | -I | -D | -B | -0 |
        -l[oops] { maximum-addr-flush-attempts } |
        -o[neline] | -t[imestamp] | -ts[hort] | -b[atch] [filename] |
        -rc[vbuf] [size] | -n[etns] name | -a[ll] }
        
网络对象：指定要管理的网络对象；
具体操作：对指定的网络对象完成具体操作；
help：显示网络对象支持的操作命令的帮助信息。
```

###  实例 

```shell
ip link show                    # 显示网络接口信息
ip link set eth0 up             # 开启网卡
ip link set eth0 down            # 关闭网卡
ip link set eth0 promisc on      # 开启网卡的混合模式
ip link set eth0 promisc offi    # 关闭网卡的混合模式
ip link set eth0 txqueuelen 1200 # 设置网卡队列长度
ip link set eth0 mtu 1400        # 设置网卡最大传输单元
ip addr show     # 显示网卡IP信息
ip addr add 192.168.0.1/24 dev eth0 # 为eth0网卡添加一个新的IP地址192.168.0.1
ip addr del 192.168.0.1/24 dev eth0 # 为eth0网卡删除一个IP地址192.168.0.1

ip route show # 显示系统路由
ip route add default via 192.168.1.254   # 设置系统默认路由
ip route list                 # 查看路由信息
ip route add 192.168.4.0/24  via  192.168.0.254 dev eth0 # 设置192.168.4.0网段的网关为192.168.0.254,数据走eth0接口
ip route add default via  192.168.0.254  dev eth0        # 设置默认网关为192.168.0.254
ip route del 192.168.4.0/24   # 删除192.168.4.0网段的网关
ip route del default          # 删除默认路由
ip route delete 192.168.1.0/24 dev eth0 # 删除路由
```

**用ip命令显示网络设备的运行状态** 

```shell
[root@localhost ~]# ip link list
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 00:16:3e:00:1e:51 brd ff:ff:ff:ff:ff:ff
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 00:16:3e:00:1e:52 brd ff:ff:ff:ff:ff:ff
```

**显示更加详细的设备信息** 

```shell
[root@localhost ~]# ip -s link list
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 16436 qdisc noqueue
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    RX: bytes  packets  errors  dropped overrun mcast   
    5082831    56145    0       0       0       0      
    TX: bytes  packets  errors  dropped carrier collsns
    5082831    56145    0       0       0       0      
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 00:16:3e:00:1e:51 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    3641655380 62027099 0       0       0       0      
    TX: bytes  packets  errors  dropped carrier collsns
    6155236    89160    0       0       0       0      
3: eth1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast qlen 1000
    link/ether 00:16:3e:00:1e:52 brd ff:ff:ff:ff:ff:ff
    RX: bytes  packets  errors  dropped overrun mcast   
    2562136822 488237847 0       0       0       0      
    TX: bytes  packets  errors  dropped carrier collsns
    3486617396 9691081  0       0       0       0     
```

**显示核心路由表** 

```shell
[root@localhost ~]# ip route list 
112.124.12.0/22 dev eth1  proto kernel  scope link  src 112.124.15.130
10.160.0.0/20 dev eth0  proto kernel  scope link  src 10.160.7.81
192.168.0.0/16 via 10.160.15.247 dev eth0
172.16.0.0/12 via 10.160.15.247 dev eth0
10.0.0.0/8 via 10.160.15.247 dev eth0
default via 112.124.15.247 dev eth1
```

**显示邻居表** 

```shell
[root@localhost ~]# ip neigh list
112.124.15.247 dev eth1 lladdr 00:00:0c:9f:f3:88 REACHABLE
10.160.15.247 dev eth0 lladdr 00:00:0c:9f:f2:c0 STALE
```

**获取主机所有网络接口**

```shell
ip link | grep -E '^[0-9]' | awk -F: '{print $2}'
```



ifconfig
===

配置和显示Linux系统网卡的网络参数

## 补充说明

**ifconfig命令** 被用于配置和显示Linux内核中网络接口的网络参数。用ifconfig命令配置的网卡信息，在网卡重启后机器重启后，配置就不存在。要想将上述的配置信息永远的存的电脑里，那就要修改网卡的配置文件了。

###  语法 

```shell
ifconfig(参数)
```

###  参数 

```shell
add<地址>：设置网络设备IPv6的ip地址；
del<地址>：删除网络设备IPv6的IP地址；
down：关闭指定的网络设备；
<hw<网络设备类型><硬件地址>：设置网络设备的类型与硬件地址；
io_addr<I/O地址>：设置网络设备的I/O地址；
irq<IRQ地址>：设置网络设备的IRQ；
media<网络媒介类型>：设置网络设备的媒介类型；
mem_start<内存地址>：设置网络设备在主内存所占用的起始地址；
metric<数目>：指定在计算数据包的转送次数时，所要加上的数目；
mtu<字节>：设置网络设备的MTU；
netmask<子网掩码>：设置网络设备的子网掩码；
tunnel<地址>：建立IPv4与IPv6之间的隧道通信地址；
up：启动指定的网络设备；
-broadcast<地址>：将要送往指定地址的数据包当成广播数据包来处理；
-pointopoint<地址>：与指定地址的网络设备建立直接连线，此模式具有保密功能；
-promisc：关闭或启动指定网络设备的promiscuous模式；
IP地址：指定网络设备的IP地址；
网络设备：指定网络设备的名称。
```

###  实例 

 **显示网络设备信息（激活状态的）：** 

```shell
[root@localhost ~]# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:16:3E:00:1E:51  
          inet addr:10.160.7.81  Bcast:10.160.15.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:61430830 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88534 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3607197869 (3.3 GiB)  TX bytes:6115042 (5.8 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:56103 errors:0 dropped:0 overruns:0 frame:0
          TX packets:56103 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:5079451 (4.8 MiB)  TX bytes:5079451 (4.8 MiB)
```

说明：

**eth0** 表示第一块网卡，其中`HWaddr`表示网卡的物理地址，可以看到目前这个网卡的物理地址(MAC地址）是`00:16:3E:00:1E:51`。

**inet addr** 用来表示网卡的IP地址，此网卡的IP地址是`10.160.7.81`，广播地址`Bcast:10.160.15.255`，掩码地址`Mask:255.255.240.0`。

**lo** 是表示主机的回坏地址，这个一般是用来测试一个网络程序，但又不想让局域网或外网的用户能够查看，只能在此台主机上运行和查看所用的网络接口。比如把 httpd服务器的指定到回坏地址，在浏览器输入127.0.0.1就能看到你所架WEB网站了。但只是您能看得到，局域网的其它主机或用户无从知道。

*   第一行：连接类型：Ethernet（以太网）HWaddr（硬件mac地址）。
*   第二行：网卡的IP地址、子网、掩码。
*   第三行：UP（代表网卡开启状态）RUNNING（代表网卡的网线被接上）MULTICAST（支持组播）MTU:1500（最大传输单元）：1500字节。
*   第四、五行：接收、发送数据包情况统计。
*   第七行：接收、发送数据字节数统计信息。

**启动关闭指定网卡：** 

```shell
ifconfig eth0 up
ifconfig eth0 down
```

`ifconfig eth0 up`为启动网卡eth0，`ifconfig eth0 down`为关闭网卡eth0。ssh登陆linux服务器操作要小心，关闭了就不能开启了，除非你有多网卡。

**为网卡配置和删除IPv6地址：** 

```shell
ifconfig eth0 add 33ffe:3240:800:1005::2/64    #为网卡eth0配置IPv6地址
ifconfig eth0 del 33ffe:3240:800:1005::2/64    #为网卡eth0删除IPv6地址
```

**用ifconfig修改MAC地址：** 

```shell
ifconfig eth0 hw ether 00:AA:BB:CC:dd:EE
```

**配置IP地址：** 

```shell
[root@localhost ~]# ifconfig eth0 192.168.2.10
[root@localhost ~]# ifconfig eth0 192.168.2.10 netmask 255.255.255.0
[root@localhost ~]# ifconfig eth0 192.168.2.10 netmask 255.255.255.0 broadcast 192.168.2.255
```

**启用和关闭arp协议：** 

```shell
ifconfig eth0 arp    #开启网卡eth0 的arp协议
ifconfig eth0 -arp   #关闭网卡eth0 的arp协议
```

**设置最大传输单元：** 

```shell
ifconfig eth0 mtu 1500    #设置能通过的最大数据包大小为 1500 bytes
```

**其它实例**

```shell
ifconfig   #处于激活状态的网络接口
ifconfig -a  #所有配置的网络接口，不论其是否激活
ifconfig eth0  #显示eth0的网卡信息
```




ping
===

测试主机之间网络的连通性(ipv4)

## 补充说明

**ping命令** 用来测试主机之间网络的连通性。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

###  语法

```shell
ping (选项) (参数)
```

###  选项

```shell
-d：使用Socket的SO_DEBUG功能；
-c<完成次数>：设置完成要求回应的次数；
-f：极限检测；
-i<间隔秒数>：指定收发信息的间隔时间；
-I<网络界面>：使用指定的网络界面送出数据包；
-l<前置载入>：设置在送出要求信息之前，先行发出的数据包；
-n：只输出数值；
-p<范本样式>：设置填满数据包的范本样式；
-q：不显示指令执行过程，开头和结尾的相关信息除外；
-r：忽略普通的Routing Table，直接将数据包送到远端主机上；
-R：记录路由过程；
-s<数据包大小>：设置数据包的大小；
-t<存活数值>：设置存活数值TTL的大小；
-v：详细显示指令的执行过程。
-w<超时秒数>：无论之前发送或接受了多少包，只要超过此秒数，程序退出；
```

###  参数

目的主机：指定发送ICMP报文的目的主机。

###  实例

```shell
[root@AY1307311912260196fcZ ~]# ping www.jsdig.com
PING host.1.jsdig.com (100.42.212.8) 56(84) bytes of data.
64 bytes from 100-42-212-8.static.webnx.com (100.42.212.8): icmp_seq=1 ttl=50 time=177 ms
64 bytes from 100-42-212-8.static.webnx.com (100.42.212.8): icmp_seq=2 ttl=50 time=178 ms
64 bytes from 100-42-212-8.static.webnx.com (100.42.212.8): icmp_seq=3 ttl=50 time=174 ms
64 bytes from 100-42-212-8.static.webnx.com (100.42.212.8): icmp_seq=4 ttl=50 time=177 ms
...按Ctrl+C结束

--- host.1.jsdig.com ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 2998ms
rtt min/avg/max/mdev = 174.068/176.916/178.182/1.683 ms
```



traceroute
===

显示数据包到主机间的路径

## 补充说明

**traceroute命令** 用于追踪数据包在网络上的传输时的全部路径，它默认发送的数据包大小是40字节。

通过traceroute我们可以知道信息从你的计算机到互联网另一端的主机是走的什么路径。当然每次数据包由某一同样的出发点（source）到达某一同样的目的地(destination)走的路径可能会不一样，但基本上来说大部分时候所走的路由是相同的。

traceroute通过发送小的数据包到目的设备直到其返回，来测量其需要多长时间。一条路径上的每个设备traceroute要测3次。输出结果中包括每次测试的时间(ms)和设备的名称（如有的话）及其ip地址。

###  语法 

```shell
traceroute(选项)(参数)
```

###  选项 

```shell
-d：使用Socket层级的排错功能；
-f<存活数值>：设置第一个检测数据包的存活数值TTL的大小；
-F：设置勿离断位；
-g<网关>：设置来源路由网关，最多可设置8个；
-i<网络界面>：使用指定的网络界面送出数据包；
-I：使用ICMP回应取代UDP资料信息；
-m<存活数值>：设置检测数据包的最大存活数值TTL的大小；
-n：直接使用IP地址而非主机名称；
-p<通信端口>：设置UDP传输协议的通信端口；
-r：忽略普通的Routing Table，直接将数据包送到远端主机上。
-s<来源地址>：设置本地主机送出数据包的IP地址；
-t<服务类型>：设置检测数据包的TOS数值；
-v：详细显示指令的执行过程；
-w<超时秒数>：设置等待远端主机回报的时间；
-x：开启或关闭数据包的正确性检验。
```

###  参数 

主机：指定目的主机IP地址或主机名。

###  实例 

```shell
traceroute www.58.com
traceroute to www.58.com (211.151.111.30), 30 hops max, 40 byte packets
 1  unknown (192.168.2.1)  3.453 ms  3.801 ms  3.937 ms
 2  221.6.45.33 (221.6.45.33)  7.768 ms  7.816 ms  7.840 ms
 3  221.6.0.233 (221.6.0.233)  13.784 ms  13.827 ms 221.6.9.81 (221.6.9.81)  9.758 ms
 4  221.6.2.169 (221.6.2.169)  11.777 ms 122.96.66.13 (122.96.66.13)  34.952 ms 221.6.2.53 (221.6.2.53)  41.372 ms
 5  219.158.96.149 (219.158.96.149)  39.167 ms  39.210 ms  39.238 ms
 6  123.126.0.194 (123.126.0.194)  37.270 ms 123.126.0.66 (123.126.0.66)  37.163 ms  37.441 ms
 7  124.65.57.26 (124.65.57.26)  42.787 ms  42.799 ms  42.809 ms
 8  61.148.146.210 (61.148.146.210)  30.176 ms 61.148.154.98 (61.148.154.98)  32.613 ms  32.675 ms
 9  202.106.42.102 (202.106.42.102)  44.563 ms  44.600 ms  44.627 ms
10  210.77.139.150 (210.77.139.150)  53.302 ms  53.233 ms  53.032 ms
11  211.151.104.6 (211.151.104.6)  39.585 ms  39.502 ms  39.598 ms
12  211.151.111.30 (211.151.111.30)  35.161 ms  35.938 ms  36.005 ms
```

记录按序列号从1开始，每个纪录就是一跳 ，每跳表示一个网关，我们看到每行有三个时间，单位是ms，其实就是`-q`的默认参数。探测数据包向每个网关发送三个数据包后，网关响应后返回的时间；如果用`traceroute -q 4 www.58.com`，表示向每个网关发送4个数据包。

有时我们traceroute一台主机时，会看到有一些行是以星号表示的。出现这样的情况，可能是防火墙封掉了ICMP的返回信息，所以我们得不到什么相关的数据包返回数据。

有时我们在某一网关处延时比较长，有可能是某台网关比较阻塞，也可能是物理设备本身的原因。当然如果某台DNS出现问题时，不能解析主机名、域名时，也会 有延时长的现象；您可以加`-n`参数来避免DNS解析，以IP格式输出数据。

如果在局域网中的不同网段之间，我们可以通过traceroute 来排查问题所在，是主机的问题还是网关的问题。如果我们通过远程来访问某台服务器遇到问题时，我们用到traceroute 追踪数据包所经过的网关，提交IDC服务商，也有助于解决问题；但目前看来在国内解决这样的问题是比较困难的，就是我们发现问题所在，IDC服务商也不可能帮助我们解决。

**跳数设置**

```shell
[root@localhost ~]# traceroute -m 10 www.baidu.com
traceroute to www.baidu.com (61.135.169.105), 10 hops max, 40 byte packets
 1  192.168.74.2 (192.168.74.2)  1.534 ms  1.775 ms  1.961 ms
 2  211.151.56.1 (211.151.56.1)  0.508 ms  0.514 ms  0.507 ms
 3  211.151.227.206 (211.151.227.206)  0.571 ms  0.558 ms  0.550 ms
 4  210.77.139.145 (210.77.139.145)  0.708 ms  0.729 ms  0.785 ms
 5  202.106.42.101 (202.106.42.101)  7.978 ms  8.155 ms  8.311 ms
 6  bt-228-037.bta.net.cn (202.106.228.37)  772.460 ms bt-228-025.bta.net.cn (202.106.228.25)  2.152 ms 61.148.154.97 (61.148.154.97)  772.107 ms
 7  124.65.58.221 (124.65.58.221)  4.875 ms 61.148.146.29 (61.148.146.29)  2.124 ms 124.65.58.221 (124.65.58.221)  4.854 ms
 8  123.126.6.198 (123.126.6.198)  2.944 ms 61.148.156.6 (61.148.156.6)  3.505 ms 123.126.6.198 (123.126.6.198)  2.885 ms
 9  * * *
10  * * *
```

其它一些实例

```shell
traceroute -m 10 www.baidu.com # 跳数设置
traceroute -n www.baidu.com    # 显示IP地址，不查主机名
traceroute -p 6888 www.baidu.com  # 探测包使用的基本UDP端口设置6888
traceroute -q 4 www.baidu.com  # 把探测包的个数设置为值4
traceroute -r www.baidu.com    # 绕过正常的路由表，直接发送到网络相连的主机
traceroute -w 3 www.baidu.com  # 把对外发探测包的等待响应时间设置为3秒
```



crontab
===

提交和管理用户的需要周期性执行的任务

## 补充说明

**crontab命令** 被用来提交和管理用户的需要周期性执行的任务，与windows下的计划任务类似，当安装完成操作系统后，默认会安装此服务工具，并且会自动启动crond进程，crond进程每分钟会定期检查是否有要执行的任务，如果有要执行的任务，则自动执行该任务。

###  语法

```shell
crontab(选项)(参数)
```

###  选项

```shell
-e：编辑该用户的计时器设置；
-l：列出该用户的计时器设置；
-r：删除该用户的计时器设置；
-u<用户名称>：指定要设定计时器的用户名称。
```

###  参数

crontab文件：指定包含待执行任务的crontab文件。

###  知识扩展

Linux下的任务调度分为两类： **系统任务调度** 和 **用户任务调度** 。

 **系统任务调度：** 系统周期性所要执行的工作，比如写缓存数据到硬盘、日志清理等。在`/etc`目录下有一个crontab文件，这个就是系统任务调度的配置文件。

`/etc/crontab`文件包括下面几行：

```shell
SHELL=/bin/bash
PATH=/sbin:/bin:/usr/sbin:/usr/bin
MAILTO=""HOME=/

# run-parts
51 * * * * root run-parts /etc/cron.hourly
24 7 * * * root run-parts /etc/cron.daily
22 4 * * 0 root run-parts /etc/cron.weekly
42 4 1 * * root run-parts /etc/cron.monthly
```

前四行是用来配置crond任务运行的环境变量，第一行SHELL变量指定了系统要使用哪个shell，这里是bash，第二行PATH变量指定了系统执行命令的路径，第三行MAILTO变量指定了crond的任务执行信息将通过电子邮件发送给root用户，如果MAILTO变量的值为空，则表示不发送任务执行信息给用户，第四行的HOME变量指定了在执行命令或者脚本时使用的主目录。

 **用户任务调度：** 用户定期要执行的工作，比如用户数据备份、定时邮件提醒等。用户可以使用 crontab 工具来定制自己的计划任务。所有用户定义的crontab文件都被保存在`/var/spool/cron`目录中。其文件名与用户名一致，使用者权限文件如下：

```shell
/etc/cron.deny     该文件中所列用户不允许使用crontab命令
/etc/cron.allow    该文件中所列用户允许使用crontab命令
/var/spool/cron/   所有用户crontab文件存放的目录,以用户名命名
```

crontab文件的含义：用户所建立的crontab文件中，每一行都代表一项任务，每行的每个字段代表一项设置，它的格式共分为六个字段，前五段是时间设定段，第六段是要执行的命令段，格式如下：

```shell
minute   hour   day   month   week   command     顺序：分 时 日 月 周
```

其中：

*   minute： 表示分钟，可以是从0到59之间的任何整数。
*   hour：表示小时，可以是从0到23之间的任何整数。
*   day：表示日期，可以是从1到31之间的任何整数。
*   month：表示月份，可以是从1到12之间的任何整数。
*   week：表示星期几，可以是从0到7之间的任何整数，这里的0或7代表星期日。
*   command：要执行的命令，可以是系统命令，也可以是自己编写的脚本文件。

在以上各个字段中，还可以使用以下特殊字符：

*   星号（*）：代表所有可能的值，例如month字段如果是星号，则表示在满足其它字段的制约条件后每月都执行该命令操作。
*   逗号（,）：可以用逗号隔开的值指定一个列表范围，例如，“1,2,5,7,8,9”
*   中杠（-）：可以用整数之间的中杠表示一个整数范围，例如“2-6”表示“2,3,4,5,6”
*   正斜线（/）：可以用正斜线指定时间的间隔频率，例如“0-23/2”表示每两小时执行一次。同时正斜线可以和星号一起使用，例如*/10，如果用在minute字段，表示每十分钟执行一次。

**crond服务** 

```shell
/sbin/service crond start    # 启动服务
/sbin/service crond stop     # 关闭服务
/sbin/service crond restart  # 重启服务
/sbin/service crond reload   # 重新载入配置
```

查看crontab服务状态：

```shell
service crond status
```

手动启动crontab服务：

```shell
service crond start
```

查看crontab服务是否已设置为开机启动，执行命令：

```shell
ntsysv
```

加入开机自动启动：

```shell
chkconfig –level 35 crond on
```

###  实例

每1分钟执行一次command

```shell
* * * * * command
```

每小时的第3和第15分钟执行

```shell
3,15 * * * * command
```

在上午8点到11点的第3和第15分钟执行

```shell
3,15 8-11 * * * command
```

每隔两天的上午8点到11点的第3和第15分钟执行

```shell
3,15 8-11 */2 * * command
```

每个星期一的上午8点到11点的第3和第15分钟执行

```shell
3,15 8-11 * * 1 command
```

每晚的21:30重启smb 

```shell
30 21 * * * /etc/init.d/smb restart
```

每月1、10、22日的4 : 45重启smb 

```shell
45 4 1,10,22 * * /etc/init.d/smb restart
```

每周六、周日的1:10重启smb

```shell
10 1 * * 6,0 /etc/init.d/smb restart
```

每天18 : 00至23 : 00之间每隔30分钟重启smb 

```shell
0,30 18-23 * * * /etc/init.d/smb restart
```

每星期六的晚上11:00 pm重启smb 

```shell
0 23 * * 6 /etc/init.d/smb restart
```

每一小时重启smb 

```shell
0 */1 * * * /etc/init.d/smb restart
```

晚上11点到早上7点之间，每隔一小时重启smb

```shell
0 23-7/1 * * * /etc/init.d/smb restart
```

每月的4号与每周一到周三的11点重启smb 

```shell
0 11 4 * mon-wed /etc/init.d/smb restart
```

一月一号的4点重启smb

```shell
0 4 1 jan * /etc/init.d/smb restart
```

每小时执行`/etc/cron.hourly`目录内的脚本

```shell
01 * * * * root run-parts /etc/cron.hourly
```



tar
===

将许多文件一起保存至一个单独的磁带或磁盘归档，并能从归档中单独还原所需文件。

## 补充说明

**tar命令** 可以为linux的文件和目录创建档案。利用tar，可以为某一特定文件创建档案（备份文件），也可以在档案中改变文件，或者向档案中加入新的文件。tar最初被用来在磁带上创建档案，现在，用户可以在任何设备上创建档案。利用tar命令，可以把一大堆的文件和目录全部打包成一个文件，这对于备份文件或将几个文件组合成为一个文件以便于网络传输是非常有用的。

首先要弄清两个概念：打包和压缩。打包是指将一大堆文件或目录变成一个总的文件；压缩则是将一个大的文件通过一些压缩算法变成一个小文件。

为什么要区分这两个概念呢？这源于Linux中很多压缩程序只能针对一个文件进行压缩，这样当你想要压缩一大堆文件时，你得先将这一大堆文件先打成一个包（tar命令），然后再用压缩程序进行压缩（gzip bzip2命令）。

### 语法

```shell
tar [选项...] [FILE]...
```

### 选项

```shell
-A, --catenate, --concatenate   追加 tar 文件至归档
-c, --create               创建一个新归档
-d, --diff, --compare      找出归档和文件系统的差异
    --delete               从归档(非磁带！)中删除
-r, --append               追加文件至归档结尾
-t, --list                 列出归档内容
    --test-label           测试归档卷标并退出
-u, --update               仅追加比归档中副本更新的文件
-x, --extract, --get       从归档中解出文件

操作修饰符:

      --check-device         当创建增量归档时检查设备号(默认)
  -g, --listed-incremental=FILE   处理新式的 GNU 格式的增量备份
  -G, --incremental          处理老式的 GNU 格式的增量备份
      --ignore-failed-read
                             当遇上不可读文件时不要以非零值退出
      --level=NUMBER         所创建的增量列表归档的输出级别
  -n, --seek                 归档可检索
      --no-check-device      当创建增量归档时不要检查设备号
      --no-seek              归档不可检索
      --occurrence[=NUMBER]  仅处理归档中每个文件的第 NUMBER
                             个事件；仅当与以下子命令 --delete,
                             --diff, --extract 或是 --list
                             中的一个联合使用时，此选项才有效。而且不管文件列表是以命令行形式给出或是通过
                             -T 选项指定的；NUMBER 值默认为 1
      --sparse-version=MAJOR[.MINOR]
                             设置所用的离散格式版本(隐含
                             --sparse)
  -S, --sparse               高效处理离散文件

 重写控制:

  -k, --keep-old-files       don't replace existing files when extracting,
                             treat them as errors
      --keep-directory-symlink   preserve existing symlinks to directories when
                             extracting
      --keep-newer-files
                             不要替换比归档中副本更新的已存在的文件
      --no-overwrite-dir     保留已存在目录的元数据
      --overwrite            解压时重写存在的文件
      --overwrite-dir        解压时重写已存在目录的元数据(默认)

      --recursive-unlink     解压目录之前先清除目录层次
      --remove-files         在添加文件至归档后删除它们
      --skip-old-files       don't replace existing files when extracting,
                             silently skip over them
  -U, --unlink-first         在解压要重写的文件之前先删除它们
  -W, --verify               在写入以后尝试校验归档

 选择输出流:

      --ignore-command-error 忽略子进程的退出代码
      --no-ignore-command-error
                             将子进程的非零退出代码认为发生错误
  -O, --to-stdout            解压文件至标准输出
      --to-command=COMMAND
                             将解压的文件通过管道传送至另一个程序

 操作文件属性:

      --atime-preserve[=METHOD]
                             在输出的文件上保留访问时间，要么通过在读取(默认
                             METHOD=‘replace’)后还原时间，要不就不要在第一次(METHOD=‘system’)设置时间
      --delay-directory-restore
                             直到解压结束才设置修改时间和所解目录的权限
      --group=名称         强制将 NAME
                             作为所添加的文件的组所有者
      --mode=CHANGES         强制将所添加的文件(符号)更改为权限
                             CHANGES
      --mtime=DATE-OR-FILE   从 DATE-OR-FILE 中为添加的文件设置
                             mtime
  -m, --touch                不要解压文件的修改时间
      --no-delay-directory-restore
                             取消 --delay-directory-restore 选项的效果
      --no-same-owner
                             将文件解压为您所有(普通用户默认此项)
      --no-same-permissions
                             从归档中解压权限时使用用户的掩码位(默认为普通用户服务)
      --numeric-owner        总是以数字代表用户/组的名称
      --owner=名称         强制将 NAME
                             作为所添加的文件的所有者
  -p, --preserve-permissions, --same-permissions
                             解压文件权限信息(默认只为超级用户服务)
      --preserve             与 -p 和 -s 一样
      --same-owner
                             尝试解压时保持所有者关系一致(超级用户默认此项)
  -s, --preserve-order, --same-order
                             member arguments are listed in the same order as
                             the files in the archive

 Handling of extended file attributes:

      --acls                 Enable the POSIX ACLs support
      --no-acls              Disable the POSIX ACLs support
      --no-selinux           Disable the SELinux context support
      --no-xattrs            Disable extended attributes support
      --selinux              Enable the SELinux context support
      --xattrs               Enable extended attributes support
      --xattrs-exclude=MASK  specify the exclude pattern for xattr keys
      --xattrs-include=MASK  specify the include pattern for xattr keys

 设备选择和切换:

  -f, --file=ARCHIVE         使用归档文件或 ARCHIVE 设备
      --force-local
                             即使归档文件存在副本还是把它认为是本地归档
  -F, --info-script=名称, --new-volume-script=名称
                             在每卷磁带最后运行脚本(隐含 -M)
  -L, --tape-length=NUMBER   写入 NUMBER × 1024 字节后更换磁带
  -M, --multi-volume         创建/列出/解压多卷归档文件
      --rmt-command=COMMAND  使用指定的 rmt COMMAND 代替 rmt
      --rsh-command=COMMAND  使用远程 COMMAND 代替 rsh
      --volno-file=FILE      使用/更新 FILE 中的卷数

 设备分块:

  -b, --blocking-factor=BLOCKS   每个记录 BLOCKS x 512 字节
  -B, --read-full-records    读取时重新分块(只对 4.2BSD 管道有效)
  -i, --ignore-zeros         忽略归档中的零字节块(即文件结尾)
      --record-size=NUMBER   每个记录的字节数 NUMBER，乘以 512

 选择归档格式:

  -H, --format=FORMAT        创建指定格式的归档

 FORMAT 是以下格式中的一种:

    gnu                      GNU tar 1.13.x 格式
    oldgnu                   GNU 格式 as per tar <= 1.12
    pax                      POSIX 1003.1-2001 (pax) 格式
    posix                    等同于 pax
    ustar                    POSIX 1003.1-1988 (ustar) 格式
    v7                       old V7 tar 格式

      --old-archive, --portability
                             等同于 --format=v7
      --pax-option=关键字[[:]=值][,关键字[[:]=值]]...
                             控制 pax 关键字
      --posix                等同于 --format=posix
  -V, --label=TEXT           创建带有卷名 TEXT
                             的归档；在列出/解压时，使用 TEXT
                             作为卷名的模式串

 压缩选项:

  -a, --auto-compress        使用归档后缀名来决定压缩程序
  -I, --use-compress-program=PROG
                             通过 PROG 过滤(必须是能接受 -d
                             选项的程序)
  -j, --bzip2                通过 bzip2 过滤归档
  -J, --xz                   通过 xz 过滤归档
      --lzip                 通过 lzip 过滤归档
      --lzma                 通过 lzma 过滤归档
      --lzop
      --no-auto-compress     不使用归档后缀名来决定压缩程序
  -z, --gzip, --gunzip, --ungzip   通过 gzip 过滤归档
  -Z, --compress, --uncompress   通过 compress 过滤归档

 本地文件选择:

      --add-file=FILE        添加指定的 FILE 至归档(如果名字以 -
                             开始会很有用的)
      --backup[=CONTROL]     在删除前备份，选择 CONTROL 版本
  -C, --directory=DIR        改变至目录 DIR
      --exclude=PATTERN      排除以 PATTERN 指定的文件
      --exclude-backups      排除备份和锁文件
      --exclude-caches       除标识文件本身外，排除包含
                             CACHEDIR.TAG 的目录中的内容
      --exclude-caches-all   排除包含 CACHEDIR.TAG 的目录
      --exclude-caches-under 排除包含 CACHEDIR.TAG 的目录中所有内容

      --exclude-tag=FILE     除 FILE 自身外，排除包含 FILE
                             的目录中的内容
      --exclude-tag-all=FILE 排除包含 FILE 的目录
      --exclude-tag-under=FILE   排除包含 FILE 的目录中的所有内容
      --exclude-vcs          排除版本控制系统目录
  -h, --dereference
                             跟踪符号链接；将它们所指向的文件归档并输出
      --hard-dereference
                             跟踪硬链接；将它们所指向的文件归档并输出
  -K, --starting-file=MEMBER-NAME
                             begin at member MEMBER-NAME when reading the
                             archive
      --newer-mtime=DATE     当只有数据改变时比较数据和时间
      --no-null              禁用上一次的效果 --null 选项
      --no-recursion         避免目录中的自动降级
      --no-unquote           不以 -T 读取的文件名作为引用结束
      --null                 -T 读取以空终止的名字，-C 禁用
  -N, --newer=DATE-OR-FILE, --after-date=DATE-OR-FILE
                             只保存比 DATE-OR-FILE 更新的文件
      --one-file-system      创建归档时保存在本地文件系统中
  -P, --absolute-names       不要从文件名中清除引导符‘/’
      --recursion            目录递归(默认)
      --suffix=STRING        在删除前备份，除非被环境变量
                             SIMPLE_BACKUP_SUFFIX
                             覆盖，否则覆盖常用后缀(‘’)
  -T, --files-from=FILE      从 FILE
                             中获取文件名来解压或创建文件
      --unquote              以 -T
                             读取的文件名作为引用结束(默认)
  -X, --exclude-from=FILE    排除 FILE 中列出的模式串

 文件名变换:

      --strip-components=NUMBER   解压时从文件名中清除 NUMBER
                             个引导部分
      --transform=EXPRESSION, --xform=EXPRESSION
                             使用 sed 代替 EXPRESSION
                             来进行文件名变换

 文件名匹配选项(同时影响排除和包括模式串):

      --anchored             模式串匹配文件名头部
      --ignore-case          忽略大小写
      --no-anchored          模式串匹配任意‘/’后字符(默认对
                             exclusion 有效)
      --no-ignore-case       匹配大小写(默认)
      --no-wildcards         逐字匹配字符串
      --no-wildcards-match-slash   通配符不匹配‘/’
      --wildcards            use wildcards (default)
      --wildcards-match-slash
                             通配符匹配‘/’(默认对排除操作有效)

 提示性输出:

      --checkpoint[=NUMBER]  每隔 NUMBER
                             个记录显示进度信息(默认为 10 个)
      --checkpoint-action=ACTION   在每个检查点上执行 ACTION
      --full-time            print file time to its full resolution
      --index-file=FILE      将详细输出发送至 FILE
  -l, --check-links
                             只要不是所有链接都被输出就打印信息
      --no-quote-chars=STRING   禁用来自 STRING 的字符引用
      --quote-chars=STRING   来自 STRING 的额外的引用字符
      --quoting-style=STYLE  设置名称引用风格；有效的 STYLE
                             值请参阅以下说明
  -R, --block-number         每个信息都显示归档内的块数
      --show-defaults        显示 tar 默认选项
      --show-omitted-dirs
                             列表或解压时，列出每个不匹配查找标准的目录
      --show-transformed-names, --show-stored-names
                             显示变换后的文件名或归档名
      --totals[=SIGNAL]      处理归档后打印出总字节数；当此
                             SIGNAL 被触发时带参数 -
                             打印总字节数；允许的信号为:
                             SIGHUP，SIGQUIT，SIGINT，SIGUSR1 和
                             SIGUSR2；同时也接受不带 SIG
                             前缀的信号名称
      --utc                  以 UTC 格式打印文件修改时间
  -v, --verbose              详细地列出处理的文件
      --warning=KEYWORD      警告控制:
  -w, --interactive, --confirmation
                             每次操作都要求确认

 兼容性选项:

  -o                         创建归档时，相当于
                             --old-archive；展开归档时，相当于
                             --no-same-owner

 其它选项:

  -?, --help                 显示此帮助列表
      --restrict             禁用某些潜在的有危险的选项
      --usage                显示简短的用法说明
      --version              打印程序版本

长选项和相应短选项具有相同的强制参数或可选参数。

除非以 --suffix 或 SIMPLE_BACKUP_SUFFIX
设置备份后缀，否则备份后缀就是“~”。
可以用 --backup 或 VERSION_CONTROL 设置版本控制，可能的值为：

  none, off	   从不做备份
  t, numbered     进行编号备份
  nil, existing
如果编号备份存在则进行编号备份，否则进行简单备份
  never, simple   总是使用简单备份

--quoting-style 选项的有效参数为:

  literal
  shell
  shell-always
  c
  c-maybe
  escape
  locale
  clocale

此 tar 默认为:
--format=gnu -f- -b20 --quoting-style=escape --rmt-command=/etc/rmt
--rsh-command=/usr/bin/ssh
```

### 参数

文件或目录：指定要打包的文件或目录列表。

### 实例

将 `/home/vivek/bin/` 目录打包，并使用 gzip 算法压缩。保存为 `/tmp/bin-backup.tar.gz` 文件。

```
tar -zcvf /tmp/bin-backup.tar.gz /home/vivek/bin/
```

```shell
- z：有gzip属性的
- j：有bz2属性的
- Z：有compress属性的
- v：显示所有过程
- O：将文件解开到标准输出
```

```shell
tar -cf archive.tar foo bar  # 从文件 foo 和 bar 创建归档文件 archive.tar。
tar -tvf archive.tar         # 详细列举归档文件 archive.tar 中的所有文件。
tar -xf archive.tar          # 展开归档文件 archive.tar 中的所有文件。
```


下面的参数-f是必须的

-f: 使用档案名字，切记，这个参数是最后一个参数，后面只能接档案名。

```shell
tar -cf all.tar *.jpg
# 这条命令是将所有.jpg的文件打成一个名为all.tar的包。-c是表示产生新的包，-f指定包的文件名。

tar -rf all.tar *.gif
# 这条命令是将所有.gif的文件增加到all.tar的包里面去。-r是表示增加文件的意思。

tar -uf all.tar logo.gif
# 这条命令是更新原来tar包all.tar中logo.gif文件，-u是表示更新文件的意思。

tar -tf all.tar
# 这条命令是列出all.tar包中所有文件，-t是列出文件的意思
```

```shell
tar -cvf archive.tar foo bar  # 从文件foo和bar创建archive.tar。
tar -tvf archive.tar         # 详细列出archive.tar中的所有文件。
tar -xf archive.tar          # 从archive.tar提取所有文件。
```

#### zip格式

压缩： zip -r [目标文件名].zip [原文件/目录名]  
解压： unzip [原文件名].zip  
注：-r参数代表递归  

#### tar格式（该格式仅仅打包，不压缩）

打包：tar -cvf [目标文件名].tar [原文件名/目录名]  
解包：tar -xvf [原文件名].tar  
注：c参数代表create（创建），x参数代表extract（解包），v参数代表verbose（详细信息），f参数代表filename（文件名），所以f后必须接文件名。  

#### tar.gz格式

方式一：利用前面已经打包好的tar文件，直接用压缩命令。

压缩：gzip [原文件名].tar  
解压：gunzip [原文件名].tar.gz  

方式二：一次性打包并压缩、解压并解包

打包并压缩： tar -zcvf [目标文件名].tar.gz [原文件名/目录名]  
解压并解包： tar -zxvf [原文件名].tar.gz  
注：z代表用gzip算法来压缩/解压。  

#### tar.bz2格式

方式一：利用已经打包好的tar文件，直接执行压缩命令：

压缩：bzip2 [原文件名].tar  
解压：bunzip2 [原文件名].tar.bz2  
方式二：一次性打包并压缩、解压并解包  

打包并压缩： tar -jcvf [目标文件名].tar.bz2 [原文件名/目录名]  
解压并解包： tar -jxvf [原文件名].tar.bz2  
注：小写j代表用bzip2算法来压缩/解压。  

#### tar.xz格式

方式一：利用已经打包好的tar文件，直接用压缩命令：

压缩：xz [原文件名].tar  
解压：unxz [原文件名].tar.xz  
方式二：一次性打包并压缩、解压并解包  

打包并压缩： tar -Jcvf [目标文件名].tar.xz [原文件名/目录名]  
解压并解包： tar -Jxvf [原文件名].tar.xz  
注：大写J代表用xz算法来压缩/解压。  

#### tar.Z格式（已过时）

方式一：利用已经打包好的tar文件，直接用压缩命令：

压缩：compress [原文件名].tar  
解压：uncompress [原文件名].tar.Z  
方式二：一次性打包并压缩、解压并解包  

打包并压缩： tar -Zcvf [目标文件名].tar.Z [原文件名/目录名]  
解压并解包： tar -Zxvf [原文件名].tar.Z  
注：大写Z代表用ncompress算法来压缩/解压。另，ncompress是早期Unix系统的压缩格式，但由于ncompress的压缩率太低，现已过时。  

#### jar格式

压缩：jar -cvf [目标文件名].jar [原文件名/目录名]  
解压：jar -xvf [原文件名].jar  

注：如果是打包的是Java类库，并且该类库中存在主类，那么需要写一个META-INF/MANIFEST.MF配置文件，内容如下：  

```shell
Manifest-Version: 1.0
Created-By: 1.6.0_27 (Sun Microsystems Inc.)
Main-class: the_name_of_the_main_class_should_be_put_here
```

然后用如下命令打包：

jar -cvfm [目标文件名].jar META-INF/MANIFEST.MF [原文件名/目录名]  
这样以后就能用“java -jar [文件名].jar”命令直接运行主类中的public static void main方法了。  

#### 7z格式

压缩：7z a [目标文件名].7z [原文件名/目录名]  
解压：7z x [原文件名].7z  
注：这个7z解压命令支持rar格式，即：  

7z x [原文件名].rar

#### 其它例子

**将文件全部打包成tar包** ：

```shell
tar -cvf log.tar log2012.log    仅打包，不压缩！
tar -zcvf log.tar.gz log2012.log   打包后，以 gzip 压缩
tar -jcvf log.tar.bz2 log2012.log  打包后，以 bzip2 压缩
```

在选项`f`之后的文件档名是自己取的，我们习惯上都用 .tar 来作为辨识。 如果加`z`选项，则以.tar.gz或.tgz来代表gzip压缩过的tar包；如果加`j`选项，则以.tar.bz2来作为tar包名。


**解压目录**

参数--strip-components NUMBER，在提取时从文件名中删除NUMBER个前导组件，如要去除前二层，参数为--strip-components 2

```shell
tar -xvf portal-web-v2.0.0.tar --strip-components 1  -C 指定目录
```

示例

```shell
tar -xvf xxx.tar.gz -C /usr/src/a
/usr/src/a/xxxxx/src/opp/b.txt
```

```shell
tar -xvf xxx.tar.gz -strip-components=1 -C /usr/src/a
/usr/src/a/src/opp/b.txt
```

**查阅上述tar包内有哪些文件** ：

```shell
tar -ztvf log.tar.gz
```

由于我们使用 gzip 压缩的log.tar.gz，所以要查阅log.tar.gz包内的文件时，就得要加上`z`这个选项了。

**将tar包解压缩** ：

```shell
tar -zxvf /opt/soft/test/log.tar.gz
```

在预设的情况下，我们可以将压缩档在任何地方解开的

**只将tar内的部分文件解压出来** ：

```shell
tar -zxvf /opt/soft/test/log30.tar.gz log2013.log
```

我可以透过`tar -ztvf`来查阅 tar 包内的文件名称，如果单只要一个文件，就可以透过这个方式来解压部分文件！

**文件备份下来，并且保存其权限** ：

```shell
tar -zcvpf log31.tar.gz log2014.log log2015.log log2016.log
```

这个`-p`的属性是很重要的，尤其是当您要保留原本文件的属性时。

**在文件夹当中，比某个日期新的文件才备份** ：

```shell
tar -N "2012/11/13" -zcvf log17.tar.gz test
```

**备份文件夹内容是排除部分文件：**

```shell
tar --exclude scf/service -zcvf scf.tar.gz scf/*
```

**打包文件之后删除源文件：**

```shell
tar -cvf test.tar test --remove-files
```

**其实最简单的使用 tar 就只要记忆底下的方式即可：**

```shell
压　缩：tar -jcv -f filename.tar.bz2 要被压缩的文件或目录名称
查　询：tar -jtv -f filename.tar.bz2
解压缩：tar -jxv -f filename.tar.bz2 -C 欲解压缩的目录
```




gzip
===

用来压缩文件

## 补充说明

**gzip命令** 用来压缩文件。gzip是个使用广泛的压缩程序，文件经它压缩过后，其名称后面会多处“.gz”扩展名。

gzip是在Linux系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。gzip不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和tar命令一起构成Linux操作系统中比较流行的压缩文件格式。据统计，gzip命令对文本文件有60%～70%的压缩率。减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。

### 语法

```shell
gzip(选项)(参数)
```

### 选项

```shell
-a或——ascii：使用ASCII文字模式；
-d或--decompress或----uncompress：解开压缩文件；
-f或——force：强行压缩文件。不理会文件名称或硬连接是否存在以及该文件是否为符号连接；
-h或——help：在线帮助；
-l或——list：列出压缩文件的相关信息；
-L或——license：显示版本与版权信息；
-n或--no-name：压缩文件时，不保存原来的文件名称及时间戳记；
-N或——name：压缩文件时，保存原来的文件名称及时间戳记；
-q或——quiet：不显示警告信息；
-r或——recursive：递归处理，将指定目录下的所有文件及子目录一并处理；
-S或<压缩字尾字符串>或----suffix<压缩字尾字符串>：更改压缩字尾字符串；
-t或——test：测试压缩文件是否正确无误；
-v或——verbose：显示指令执行过程；
-V或——version：显示版本信息；
-<压缩效率>：压缩效率是一个介于1~9的数值，预设值为“6”，指定愈大的数值，压缩效率就会愈高；
--best：此参数的效果和指定“-9”参数相同；
--fast：此参数的效果和指定“-1”参数相同。
-num 用指定的数字num调整压缩的速度，-1或--fast表示最快压缩方法（低压缩比），-9或--best表示最慢压缩方法（高压缩比）。系统缺省值为6。
-c或--stdout或--to-stdout：保留原始文件，生成标准输出流（结合重定向使用）。
```

### 参数

文件列表：指定要压缩的文件列表。

### 实例

把test6目录下的每个文件压缩成.gz文件

```shell
gzip *
```

把上例中每个压缩的文件解压，并列出详细的信息

```shell
gzip -dv *
```

详细显示例1中每个压缩的文件的信息，并不解压

```shell
gzip -l *
```

压缩一个tar备份文件，此时压缩文件的扩展名为.tar.gz

```shell
gzip -r log.tar
```

递归的压缩目录

```shell
gzip -rv test6
```

这样，所有test下面的文件都变成了*.gz，目录依然存在只是目录里面的文件相应变成了*.gz.这就是压缩，和打包不同。因为是对目录操作，所以需要加上-r选项，这样也可以对子目录进行递归了。

递归地解压目录

```shell
gzip -dr test6
```

保留原始文件，把压缩/解压流重定向到新文件

```shell
gzip -c aa > aa.gz
gzip -dc bb.gz > bb
```




systemctl
===

系统服务管理器指令

## 补充说明

**systemctl命令** 是系统服务管理器指令，它实际上将 service 和 chkconfig 这两个命令组合到一起。

| 任务 | 旧指令 | 新指令 |
| ---- | ---- | ---- |
| 使某服务自动启动 | chkconfig --level 3 httpd on | systemctl enable httpd.service |
| 使某服务不自动启动 | chkconfig --level 3 httpd off | systemctl disable httpd.service |
| 检查服务状态 | service httpd status | systemctl status httpd.service （服务详细信息） systemctl is-active httpd.service （仅显示是否 Active) |
| 显示所有已启动的服务 | chkconfig --list | systemctl list-units --type=service |
| 启动服务 | service httpd start | systemctl start httpd.service |
| 停止服务 | service httpd stop | systemctl stop httpd.service |
| 重启服务 | service httpd restart | systemctl restart httpd.service |
| 重载服务 | service httpd reload | systemctl reload httpd.service |

### 实例

```shell
systemctl start nfs-server.service . # 启动nfs服务
systemctl enable nfs-server.service # 设置开机自启动
systemctl disable nfs-server.service # 停止开机自启动
systemctl status nfs-server.service # 查看服务当前状态
systemctl restart nfs-server.service # 重新启动某服务
systemctl list-units --type=service # 查看所有已启动的服务
```

开启防火墙22端口

```shell
iptables -I INPUT -p tcp --dport 22 -j accept
```

如果仍然有问题，就可能是SELinux导致的

关闭SElinux：

修改`/etc/selinux/config`文件中的`SELINUX=""`为disabled，然后重启。

彻底关闭防火墙：

```shell
sudo systemctl status firewalld.service
sudo systemctl stop firewalld.service          
sudo systemctl disable firewalld.service
```




service
===

控制系统服务的实用工具

## 补充说明

**service命令** 是Redhat Linux兼容的发行版中用来控制系统服务的实用工具，它以启动、停止、重新启动和关闭系统服务，还可以显示所有系统服务的当前状态。

###  语法

```shell
service(选项)(参数)
```

###  选项

```shell
-h：显示帮助信息；
--status-all：显示所服务的状态。
```

###  参数

*   服务名：自动要控制的服务名，即`/etc/init.d`目录下的脚本文件名；
*   控制命令：系统服务脚本支持的控制命令。

###  实例

当修改了主机名、ip地址等信息时，经常需要把网络重启使之生效。

```shell
service network status
配置设备：
lo eth0
当前的活跃设备：
lo eth0

service network restart
正在关闭接口 eth0：                                        [  确定  ]
关闭环回接口：                                             [  确定  ]
设置网络参数：                                             [  确定  ]
弹出环回接口：                                             [  确定  ]
弹出界面 eth0：                                            [  确定  ]
```

重启mysql

```shell
service mysqld status
mysqld (pid 1638) 正在运行...

service mysqld restart
停止 MySQL：                                               [  确定  ]
启动 MySQL：                                               [  确定  ]
```



iptables
===

Linux上常用的防火墙软件

## 补充说明

**iptables命令** 是Linux上常用的防火墙软件，是netfilter项目的一部分。可以直接配置，也可以通过许多前端和图形界面配置。

<!-- TOC -->

- [补充说明](#补充说明)
  - [语法](#语法)
  - [选项](#选项)
- [基本参数](#基本参数)
    - [命令选项输入顺序](#命令选项输入顺序)
    - [工作机制](#工作机制)
    - [防火墙的策略](#防火墙的策略)
    - [防火墙的策略](#防火墙的策略-1)
  - [实例](#实例)
    - [清空当前的所有规则和计数](#清空当前的所有规则和计数)
    - [配置允许ssh端口连接](#配置允许ssh端口连接)
    - [允许本地回环地址可以正常使用](#允许本地回环地址可以正常使用)
    - [设置默认的规则](#设置默认的规则)
    - [配置白名单](#配置白名单)
    - [开启相应的服务端口](#开启相应的服务端口)
    - [保存规则到配置文件中](#保存规则到配置文件中)
    - [列出已设置的规则](#列出已设置的规则)
    - [清除已有规则](#清除已有规则)
    - [删除已添加的规则](#删除已添加的规则)
    - [开放指定的端口](#开放指定的端口)
    - [屏蔽IP](#屏蔽ip)
    - [指定数据包出去的网络接口](#指定数据包出去的网络接口)
    - [查看已添加的规则](#查看已添加的规则)
    - [启动网络转发规则](#启动网络转发规则)
    - [端口映射](#端口映射)
    - [字符串匹配](#字符串匹配)
    - [阻止Windows蠕虫的攻击](#阻止windows蠕虫的攻击)
    - [防止SYN洪水攻击](#防止syn洪水攻击)

<!-- /TOC -->

### 语法

```shell
iptables(选项)(参数)
```

### 选项

```shell
-t, --table table 对指定的表 table 进行操作， table 必须是 raw， nat，filter，mangle 中的一个。如果不指定此选项，默认的是 filter 表。

# 通用匹配：源地址目标地址的匹配
-p：指定要匹配的数据包协议类型；
-s, --source [!] address[/mask] ：把指定的一个／一组地址作为源地址，按此规则进行过滤。当后面没有 mask 时，address 是一个地址，比如：192.168.1.1；当 mask 指定时，可以表示一组范围内的地址，比如：192.168.1.0/255.255.255.0。
-d, --destination [!] address[/mask] ：地址格式同上，但这里是指定地址为目的地址，按此进行过滤。
-i, --in-interface [!] <网络接口name> ：指定数据包的来自来自网络接口，比如最常见的 eth0 。注意：它只对 INPUT，FORWARD，PREROUTING 这三个链起作用。如果没有指定此选项， 说明可以来自任何一个网络接口。同前面类似，"!" 表示取反。
-o, --out-interface [!] <网络接口name> ：指定数据包出去的网络接口。只对 OUTPUT，FORWARD，POSTROUTING 三个链起作用。

# 查看管理命令
-L, --list [chain] 列出链 chain 上面的所有规则，如果没有指定链，列出表上所有链的所有规则。

# 规则管理命令
-A, --append chain rule-specification 在指定链 chain 的末尾插入指定的规则，也就是说，这条规则会被放到最后，最后才会被执行。规则是由后面的匹配来指定。
-I, --insert chain [rulenum] rule-specification 在链 chain 中的指定位置插入一条或多条规则。如果指定的规则号是1，则在链的头部插入。这也是默认的情况，如果没有指定规则号。
-D, --delete chain rule-specification -D, --delete chain rulenum 在指定的链 chain 中删除一个或多个指定规则。
-R num：Replays替换/修改第几条规则

# 链管理命令（这都是立即生效的）
-P, --policy chain target ：为指定的链 chain 设置策略 target。注意，只有内置的链才允许有策略，用户自定义的是不允许的。
-F, --flush [chain] 清空指定链 chain 上面的所有规则。如果没有指定链，清空该表上所有链的所有规则。
-N, --new-chain chain 用指定的名字创建一个新的链。
-X, --delete-chain [chain] ：删除指定的链，这个链必须没有被其它任何规则引用，而且这条上必须没有任何规则。如果没有指定链名，则会删除该表中所有非内置的链。
-E, --rename-chain old-chain new-chain ：用指定的新名字去重命名指定的链。这并不会对链内部造成任何影响。
-Z, --zero [chain] ：把指定链，或者表中的所有链上的所有计数器清零。

-j, --jump target <指定目标> ：即满足某条件时该执行什么样的动作。target 可以是内置的目标，比如 ACCEPT，也可以是用户自定义的链。
-h：显示帮助信息；
```

## 基本参数

| 参数 | 作用 |
| ---- | ---- |
| -P |  设置默认策略:iptables -P INPUT (DROP|ACCEPT) |
| -F |  清空规则链 |
| -L |  查看规则链 |
| -A |  在规则链的末尾加入新规则 |
| -I | num  在规则链的头部加入新规则 |
| -D | num  删除某一条规则 |
| -s |  匹配来源地址IP/MASK，加叹号"!"表示除这个IP外。 |
| -d |  匹配目标地址 |
| -i | 网卡名称 匹配从这块网卡流入的数据 |
| -o | 网卡名称 匹配从这块网卡流出的数据 |
| -p |  匹配协议,如tcp,udp,icmp |
| --dport num | 匹配目标端口号 |
| --sport num | 匹配来源端口号 |

#### 命令选项输入顺序

```shell
iptables -t 表名 <-A/I/D/R> 规则链名 [规则号] <-i/o 网卡名> -p 协议名 <-s 源IP/源子网> --sport 源端口 <-d 目标IP/目标子网> --dport 目标端口 -j 动作
```

#### 工作机制

规则链名包括(也被称为五个钩子函数（hook functions）)：

- **INPUT链** ：处理输入数据包。
- **OUTPUT链** ：处理输出数据包。
- **FORWARD链** ：处理转发数据包。
- **PREROUTING链** ：用于目标地址转换（DNAT）。
- **POSTOUTING链** ：用于源地址转换（SNAT）。

#### 防火墙的策略

防火墙策略一般分为两种，一种叫`通`策略，一种叫`堵`策略，通策略，默认门是关着的，必须要定义谁能进。堵策略则是，大门是洞开的，但是你必须有身份认证，否则不能进。所以我们要定义，让进来的进来，让出去的出去，`所以通，是要全通，而堵，则是要选择`。当我们定义的策略的时候，要分别定义多条功能，其中：定义数据包中允许或者不允许的策略，filter过滤的功能，而定义地址转换的功能的则是nat选项。为了让这些功能交替工作，我们制定出了“表”这个定义，来定义、区分各种不同的工作功能和处理方式。

我们现在用的比较多个功能有3个：

1. filter 定义允许或者不允许的，只能做在3个链上：INPUT ，FORWARD ，OUTPUT
2. nat 定义地址转换的，也只能做在3个链上：PREROUTING ，OUTPUT ，POSTROUTING
3. mangle功能:修改报文原数据，是5个链都可以做：PREROUTING，INPUT，FORWARD，OUTPUT，POSTROUTING

我们修改报文原数据就是来修改TTL的。能够实现将数据包的元数据拆开，在里面做标记/修改内容的。而防火墙标记，其实就是靠mangle来实现的。

小扩展:

- 对于filter来讲一般只能做在3个链上：INPUT ，FORWARD ，OUTPUT
- 对于nat来讲一般也只能做在3个链上：PREROUTING ，OUTPUT ，POSTROUTING
- 而mangle则是5个链都可以做：PREROUTING，INPUT，FORWARD，OUTPUT，POSTROUTING

iptables/netfilter（这款软件）是工作在用户空间的，它可以让规则进行生效的，本身不是一种服务，而且规则是立即生效的。而我们iptables现在被做成了一个服务，可以进行启动，停止的。启动，则将规则直接生效，停止，则将规则撤销。

iptables还支持自己定义链。但是自己定义的链，必须是跟某种特定的链关联起来的。在一个关卡设定，指定当有数据的时候专门去找某个特定的链来处理，当那个链处理完之后，再返回。接着在特定的链中继续检查。

注意：规则的次序非常关键，`谁的规则越严格，应该放的越靠前`，而检查规则的时候，是按照从上往下的方式进行检查的。


表名包括：

- **raw** ：高级功能，如：网址过滤。
- **mangle** ：数据包修改（QOS），用于实现服务质量。
- **nat** ：地址转换，用于网关路由器。
- **filter** ：包过滤，用于防火墙规则。

动作包括：

- **ACCEPT** ：接收数据包。
- **DROP** ：丢弃数据包。
- **REDIRECT** ：重定向、映射、透明代理。
- **SNAT** ：源地址转换。
- **DNAT** ：目标地址转换。
- **MASQUERADE** ：IP伪装（NAT），用于ADSL。
- **LOG** ：日志记录。
- **SEMARK** : 添加SEMARK标记以供网域内强制访问控制（MAC）

```shell
                             ┏╍╍╍╍╍╍╍╍╍╍╍╍╍╍╍┓
 ┌───────────────┐           ┃    Network    ┃
 │ table: filter │           ┗━━━━━━━┳━━━━━━━┛
 │ chain: INPUT  │◀────┐             │
 └───────┬───────┘     │             ▼
         │             │   ┌───────────────────┐
  ┌      ▼      ┐      │   │ table: nat        │
  │local process│      │   │ chain: PREROUTING │
  └             ┘      │   └─────────┬─────────┘
         │             │             │
         ▼             │             ▼              ┌─────────────────┐
┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅    │     ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅      │table: nat       │
 Routing decision      └───── outing decision ─────▶│chain: PREROUTING│
┅┅┅┅┅┅┅┅┅┳┅┅┅┅┅┅┅┅┅          ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅      └────────┬────────┘
         │                                                   │
         ▼                                                   │
 ┌───────────────┐                                           │
 │ table: nat    │           ┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅┅               │
 │ chain: OUTPUT │    ┌─────▶ outing decision ◀──────────────┘
 └───────┬───────┘    │      ┅┅┅┅┅┅┅┅┳┅┅┅┅┅┅┅┅
         │            │              │
         ▼            │              ▼
 ┌───────────────┐    │   ┌────────────────────┐
 │ table: filter │    │   │ chain: POSTROUTING │
 │ chain: OUTPUT ├────┘   └──────────┬─────────┘
 └───────────────┘                   │
                                     ▼
                             ┏╍╍╍╍╍╍╍╍╍╍╍╍╍╍╍┓
                             ┃    Network    ┃
                             ┗━━━━━━━━━━━━━━━┛
```


### 实例

#### 清空当前的所有规则和计数

```shell
iptables -F  # 清空所有的防火墙规则
iptables -X  # 删除用户自定义的空链
iptables -Z  # 清空计数
```

#### 配置允许ssh端口连接

```shell
iptables -A INPUT -s 192.168.1.0/24 -p tcp --dport 22 -j ACCEPT
# 22为你的ssh端口， -s 192.168.1.0/24表示允许这个网段的机器来连接，其它网段的ip地址是登陆不了你的机器的。 -j ACCEPT表示接受这样的请求
```

#### 允许本地回环地址可以正常使用

```shell
iptables -A INPUT -i lo -j ACCEPT
#本地圆环地址就是那个127.0.0.1，是本机上使用的,它进与出都设置为允许
iptables -A OUTPUT -o lo -j ACCEPT
```

#### 设置默认的规则

```shell
iptables -P INPUT DROP # 配置默认的不让进
iptables -P FORWARD DROP # 默认的不允许转发
iptables -P OUTPUT ACCEPT # 默认的可以出去
```

#### 配置白名单

```shell
iptables -A INPUT -p all -s 192.168.1.0/24 -j ACCEPT  # 允许机房内网机器可以访问
iptables -A INPUT -p all -s 192.168.140.0/24 -j ACCEPT  # 允许机房内网机器可以访问
iptables -A INPUT -p tcp -s 183.121.3.7 --dport 3380 -j ACCEPT # 允许183.121.3.7访问本机的3380端口
```

#### 开启相应的服务端口

```shell
iptables -A INPUT -p tcp --dport 80 -j ACCEPT # 开启80端口，因为web对外都是这个端口
iptables -A INPUT -p icmp --icmp-type 8 -j ACCEPT # 允许被ping
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT # 已经建立的连接得让它进来
```

#### 保存规则到配置文件中

```shell
cp /etc/sysconfig/iptables /etc/sysconfig/iptables.bak # 任何改动之前先备份，请保持这一优秀的习惯
iptables-save > /etc/sysconfig/iptables
cat /etc/sysconfig/iptables
```

#### 列出已设置的规则

> iptables -L [-t 表名] [链名]

- 四个表名 `raw`，`nat`，`filter`，`mangle`
- 五个规则链名 `INPUT`、`OUTPUT`、`FORWARD`、`PREROUTING`、`POSTROUTING`
- filter表包含`INPUT`、`OUTPUT`、`FORWARD`三个规则链

```shell
iptables -L -t nat                  # 列出 nat 上面的所有规则
#            ^ -t 参数指定，必须是 raw， nat，filter，mangle 中的一个
iptables -L -t nat  --line-numbers  # 规则带编号
iptables -L INPUT

iptables -L -nv  # 查看，这个列表看起来更详细
```

#### 清除已有规则

```shell
iptables -F INPUT  # 清空指定链 INPUT 上面的所有规则
iptables -X INPUT  # 删除指定的链，这个链必须没有被其它任何规则引用，而且这条上必须没有任何规则。
                   # 如果没有指定链名，则会删除该表中所有非内置的链。
iptables -Z INPUT  # 把指定链，或者表中的所有链上的所有计数器清零。
```

#### 删除已添加的规则

```shell
# 添加一条规则
iptables -A INPUT -s 192.168.1.5 -j DROP
```

将所有iptables以序号标记显示，执行：

```shell
iptables -L -n --line-numbers
```

比如要删除INPUT里序号为8的规则，执行：

```shell
iptables -D INPUT 8
```

#### 开放指定的端口

```shell
iptables -A INPUT -s 127.0.0.1 -d 127.0.0.1 -j ACCEPT               #允许本地回环接口(即运行本机访问本机)
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT    #允许已建立的或相关连的通行
iptables -A OUTPUT -j ACCEPT         #允许所有本机向外的访问
iptables -A INPUT -p tcp --dport 22 -j ACCEPT    #允许访问22端口
iptables -A INPUT -p tcp --dport 80 -j ACCEPT    #允许访问80端口
iptables -A INPUT -p tcp --dport 21 -j ACCEPT    #允许ftp服务的21端口
iptables -A INPUT -p tcp --dport 20 -j ACCEPT    #允许FTP服务的20端口
iptables -A INPUT -j reject       #禁止其他未允许的规则访问
iptables -A FORWARD -j REJECT     #禁止其他未允许的规则访问
```

#### 屏蔽IP

```shell
iptables -A INPUT -p tcp -m tcp -s 192.168.0.8 -j DROP  # 屏蔽恶意主机（比如，192.168.0.8
iptables -I INPUT -s 123.45.6.7 -j DROP       #屏蔽单个IP的命令
iptables -I INPUT -s 123.0.0.0/8 -j DROP      #封整个段即从123.0.0.1到123.255.255.254的命令
iptables -I INPUT -s 124.45.0.0/16 -j DROP    #封IP段即从123.45.0.1到123.45.255.254的命令
iptables -I INPUT -s 123.45.6.0/24 -j DROP    #封IP段即从123.45.6.1到123.45.6.254的命令是
```

#### 指定数据包出去的网络接口

只对 OUTPUT，FORWARD，POSTROUTING 三个链起作用。

```shell
iptables -A FORWARD -o eth0
```

#### 查看已添加的规则

```shell
iptables -L -n -v
Chain INPUT (policy DROP 48106 packets, 2690K bytes)
 pkts bytes target     prot opt in     out     source               destination
 5075  589K ACCEPT     all  --  lo     *       0.0.0.0/0            0.0.0.0/0
 191K   90M ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0           tcp dpt:22
1499K  133M ACCEPT     tcp  --  *      *       0.0.0.0/0            0.0.0.0/0           tcp dpt:80
4364K 6351M ACCEPT     all  --  *      *       0.0.0.0/0            0.0.0.0/0           state RELATED,ESTABLISHED
 6256  327K ACCEPT     icmp --  *      *       0.0.0.0/0            0.0.0.0/0

Chain FORWARD (policy ACCEPT 0 packets, 0 bytes)
 pkts bytes target     prot opt in     out     source               destination

Chain OUTPUT (policy ACCEPT 3382K packets, 1819M bytes)
 pkts bytes target     prot opt in     out     source               destination
 5075  589K ACCEPT     all  --  *      lo      0.0.0.0/0            0.0.0.0/0
```

#### 启动网络转发规则

公网`210.14.67.7`让内网`192.168.188.0/24`上网

```shell
iptables -t nat -A POSTROUTING -s 192.168.188.0/24 -j SNAT --to-source 210.14.67.127
```

#### 端口映射

本机的 2222 端口映射到内网 虚拟机的22 端口

```shell
iptables -t nat -A PREROUTING -d 210.14.67.127 -p tcp --dport 2222  -j DNAT --to-dest 192.168.188.115:22
```

#### 字符串匹配

比如，我们要过滤所有TCP连接中的字符串`test`，一旦出现它我们就终止这个连接，我们可以这么做：

```shell
iptables -A INPUT -p tcp -m string --algo kmp --string "test" -j REJECT --reject-with tcp-reset
iptables -L

# Chain INPUT (policy ACCEPT)
# target     prot opt source               destination
# REJECT     tcp  --  anywhere             anywhere            STRING match "test" ALGO name kmp TO 65535 reject-with tcp-reset
#
# Chain FORWARD (policy ACCEPT)
# target     prot opt source               destination
#
# Chain OUTPUT (policy ACCEPT)
# target     prot opt source               destination
```

#### 阻止Windows蠕虫的攻击

```shell
iptables -I INPUT -j DROP -p tcp -s 0.0.0.0/0 -m string --algo kmp --string "cmd.exe"
```

#### 防止SYN洪水攻击

```shell
iptables -A INPUT -p tcp --syn -m limit --limit 5/second -j ACCEPT
```

#### 添加SECMARK记录
```shell
iptables -t mangle -A INPUT -p tcp --src 192.168.1.2 --dport 443 -j SECMARK --selctx system_u:object_r:myauth_packet_t
# 向从 192.168.1.2:443 以TCP方式发出到本机的包添加MAC安全上下文 system_u:object_r:myauth_packet_t
```

## 更多实例
> 用iptables搭建一套强大的安全防护盾 http://www.imooc.com/learn/389

iptables: linux 下应用层防火墙工具

iptables 5链: 对应 Hook point
netfilter: linux 操作系统核心层内部的一个数据包处理模块
Hook point: 数据包在 netfilter 中的挂载点; `PRE_ROUTING / INPUT / OUTPUT / FORWARD / POST_ROUTING`

iptables & netfilter
![](http://7xq89b.com1.z0.glb.clouddn.com/netfilter&iptables.jpg)

iptables 4表5链
![](http://7xq89b.com1.z0.glb.clouddn.com/iptables-data-stream.jpg)

iptables rules
![](http://7xq89b.com1.z0.glb.clouddn.com/iptables-rules.jpg)

- 4表

**filter**: 访问控制 / 规则匹配
**nat**: 地址转发
 mangle / raw

 - 规则

数据访问控制: ACCEPT / DROP / REJECT
数据包改写(nat -> 地址转换): snat / dnat
信息记录: log

## 使用场景实例
- 场景一

开放 tcp 10-22/80 端口
开放 icmp
其他未被允许的端口禁止访问

存在的问题: 本机无法访问本机; 本机无法访问其他主机

- 场景二

ftp: 默认被动模式(服务器产生随机端口告诉客户端, 客户端主动连接这个端口拉取数据)
vsftpd: 使 ftp 支持主动模式(客户端产生随机端口通知服务器, 服务器主动连接这个端口发送数据)

- 场景三

允许外网访问:
web
http -> 80/tcp; https -> 443/tcp
mail
smtp -> 25/tcp; smtps -> 465/tcp
pop3 -> 110/tcp; pop3s -> 995/tcp
imap -> 143/tcp

内部使用:
file
nfs -> 123/udp
samba -> 137/138/139/445/tcp
ftp -> 20/21/tcp
remote
ssh -> 22/tcp
sql
mysql -> 3306/tcp
oracle -> 1521/tcp

- 场景四

nat 转发

- 场景五

防CC攻击

```shell
iptables -L -F -A -D # list flush append delete
# 场景一
iptables -I INPUT -p tcp --dport 80 -j ACCEPT # 允许 tcp 80 端口
iptables -I INPUT -p tcp --dport 10:22 -j ACCEPT # 允许 tcp 10-22 端口
iptables -I INPUT -p icmp -j ACCEPT # 允许 icmp
iptables -A INPUT -j REJECT # 添加一条规则, 不允许所有

# 优化场景一
iptables -I INPUT -i lo -j ACCEPT # 允许本机访问
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT # 允许访问外网
iptables -I INPUT -p tcp --dport 80 -s 10.10.188.233 -j ACCEPT # 只允许固定ip访问80

# 场景二
vi /etc/vsftpd/vsftpd.conf # 使用 vsftpd 开启 ftp 主动模式
port_enable=yes
connect_from_port_20=YES
iptables -I INPUT -p tcp --dport 21 -j ACCEPT

vi /etc/vsftpd/vsftpd.conf # 建议使用 ftp 被动模式
pasv_min_port=50000
pasv_max_port=60000
iptables -I INPUT -p tcp --dport 21 -j ACCEPT
iptables -I INPUT -p tcp --dport 50000:60000 -j ACCEPT

# 还可以使用 iptables 模块追踪来自动开发对应的端口

# 场景三
iptables -I INPUT -i lo -j ACCEPT # 允许本机访问
iptables -I INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT # 允许访问外网
iptables -I INPUT -s 10.10.155.0/24 -j ACCEPT # 允许内网访问
iptables -I INPUT -p tcp -m multiport --dports 80,1723 -j ACCEPT # 允许端口, 80 -> http, 1723 -> vpn
iptables -A INPUT -j REJECT # 添加一条规则, 不允许所有

iptables-save # 保存设置到配置文件

# 场景四
iptables -t nat -L # 查看 nat 配置

iptables -t nat -A POST_ROUTING -s 10.10.177.0/24 -j SNAT --to 10.10.188.232 # SNAT
vi /etc/sysconfig/network # 配置网关

iptables -t nat -A POST_ROUTING -d 10.10.188.232 -p tcp --dport 80 -j DNAT --to 10.10.177.232:80 # DNAT

#场景五
iptables -I INPUT -p tcp --syn --dport 80 -m connlimit --connlimit-above 100 -j REJECT # 限制并发连接访问数
iptables -I INPUT -m limit --limit 3/hour --limit-burst 10 -j ACCEPT # limit模块; --limit-burst 默认为5
```



lsblk
===

列出块设备信息

## 补充说明

**lsblk命令** 用于列出所有可用块设备的信息，而且还能显示他们之间的依赖关系，但是它不会列出RAM盘的信息。块设备有硬盘，闪存盘，cd-ROM等等。lsblk命令包含在util-linux-ng包中，现在该包改名为util-linux。这个包带了几个其它工具，如dmesg。要安装lsblk，请在此处下载util-linux包。Fedora用户可以通过命令`sudo yum install util-linux-ng`来安装该包。

###  选项

```shell
-a, --all            # 显示所有设备。
-b, --bytes          # 以bytes方式显示设备大小。
-d, --nodeps         # 不显示 slaves 或 holders。
-D, --discard        # print discard capabilities。
-e, --exclude <list> # 排除设备 (default: RAM disks)。
-f, --fs             # 显示文件系统信息。
-h, --help           # 显示帮助信息。
-i, --ascii          # use ascii characters only。
-m, --perms          # 显示权限信息。
-l, --list           # 使用列表格式显示。
-n, --noheadings     # 不显示标题。
-o, --output <list>  # 输出列。
-P, --pairs          # 使用key="value"格式显示。
-r, --raw            # 使用原始格式显示。
-t, --topology       # 显示拓扑结构信息。
```

###  实例

lsblk命令默认情况下将以树状列出所有块设备。打开终端，并输入以下命令：

```shell
lsblk

NAME   MAJ:MIN rm   SIZE RO type mountpoint
sda      8:0    0 232.9G  0 disk 
├─sda1   8:1    0  46.6G  0 part /
├─sda2   8:2    0     1K  0 part 
├─sda5   8:5    0   190M  0 part /boot
├─sda6   8:6    0   3.7G  0 part [SWAP]
├─sda7   8:7    0  93.1G  0 part /data
└─sda8   8:8    0  89.2G  0 part /personal
sr0     11:0    1  1024M  0 rom
```

7个栏目名称如下：

1.   **NAME** ：这是块设备名。
2.   **MAJ:MIN** ：本栏显示主要和次要设备号。
3.   **RM** ：本栏显示设备是否可移动设备。注意，在本例中设备sdb和sr0的RM值等于1，这说明他们是可移动设备。
4.   **SIZE** ：本栏列出设备的容量大小信息。例如298.1G表明该设备大小为298.1GB，而1K表明该设备大小为1KB。
5.   **RO** ：该项表明设备是否为只读。在本案例中，所有设备的RO值为0，表明他们不是只读的。
6.   **TYPE** ：本栏显示块设备是否是磁盘或磁盘上的一个分区。在本例中，sda和sdb是磁盘，而sr0是只读存储（rom）。
7.   **MOUNTPOINT** ：本栏指出设备挂载的挂载点。

默认选项不会列出所有空设备。要查看这些空设备，请使用以下命令：

```shell
lsblk -a
```

lsblk命令也可以用于列出一个特定设备的拥有关系，同时也可以列出组和模式。可以通过以下命令来获取这些信息：

```shell
lsblk -m
```

该命令也可以只获取指定设备的信息。这可以通过在提供给lsblk命令的选项后指定设备名来实现。例如，你可能对了解以字节显示你的磁盘驱动器大小比较感兴趣，那么你可以通过运行以下命令来实现：

```shell
lsblk -b /dev/sda

等价于

lsblk --bytes /dev/sda
```

你也可以组合几个选项来获取指定的输出。例如，你也许想要以列表格式列出设备，而不是默认的树状格式。你可能也对移除不同栏目名称的标题感兴趣。可以将两个不同的选项组合，以获得期望的输出，命令如下：

```shell
lsblk -nl
```

要获取SCSI设备的列表，你只能使用-S选项。该选项是大写字母S，不能和-s选项混淆，该选项是用来以颠倒的顺序打印依赖的。

```shell
lsblk -S
```

lsblk列出SCSI设备，而-s是逆序选项（将设备和分区的组织关系逆转过来显示），其将给出如下输出。输入命令：

```shell
lsblk -s
```



hostnamectl
===

查询或更改系统主机名

## 补充说明

hostnamectl可用于查询和更改系统主机名和相关设置。

### 语法

```bash
hostnamectl [选项...] 指令 ...
```
### 指令

```bash
status                 显示当前主机名设置
set-hostname NAME      设置系统主机名
set-icon-name NAME     设置主机的图标名称
set-chassis NAME       设置主机的机箱类型 
set-deployment NAME    设置主机的部署环境 
set-location NAME      设置主机位置
```

### 选项

```bash
-h --help               显示此帮助
    --version           显示包的版本
    --no-ask-password   不提示输入密码
-H --host=[USER@]HOST   在远程主机上操作
-M --machine=CONTAINER  在本地容器上执行操作。指定要连接到的容器名称。
--transient, --static, --pretty  
                        如果调用了status（或者没有给出显式命令）并且指定了其中一个开关，hostnamectl将只打印出这个选定的主机名。
```

### 实例

显示主机名设置

```bash
$ hostnamectl status
```


改变主机名(永久修改,不用重启哦~)

```bash
$ sudo hostnamectl set-hostname newname
```


cp
===

将源文件或目录复制到目标文件或目录中

## 补充说明

**cp命令** 用来将一个或多个源文件或者目录复制到指定的目的文件或目录。它可以将单个源文件复制成一个指定文件名的具体的文件或一个已经存在的目录下。cp命令还支持同时复制多个文件，当一次复制多个文件时，目标文件参数必须是一个已经存在的目录，否则将出现错误。

###  语法 

```shell
cp(选项)(参数)
```

###  选项 

```shell
-a：此参数的效果和同时指定"-dpR"参数相同；
-d：当复制符号连接时，把目标文件或目录也建立为符号连接，并指向与源文件或目录连接的原始文件或目录；
-f：强行复制文件或目录，不论目标文件或目录是否已存在；
-i：覆盖既有文件之前先询问用户；
-l：对源文件建立硬连接，而非复制文件；
-p：保留源文件或目录的属性；
-R/r：递归处理，将指定目录下的所有文件与子目录一并处理；
-s：对源文件建立符号连接，而非复制文件；
-u：使用这项参数后只会在源文件的更改时间较目标文件更新时或是名称相互对应的目标文件并不存在时，才复制文件；
-S：在备份文件时，用指定的后缀“SUFFIX”代替文件的默认后缀；
-b：覆盖已存在的文件目标前将目标文件备份；
-v：详细显示命令执行的操作。
```

###  参数 

*   源文件：制定源文件列表。默认情况下，cp命令不能复制目录，如果要复制目录，则必须使用`-R`选项；
*   目标文件：指定目标文件。当“源文件”为多个文件时，要求“目标文件”为指定的目录。

###  实例 

下面的第一行中是 cp 命令和具体的参数（-r 是“递归”， -u 是“更新”，-v 是“详细”）。接下来的三行显示被复制文件的信息，最后一行显示命令行提示符。这样，只拷贝新的文件到我的存储设备上，我就使用 cp 的“更新”和“详细”选项。

通常来说，参数 `-r` 也可用更详细的风格 `--recursive`。但是以简短的方式，也可以这么连用 `-ruv`。

```shell
cp -r -u -v /usr/men/tmp ~/men/tmp
```

版本备份 `--backup=numbered` 参数意思为“我要做个备份，而且是带编号的连续备份”。所以一个备份就是 1 号，第二个就是 2 号，等等。

```shell
$ cp --force --backup=numbered test1.py test1.py
$ ls
test1.py test1.py.~1~ test1.py.~2~
```

如果把一个文件复制到一个目标文件中，而目标文件已经存在，那么，该目标文件的内容将被破坏。此命令中所有参数既可以是绝对路径名，也可以是相对路径名。通常会用到点`.`或点点`..`的形式。例如，下面的命令将指定文件复制到当前目录下：

```shell
cp ../mary/homework/assign .
```

所有目标文件指定的目录必须是己经存在的，cp命令不能创建目录。如果没有文件复制的权限，则系统会显示出错信息。

将文件file复制到目录`/usr/men/tmp`下，并改名为file1

```shell
cp file /usr/men/tmp/file1
```

将目录`/usr/men`下的所有文件及其子目录复制到目录`/usr/zh`中

```shell
cp -r /usr/men /usr/zh
```

交互式地将目录`/usr/men`中的以m打头的所有.c文件复制到目录`/usr/zh`中

```shell
cp -i /usr/men m*.c /usr/zh
```

我们在Linux下使用cp命令复制文件时候，有时候会需要覆盖一些同名文件，覆盖文件的时候都会有提示：需要不停的按Y来确定执行覆盖。文件数量不多还好，但是要是几百个估计按Y都要吐血了，于是折腾来半天总结了一个方法：

```shell
cp aaa/* /bbb
# 复制目录aaa下所有到/bbb目录下，这时如果/bbb目录下有和aaa同名的文件，需要按Y来确认并且会略过aaa目录下的子目录。

cp -r aaa/* /bbb
# 这次依然需要按Y来确认操作，但是没有忽略子目录。

cp -r -a aaa/* /bbb
# 依然需要按Y来确认操作，并且把aaa目录以及子目录和文件属性也传递到了/bbb。

\cp -r -a aaa/* /bbb
# 成功，没有提示按Y、传递了目录属性、没有略过目录。
```

递归强制复制目录到指定目录中覆盖已存在文件

```shell
cp -rfb ./* ../backup
# 将当前目录下所有文件，复制到当前目录的兄弟目录 backup 文件夹中
```

拷贝目录下的隐藏文件如 `.babelrc`

```shell
cp -r aaa/.* ./bbb
# 将 aaa 目录下的，所有`.`开头的文件，复制到 bbb 目录中。

cp -a aaa ./bbb/ 
# 记住后面目录最好的'/' 带上 `-a` 参数
```

复制到当前目录

```shell
cp aaa.conf ./
# 将 aaa.conf 复制到当前目录
```


export
===

为shell变量或函数设置导出属性。

## 概要

```
export [-fn] [name[=word]]...
export -p
```

## 主要用途

- 定义一到多个变量并设置导出属性。
- 修改一到多个变量的值并设置导出属性。
- 删除一到多个变量的导出属性。
- 显示全部拥有导出属性的变量。
- 为一到多个已定义函数新增导出属性。
- 删除一到多个函数的导出属性。
- 显示全部拥有导出属性的函数。

## 选项

```shell
-f：指向函数。
-n：删除变量的导出属性。
-p：显示全部拥有导出属性的变量。
-pf：显示全部拥有导出属性的函数。
-nf：删除函数的导出属性。
--：在它之后的选项无效。
```

## 参数

name（可选）：变量名或已定义函数名。

value（可选）：变量的值。

### 返回值

export返回true除非你提供了非法选项或非法名称。

## 例子

```shell
# 显示全部拥有导出属性的变量。
# export -p
# export
# 显示全部拥有导出属性的函数。
# export -pf
```

```shell
# 首先删除要演示的变量名
#unset a b
# 定义变量的同时增加导出属性
export a b=3
# 当然也可以先定义后增加导出属性
b=3
export b

# 修改拥有导出属性的变量的值
export a=5 b=7
# 当然也可以直接赋值修改
a=5;b=7

# 删除变量的导出属性
export -n a b
```


```shell
# 首先删除要演示的函数名
unset func_1 func_2
# 创建函数
function func_1(){ echo '123'; }
function func_2(){ echo '890'; }

# 为已定义函数增加导出属性
export -f func_1 func_2

# 删除函数的导出属性
export -fn a b
```

```shell
# 添加环境变量（JAVA）到`~/.bashrc`
PATH=/usr/local/jdk1.7.0/bin:$PATH
# 添加当前位置到动态库环境变量
export LD_LIBRARY_PATH=$(pwd):${LD_LIBRARY_PATH}
```

## 错误用法

- 对未定义的函数添加导出属性。
- 对没有导出属性的函数/变量执行删除导出属性操作。
- 在 `--` 后使用选项。

## Q&A

#### Q：对变量或函数设置导出属性有什么用？  

A：它们会成为环境变量，可以在脚本中访问它们，尤其是脚本中调用的子进程需要时。（ **[参考链接4][4]** ）

#### Q：如果我编写的脚本修改了已有的环境变量的值，那么执行它会在当前终端生效吗？会影响之前以及之后打开的终端吗？  

A：只有通过`source`方式调用的脚本会生效，您可以查看`source`命令获得更多信息；其他方式只是在子shell中执行。
之前的不会影响，之后的除非是修改了`~/.bashrc`这种启动终端时加载的脚本。（ **[参考链接1][1]** ）

#### Q：我脚本文件中调用`~/.bashrc`中定义的函数和变量。为什么在新打开的终端中通过 `sh` 方式调用该脚本或直接运行

这个当前用户有执行权限的脚本却不能使用这些函数和变量？  
A：请在`~/.bashrc`文件中增加export它们的语句。另请参阅 **知识点** 段落。

#### Q：数组和关联数组也可以设置导出属性吗？

A：是可以的（如果你的bash支持它们），不过有些问题（ **[参考链接2][2]** ）。

#### Q：为什么我在查看变量或函数导出属性的时候显示的开头是`declare`？  

A：因为`declare`也能够设置变量或函数的导出属性，详见`declare`命令。

### 注意

1. 该命令是bash内建命令，相关的帮助信息请查看`help`命令。

### 知识点

在`info bash`或 [bash在线文档](http://www.gnu.org/software/bash/manual/bash.html) 的
 `3.7.3`节提到了shell执行环境，其中涉及变量和函数的内容如下

> - shell parameters that are set by variable assignment or with set or inherited from the shell’s parent in the environment
> - shell functions defined during execution or inherited from the shell’s parent in the environment

那么第一句话中的参数又和变量有什么关系呢？在`3.4`节第一段中提到：

>  A variable is a parameter denoted by a name.

变量是有名字的参数。

那么子shell确实继承了父shell中带有导出属性的变量或函数。

可参考链接： [执行脚本方式的区别](https://blog.csdn.net/soaringlee_fighting/article/details/78759448)


### 参考链接

1. [关于bashrc profile文件的讨论][1]
2. [关于export数组的讨论][2]
3. [export -pf用法][3]
4. [环境变量和shell变量的区别][4]

### 扩展阅读

一般来说，配置交叉编译工具链的时候需要指定编译工具的路径，此时就需要设置环境变量。查看已经存在的环境变量：

```shell
[root@localhost ~]# export
declare -x G_BROKEN_FILENAMES="1"
declare -x HISTSIZE="1000"
declare -x HOME="/root"
declare -x hostname="localhost"
declare -x INPUTRC="/etc/inputrc"
declare -x LANG="zh_CN.UTF-8"
declare -x LESSOPEN="|/usr/bin/lesspipe.sh %s"
declare -x logname="root"
declare -x LS_COLORS="no=00:fi=00:di=01;34:ln=01;36:pi=40;33:so=01;35:bd=40;33;01:cd=40;33;01:or=01;05;37;41:mi=01;05;37;41:ex=01;32:*.cmd=01;32:*.exe=01;32:*.com=01;32:*.btm=01;32:*.bat=01;32:*.sh=01;32:*.csh=01;32:*.tar=01;31:*.tgz=01;31:*.arj=01;31:*.taz=01;31:*.lzh=01;31:*.zip=01;31:*.z=01;31:*.Z=01;31:*.gz=01;31:*.bz2=01;31:*.bz=01;31:*.tz=01;31:*.rpm=01;31:*.cpio=01;31:*.jpg=01;35:*.gif=01;35:*.bmp=01;35:*.xbm=01;35:*.xpm=01;35:*.png=01;35:*.tif=01;35:"
declare -x mail="/var/spool/mail/root"
declare -x OLDPWD
declare -x PATH="/usr/kerberos/sbin:/usr/kerberos/bin:/usr/local/sbin:/usr/local/bin:/sbin:/bin:/usr/sbin:/usr/bin:/root/bin"
declare -x pwd="/root"
declare -x SHELL="/bin/bash"
declare -x SHLVL="1"
declare -x SSH_CLIENT="192.168.2.111 2705 22"
declare -x SSH_CONNECTION="192.168.2.111 2705 192.168.2.2 22"
declare -x SSH_TTY="/dev/pts/0"
declare -x TERM="linux"
declare -x USER="root"
```

[1]: https://www.cnblogs.com/hongzg1982/articles/2101792.html
[2]: https://stackoverflow.com/questions/5564418/exporting-an-array-in-bash-script
[3]: https://unix.stackexchange.com/questions/22796/can-i-export-functions-in-bash
[4]: https://askubuntu.com/questions/26318/environment-variable-vs-shell-variable-whats-the-difference



openssl
===

强大的安全套接字层密码库

## 补充说明

**OpenSSL** 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及SSL协议，并提供丰富的应用程序供测试或其它目的使用。在OpenSSL被曝出现严重安全漏洞后，发现多数通过SSL协议加密的网站使用名为OpenSSL的开源软件包。由于这是互联网应用最广泛的安全传输方法，被网银、在线支付、电商网站、门户网站、电子邮件等重要网站广泛使用，所以该漏洞影响范围广大。

OpenSSL有两种运行模式：交互模式和批处理模式。

直接输入openssl回车进入交互模式，输入带命令选项的openssl进入批处理模式。

OpenSSL整个软件包大概可以分成三个主要的功能部分：密码算法库、SSL协议库以及应用程序。OpenSSL的目录结构自然也是围绕这三个功能部分进行规划的。 

 **对称加密算法**

OpenSSL一共提供了8种对称加密算法，其中7种是分组加密算法，仅有的一种流加密算法是RC4。这7种分组加密算法分别是AES、DES、Blowfish、CAST、IDEA、RC2、RC5，都支持电子密码本模式（ECB）、加密分组链接模式（CBC）、加密反馈模式（CFB）和输出反馈模式（OFB）四种常用的分组密码加密模式。其中，AES使用的加密反馈模式（CFB）和输出反馈模式（OFB）分组长度是128位，其它算法使用的则是64位。事实上，DES算法里面不仅仅是常用的DES算法，还支持三个密钥和两个密钥3DES算法。 

 **非对称加密算法**

OpenSSL一共实现了4种非对称加密算法，包括DH算法、RSA算法、DSA算法和椭圆曲线算法（EC）。DH算法一般用于密钥交换。RSA算法既可以用于密钥交换，也可以用于数字签名，当然，如果你能够忍受其缓慢的速度，那么也可以用于数据加密。DSA算法则一般只用于数字签名。 

 **信息摘要算法**

OpenSSL实现了5种信息摘要算法，分别是MD2、MD5、MDC2、SHA（SHA1）和RIPEMD。SHA算法事实上包括了SHA和SHA1两种信息摘要算法，此外，OpenSSL还实现了DSS标准中规定的两种信息摘要算法DSS和DSS1。 

 **密钥和证书管理**

密钥和证书管理是PKI的一个重要组成部分，OpenSSL为之提供了丰富的功能，支持多种标准。 

首先，OpenSSL实现了ASN.1的证书和密钥相关标准，提供了对证书、公钥、私钥、证书请求以及CRL等数据对象的DER、PEM和BASE64的编解码功能。OpenSSL提供了产生各种公开密钥对和对称密钥的方法、函数和应用程序，同时提供了对公钥和私钥的DER编解码功能。并实现了私钥的PKCS#12和PKCS#8的编解码功能。OpenSSL在标准中提供了对私钥的加密保护功能，使得密钥可以安全地进行存储和分发。 

在此基础上，OpenSSL实现了对证书的X.509标准编解码、PKCS#12格式的编解码以及PKCS#7的编解码功能。并提供了一种文本数据库，支持证书的管理功能，包括证书密钥产生、请求产生、证书签发、吊销和验证等功能。 

事实上，OpenSSL提供的CA应用程序就是一个小型的证书管理中心（CA），实现了证书签发的整个流程和证书管理的大部分机制。

### 实例

**1、使用 openssl 生成密码**

几乎所有 Linux 发行版都包含 openssl。我们可以利用它的随机功能来生成可以用作密码的随机字母字符串。

```shell
openssl rand -base64 10
# nU9LlHO5nsuUvw==
```

nU9LlHO5nsuUvw==

**2、消息摘要算法应用例子**

用SHA1算法计算文件file.txt的哈西值，输出到stdout：

```shell
# openssl dgst -sha1 file.txt
```

用SHA1算法计算文件file.txt的哈西值，输出到文件digest.txt：

```shell
# openssl sha1 -out digest.txt file.txt
```

用DSS1(SHA1)算法为文件file.txt签名，输出到文件dsasign.bin。签名的private key必须为DSA算法产生的，保存在文件dsakey.pem中。

```shell
# openssl dgst -dss1 -sign dsakey.pem -out dsasign.bin file.txt
```

用dss1算法验证file.txt的数字签名dsasign.bin，验证的private key为DSA算法产生的文件dsakey.pem。

```shell
# openssl dgst -dss1 -prverify dsakey.pem -signature dsasign.bin file.txt
```

用sha1算法为文件file.txt签名,输出到文件rsasign.bin，签名的private key为RSA算法产生的文件rsaprivate.pem。

```shell
# openssl sha1 -sign rsaprivate.pem -out rsasign.bin file.txt
```

用sha1算法验证file.txt的数字签名rsasign.bin，验证的public key为RSA算法生成的rsapublic.pem。

```shell
# openssl sha1 -verify rsapublic.pem -signature rsasign.bin file.txt
```

 **3、对称加密应用例子**

对称加密应用例子，用DES3算法的CBC模式加密文件plaintext.doc，加密结果输出到文件ciphertext.bin。

```shell
# openssl enc -des3 -salt -in plaintext.doc -out ciphertext.bin
```

用DES3算法的OFB模式解密文件ciphertext.bin，提供的口令为trousers，输出到文件plaintext.doc。注意：因为模式不同，该命令不能对以上的文件进行解密。

```shell
# openssl enc -des-ede3-ofb -d -in ciphertext.bin -out plaintext.doc -pass pass:trousers
```

用Blowfish的CFB模式加密plaintext.doc，口令从环境变量PASSWORD中取，输出到文件ciphertext.bin。

```shell
# openssl bf-cfb -salt -in plaintext.doc -out ciphertext.bin -pass env:PASSWORD
```

给文件ciphertext.bin用base64编码，输出到文件base64.txt。

```shell
# openssl base64 -in ciphertext.bin -out base64.txt
```

用RC5算法的CBC模式加密文件plaintext.doc，输出到文件ciphertext.bin，salt、key和初始化向量(iv)在命令行指定。

```shell
# openssl rc5 -in plaintext.doc -out ciphertext.bin -S C62CB1D49F158ADC -iv E9EDACA1BD7090C6 -K 89D4B1678D604FAA3DBFFD030A314B29
```

 **4、Diffie-Hellman应用例子**

使用生成因子2和随机的1024-bit的素数产生D0ffie-Hellman参数，输出保存到文件dhparam.pem

```shell
# openssl dhparam -out dhparam.pem -2 1024
```

从dhparam.pem中读取Diffie-Hell参数，以C代码的形式，输出到stdout。

```shell
# openssl dhparam -in dhparam.pem -noout -C
```

 **5、DSA应用例子应用例子**

生成1024位DSA参数集，并输出到文件dsaparam.pem。

```shell
# openssl dsaparam -out dsaparam.pem 1024
```

使用参数文件dsaparam.pem生成DSA私钥匙，采用3DES加密后输出到文件dsaprivatekey.pem

```shell
# openssl gendsa -out dsaprivatekey.pem -des3 dsaparam.pem
```

使用私钥匙dsaprivatekey.pem生成公钥匙，输出到dsapublickey.pem

```shell
# openssl dsa -in dsaprivatekey.pem -pubout -out dsapublickey.pem
```

从dsaprivatekey.pem中读取私钥匙，解密并输入新口令进行加密，然后写回文件dsaprivatekey.pem

```shell
# openssl dsa -in dsaprivatekey.pem -out dsaprivatekey.pem -des3 -passin
```

 **6、RSA应用例子**

产生1024位RSA私匙，用3DES加密它，口令为trousers，输出到文件rsaprivatekey.pem

```shell
# openssl genrsa -out rsaprivatekey.pem -passout pass:trousers -des3 1024
```

从文件rsaprivatekey.pem读取私匙，用口令trousers解密，生成的公钥匙输出到文件rsapublickey.pem

```shell
# openssl rsa -in rsaprivatekey.pem -passin pass:trousers -pubout -out rsapubckey.pem
```

用公钥匙rsapublickey.pem加密文件plain.txt，输出到文件cipher.txt

```shell
# openssl rsautl -encrypt -pubin -inkey rsapublickey.pem -in plain.txt -out cipher.txt
```

使用私钥匙rsaprivatekey.pem解密密文cipher.txt，输出到文件plain.txt

```shell
# openssl rsautl -decrypt -inkey rsaprivatekey.pem -in cipher.txt -out plain.txt
```

用私钥匙rsaprivatekey.pem给文件plain.txt签名，输出到文件signature.bin

```shell
# openssl rsautl -sign -inkey rsaprivatekey.pem -in plain.txt -out signature.bin
```

用公钥匙rsapublickey.pem验证签名signature.bin，输出到文件plain.txt

```shell
# openssl rsautl -verify -pubin -inkey rsapublickey.pem -in signature.bin -out plain
```

从X.509证书文件cert.pem中获取公钥匙，用3DES加密mail.txt，输出到文件mail.enc

```shell
# openssl smime -encrypt -in mail.txt -des3 -out mail.enc cert.pem
```

从X.509证书文件cert.pem中获取接收人的公钥匙，用私钥匙key.pem解密S/MIME消息mail.enc，结果输出到文件mail.txt

```shell
# openssl smime -decrypt -in mail.enc -recip cert.pem -inkey key.pem -out mail.txt
```

cert.pem为X.509证书文件，用私匙key,pem为mail.txt签名，证书被包含在S/MIME消息中，输出到文件mail.sgn

```shell
# openssl smime -sign -in mail.txt -signer cert.pem -inkey key.pem -out mail.sgn
```

验证S/MIME消息mail.sgn，输出到文件mail.txt，签名者的证书应该作为S/MIME消息的一部分包含在mail.sgn中

```shell
# openssl smime -verify -in mail.sgn -out mail.txt
```

更多实例:

```shell
openssl version -a
openssl help
openssl genrsa -aes128 -out fd.key 2048 # pem format
openssl rsa -text -in fd.key
```



yum
===

基于RPM的软件包管理器

## 补充说明

**yum命令** 是在Fedora和RedHat以及SUSE中基于rpm的软件包管理器，它可以使系统管理人员交互和自动化地更新与管理RPM软件包，能够从指定的服务器自动下载RPM包并且安装，可以自动处理依赖性关系，并且一次安装所有依赖的软体包，无须繁琐地一次次下载、安装。

yum提供了查找、安装、删除某一个、一组甚至全部软件包的命令，而且命令简洁而又好记。

### 语法

```shell
yum(选项)(参数)
```

### 选项

```shell
-h：显示帮助信息；
-y：对所有的提问都回答“yes”；
-c：指定配置文件；
-q：安静模式；
-v：详细模式；
-d：设置调试等级（0-10）；
-e：设置错误等级（0-10）；
-R：设置yum处理一个命令的最大等待时间；
-C：完全从缓存中运行，而不去下载或者更新任何头文件。
```

### 参数

```shell
install：安装rpm软件包；
update：更新rpm软件包；
check-update：检查是否有可用的更新rpm软件包；
remove：删除指定的rpm软件包；
list：显示软件包的信息；
search：检查软件包的信息；
info：显示指定的rpm软件包的描述信息和概要信息；
clean：清理yum过期的缓存；
shell：进入yum的shell提示符；
resolvedep：显示rpm软件包的依赖关系；
localinstall：安装本地的rpm软件包；
localupdate：显示本地rpm软件包进行更新；
deplist：显示rpm软件包的所有依赖关系；
provides：查询某个程序所在安装包。
```

### 实例

部分常用的命令包括：

*   自动搜索最快镜像插件：`yum install yum-fastestmirror`
*   安装yum图形窗口插件：`yum install yumex`
*   查看可能批量安装的列表：`yum grouplist`

**安装**

```shell
yum install              #全部安装
yum install package1     #安装指定的安装包package1
yum groupinsall group1   #安装程序组group1
```

**更新和升级**

```shell
yum update               #全部更新
yum update package1      #更新指定程序包package1
yum check-update         #检查可更新的程序
yum upgrade package1     #升级指定程序包package1
yum groupupdate group1   #升级程序组group1
```

**查找和显示**

```shell
# 检查 MySQL 是否已安装
yum list installed | grep mysql
yum list installed mysql*

yum info package1      #显示安装包信息package1
yum list               #显示所有已经安装和可以安装的程序包
yum list package1      #显示指定程序包安装情况package1
yum groupinfo group1   #显示程序组group1信息yum search string 根据关键字string查找安装包
```

**删除程序**

```shell
yum remove &#124; erase package1   #删除程序包package1
yum groupremove group1             #删除程序组group1
yum deplist package1               #查看程序package1依赖情况
```

**清除缓存**

```shell
yum clean packages       # 清除缓存目录下的软件包
yum clean headers        # 清除缓存目录下的 headers
yum clean oldheaders     # 清除缓存目录下旧的 headers
```

**更多实例**

```shell
# yum
/etc/yum.repos.d/       # yum 源配置文件
vi /etc/yum.repos.d/nginx.repo # 举个栗子: nginx yum源
[nginx]
name=nginx repo
baseurl=http://nginx.org/packages/centos/6/$basearch/
gpgcheck=0
enabled=1

# yum mirror
mv /etc/yum.repos.d/CentOS-Base.repo /etc/yum.repos.d/CentOS-Base.repo.bak
wget https://mirror.tuna.tsinghua.edu.cn/help/centos/
yum makecache

# 添加中文语言支持
LANG=C # 原始语言
LANG=zh_CN.utf8 # 切换到中文
yum groupinstall "Chinese Support" # 添加中文语言支持
```




pip
===

Python 编程语言中的包管理器，用于安装和管理第三方 Python 模块

## 语法

```bash
pip <命令> [选项]
```

## 选项

命令

```bash
install                     安装包。
download                    下载包。
uninstall                   卸载包。
freeze                      以requirements格式输出已安装的包。
inspect                     检查 Python 环境。
list                        列出已安装的包。
show                        显示有关已安装包的信息。
check                       验证已安装的包是否具有兼容的依赖关系。
config                      管理本地和全局配置。
search                      在 PyPI 搜索包。
cache                       检查和管理 pip 的wheel缓存。
index                       检查从软件包索引中获取的信息。
wheel                       从你的要求构建wheels。
hash                        计算包存档的哈希值。
completion                  用于命令完成的辅助命令。
debug                       显示用于调试的有用信息。
help                        显示命令的帮助信息。
```

通用选项

```bash
-h, --help                  显示帮助。
--debug                     允许未处理的异常传播到主要子例程之外，而不是将其记录到stderr。
--isolated                  在隔离模式下运行 pip，忽略环境变量和用户配置。
--require-virtualenv        允许 pip 仅在虚拟环境中运行；否则退出并显示错误。
--python <python>           使用指定的 Python 解释器运行 pip。
-v, --verbose               提供更多输出。该选项是可叠加的，最多可使用3次。
-V, --version               显示版本并退出。
-q, --quiet                 提供更少的输出。该选项是可叠加的，最多可使用3次（对应 WARNING、ERROR 和 CRITICAL 日志级别）。
--log <path>                要附加日志的路径。
--no-input                  禁用输入提示。
--keyring-provider <keyring_provider>
                            如果允许用户输入，则启用通过 keyring 库进行凭据查找。指定要使用的机制[disabled, import, subprocess]。（默认: disabled）
--proxy <proxy>             指定代理，格式为 scheme://[user:passwd@]proxy.server:port。
--retries <retries>         每个连接应尝试的最大次数（默认5次）。
--timeout <sec>             设置套接字超时（默认15秒）。
--exists-action <action>    当路径已经存在时的默认操作：(s)切换，(i)忽略，(w)擦除，(b)备份，(a)中止。
--trusted-host <hostname>   将此主机或主机:端口对标记为可信，即使它没有有效或任何 HTTPS。
--cert <path>               PEM编码的CA证书包的路径。如果提供，将覆盖默认值。有关更多信息，请参阅 pip 文档中的 'SSL证书验证'。
--client-cert <path>        SSL客户端证书的路径，一个包含私钥和PEM格式的证书的单个文件。
--cache-dir <dir>           在 <dir> 中存储缓存数据。
--no-cache-dir              禁用缓存。
--disable-pip-version-check
                            不定期检查 PyPI 是否有可下载的 pip 新版本。与 --no-index 隐含。
--no-color                  抑制有色输出。
--no-python-version-warning
                            对即将不受支持的 Python 沉默弃用警告。
--use-feature <feature>     启用可能不向后兼容的新功能。
--use-deprecated <feature>  启用在将来将被删除的已弃用功能。
```

### 安装

Pip 是 Python 的包管理工具，通常随着 Python 的安装一起安装。确保你的 Python 版本是 3.4 或更高版本。

```bash
# Ubuntu系统
sudo apt install python3-pip
```

```bash
# CentOS
sudo yum install python3-pip
```

如果需要更新 Pip，可以运行以下命令：

```bash
python -m pip install --upgrade pip
```

检查 `pip` 是否已安装

```bash
pip --version
```

确保您使用的是最新版本的 `pip`，您可以运行以下命令来**升级**

```bash
python -m pip install --upgrade pip
```

## 安装包

通过 Pip 安装 Python 包非常简单。使用以下命令：

```bash
pip install <package_name>
```

例如，安装一个名为 `requests` 的包：

```bash
pip install requests
```

## 卸载包

要卸载已安装的包，使用以下命令：

```bash
pip uninstall package_name
```

例如，卸载 `requests` 包：

```bash
pip uninstall requests
```

## 查看已安装的包

你可以使用以下命令查看当前环境中已安装的所有包及其版本：

```bash
pip list
```

## 导出和导入依赖关系

使用 `pip freeze` 命令可以将当前环境中的所有包及其版本导出到一个文本文件，通常命名为 `requirements.txt`：

```bash
pip freeze > requirements.txt
```

要在另一个环境中安装相同的依赖，可以使用以下命令：

```bash
pip install -r requirements.txt
```

## 安装特定版本的包

如果需要安装特定版本的包，可以在包名后面添加版本号：

```bash
pip install package_name==1.2.3
```

## 搜索包

要搜索可用的 Python 包，可以使用 `pip search` 命令：

```bash
pip search package_name
```

## 安装开发版本

有时你可能需要安装包的开发版本。通常，开发版本存储在版本控制系统中（如 GitHub）：

```bash
pip install git+https://github.com/user/repo.git
```

这将安装存储库的最新版本。

以上是一些常用的 Pip 命令，希望这个简要教程能够帮助你更好地使用 Python 包管理工具。

## 官网

更多安装使用方法可以访问官网学习：[https://pypi.org/project/pip/](https://pypi.org/project/pip/)

echo
===

输出指定的字符串或者变量

## 补充说明

**echo命令** 用于在shell中打印shell变量的值，或者直接输出指定的字符串。linux的echo命令，在shell编程中极为常用, 在终端下打印变量value的时候也是常常用到的，因此有必要了解下echo的用法echo命令的功能是在显示器上显示一段文字，一般起到一个提示的作用。

###  语法 

```shell
echo(选项)(参数)
```

###  选项 

```shell
-e：启用转义字符。
-E: 不启用转义字符（默认）
-n: 结尾不换行
```

使用`-e`选项时，若字符串中出现以下字符，则特别加以处理，而不会将它当成一般文字输出：

- `\a` 发出警告声；
- `\b` 删除前一个字符；
- `\c` 不产生进一步输出 (\c 后面的字符不会输出)；
- `\f` 换行但光标仍旧停留在原来的位置；
- `\n` 换行且光标移至行首；
- `\r` 光标移至行首，但不换行；
- `\t` 插入tab；
- `\v` 与\f相同；
- `\\` 插入\字符；
- `\nnn` 插入 `nnn`（八进制）所代表的ASCII字符；

###  参数 

变量：指定要打印的变量。

###  实例 

```shell
/bin/echo Hello, world!
```

在上面的命令中，两个词（Hello 和 world！）作为单独的参数传递给 echo，并且 echo 按顺序打印它们，用空格分隔

下一个命令产生相同的输出：

```shell
/bin/echo 'Hello, World!'
```

但是，与第一个示例不同，上述命令提供了单引号字符串 'Hello, world!' 作为一个单一的一个参数。

单引号将可靠地保护它免受 shell 解释，将特殊字符和转义序列逐字传递给 echo。

例如，在 `bash shell` 中，变量名前面有一个美元符号 ($)。 在下一个命令中，引号内的变量名按字面意思处理； 在引号之外，它被转换为它的值。

```shell
/bin/echo 'The value of $PATH is' $PATH
# The value of $PATH is
# /home/hope/bin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin
```

用echo命令打印带有色彩的文字：

**文字色：** 

```shell
echo -e "\e[1;31mThis is red text\e[0m"
This is red text
```

*   `\e[1;31m` 将颜色设置为红色
*   `\e[0m` 将颜色重新置回

颜色码：重置=0，黑色=30，红色=31，绿色=32，黄色=33，蓝色=34，洋红=35，青色=36，白色=37

```shell
echo -e "\x1b[30;1m 0 黑色 \x1b[0m"\
"\x1b[31;1m 1 红色 \x1b[0m"\
"\x1b[32;1m 2 绿色 \x1b[0m"\
"\x1b[33;1m 3 黄色 \x1b[0m"\
"\x1b[34;1m 4 蓝色 \x1b[0m"\
"\x1b[35;1m 5 洋红 \x1b[0m"\
"\x1b[36;1m 6 青色 \x1b[0m"\
"\x1b[37;1m 7 白色 \x1b[0m"
```

 **背景色** ：

```shell
echo -e "\e[1;42mGreed Background\e[0m"
Greed Background
```

颜色码：重置=0，黑色=40，红色=41，绿色=42，黄色=43，蓝色=44，洋红=45，青色=46，白色=47

 **文字闪动：** 

```shell
echo -e "\033[37;31;5mMySQL Server Stop...\033[39;49;0m"
```

红色数字处还有其他数字参数：0 关闭所有属性、1 设置高亮度（加粗）、4 下划线、5 闪烁、7 反显、8 消隐



**输出内容结尾不添加换行符**

```shell
echo -n 'hello'
```

ulimit
===

控制shell程序的资源

## 补充说明

**ulimit命令** 用来限制系统用户对shell资源的访问。如果不懂什么意思，下面一段内容可以帮助你理解：

假设有这样一种情况，当一台 Linux 主机上同时登陆了 10 个人，在系统资源无限制的情况下，这 10 个用户同时打开了 500 个文档，而假设每个文档的大小有 10M，这时系统的内存资源就会受到巨大的挑战。

而实际应用的环境要比这种假设复杂的多，例如在一个嵌入式开发环境中，各方面的资源都是非常紧缺的，对于开启文件描述符的数量，分配堆栈的大 小，CPU 时间，虚拟内存大小，等等，都有非常严格的要求。资源的合理限制和分配，不仅仅是保证系统可用性的必要条件，也与系统上软件运行的性能有着密不可分的联 系。这时，ulimit 可以起到很大的作用，它是一种简单并且有效的实现资源限制的方式。

ulimit 用于限制 shell 启动进程所占用的资源，支持以下各种类型的限制：所创建的内核文件的大小、进程数据块的大小、Shell 进程创建文件的大小、内存锁住的大小、常驻内存集的大小、打开文件描述符的数量、分配堆栈的最大大小、CPU 时间、单个用户的最大线程数、Shell 进程所能使用的最大虚拟内存。同时，它支持硬资源和软资源的限制。

作为临时限制，ulimit 可以作用于通过使用其命令登录的 shell 会话，在会话终止时便结束限制，并不影响于其他 shell 会话。而对于长期的固定限制，ulimit 命令语句又可以被添加到由登录 shell 读取的文件中，作用于特定的 shell 用户。

### 语法

```shell
ulimit(选项)
```

### 选项

```shell
-a：显示目前资源限制的设定；
-c <core文件上限>：设定core文件的最大值，单位为区块；
-d <数据节区大小>：程序数据节区的最大值，单位为KB；
-e 默认进程优先级, 值越小优先级越高
-f <文件大小>：shell所能建立的最大文件，单位为区块；
-H：设定资源的硬性限制，也就是管理员所设下的限制；
-m <内存大小>：指定可使用内存的上限，单位为KB；
-n <文件数目>：指定同一时间最多可开启的文件数；
-p <缓冲区大小>：指定管道缓冲区的大小，单位512字节；
-s <堆叠大小>：指定堆叠的上限，单位为KB；
-S：设定资源的弹性限制；
-t <CPU时间>：指定CPU使用时间的上限，单位为秒；
-u <程序数目>：用户最多可开启的程序数目；
-v <虚拟内存大小>：指定可使用的虚拟内存上限，单位为KB。
```

### 实例

```shell
[root@localhost ~]# ulimit -a
core file size          (blocks, -c) 0           #core文件的最大值为100 blocks。
data seg size           (kbytes, -d) unlimited   #进程的数据段可以任意大。
scheduling priority             (-e) 0
file size               (blocks, -f) unlimited   #文件可以任意大。
pending signals                 (-i) 98304       #最多有98304个待处理的信号。
max locked memory       (kbytes, -l) 32          #一个任务锁住的物理内存的最大值为32KB。
max memory size         (kbytes, -m) unlimited   #一个任务的常驻物理内存的最大值。
open files                      (-n) 1024        #一个任务最多可以同时打开1024的文件。
pipe size            (512 bytes, -p) 8           #管道的最大空间为4096字节。
POSIX message queues     (bytes, -q) 819200      #POSIX的消息队列的最大值为819200字节。
real-time priority              (-r) 0
stack size              (kbytes, -s) 10240       #进程的栈的最大值为10240字节。
cpu time               (seconds, -t) unlimited   #进程使用的CPU时间。
max user processes              (-u) 98304       #当前用户同时打开的进程（包括线程）的最大个数为98304。
virtual memory          (kbytes, -v) unlimited   #没有限制进程的最大地址空间。
file locks                      (-x) unlimited   #所能锁住的文件的最大个数没有限制。
```






<Comment />