---
title: SVN服务仓库搭建记录
date: 2021-12-16 23:41:15
tags:
---
## 目标
在ubuntu上搭建SVN仓库，通过windows访问仓库以记录和管理文档。

## 步骤
1. 检查ubuntu是否以及安装SVN工具
$ svn --version
2. 如果没有SVN则安装SVN工具
$ sudo apt install subversion
3. 再次检查SVN
$ svn --version
svn, version 1.9.7 (r1800392)
   compiled Mar 28 2018, 08:49:13 on x86_64-pc-linux-gnu
   ......
4. 创建SVN仓库测试目录
$ mkdir test
$ cd test/
$ mkdir svn
$ cd svn/
5. 创建第一个SVN仓库，取名为repo1
$ svnadmin create ~/work/test/svn/repo1
$ ls
repo1
$ cd repo1/
$ ls
conf  db  format  hooks  locks  README.txt

6. 进入仓库配置目录
$ cd conf/
$ ls
authz  hooks-env.tmpl  passwd  svnserve.conf

7. 配置仓库
$ 修改文件svnserve.conf以设置以下选项 
anon-access = none
auth-access = write
password-db = passwd
authz-db = authz
realm = My First Repository

8.修改文件passwod,以配置访问密码
//配置用户名为admin,等号右边为admin密码
[users]
admin = admin

9. 修改文件authz,以配置访问权限和路径
[groups]
g_admin = admin

[/]
@g_admin = rw
* = r

10.启动SVN服务,即可通过 svn://192.168.0.110/repo1使用仓库
$ pwd
~/work/test/svn
$ ifconfig获取IP地址为: 192.168.0.110

11. 在windows上安装TortoiseSVN
12. 通过安装的TortoiseSVN工具访问SVN仓库
首次使用示例
1)创建test文件夹
2)鼠标右键->TortoiseSVN->Repo-broswer
3)输入仓库地址 svn://192.168.0.110/repo1
4)输入账号登陆即可浏览仓库
5)也可以鼠标右键->SVN checkout...到本地文件夹
6)更新仓库到最新，修改文件，提交文件到远程仓库


# 后记
今天状态不好，错误的修改了领导的文档，发现不好恢复到原始模样，所以记录一下SVN版本控制工具。
