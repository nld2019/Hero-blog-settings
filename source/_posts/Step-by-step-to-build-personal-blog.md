---
title: Step by step to build personal blog.
date: 2021-12-08 23:48:18
tags:
---
## OS:ubuntu
1.https://nodejs.org/en/ 下载nodejs
2.解压下载的nodejs
``` bash
$ tar -xvf node-v16.13.1-linux-x64.tar.xz
```
3.配置nodejs
``` bash
$ cp node-v16.13.1-linux-x64 ~/nodejs
$ sudo ln -s ~/nodejs/bin/npm /usr/bin/
$ sudo ln -s ~/nodejs/bin/npm /usr/bin/
```
4.安装cnpm
``` bash
$ sudo npm install -g cnpm --registry=https://registry.npm.taobao.org
$ sudo ln -s ~/nodejs/bin/cnpm /usr/bin/
```

5.安装hexo
``` bash
$ cnpm install -g hexo-cli
$ sudo ln -s ~/nodejs/bin/hexo /usr/bin/
```

6.创建目录
``` bash
$ mkdir blog
$ cd blog
```

7.生成博客
``` bash
$ hexo init
```

8.启动博客
``` bash
$ hexo s
```

9.浏览器打开本地预览
> http://localhost:4000

10.清理
``` bash
$ hexo clean
```

11.生成博客
``` bash
$ hexo g
$ hexo s
```

12.浏览器打开
> http://localhost:4000 

13.安装部署工具
``` bash
$ cnpm install --save hexo-deployer-git
```

14.修改配置用于部署
> 在__config.yml最后添加：
    deploy:
    type: git
    repo: https://github.com/nld2019/nld2019.github.io.git
    branch: master

15.部署博客
``` bash
$ hexo d
```

16.根据仓库名称访问博客
> https://nld2019.github.io/
