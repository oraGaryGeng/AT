# python basic 



## 1. anaconda 

本段记录如何在远端Linux服务器安装anaconda，并添加一些基础配置已实现远程编程。



### 1.1 安装anaconda

从清华源下载最新的anaconda。清华大学镜像地址：https://mirror.tuna.tsinghua.edu.cn/help/anaconda/

![image-20220610164042851](C:\Users\xfgeng2\AppData\Roaming\Typora\typora-user-images\image-20220610164042851.png)

将安装包上传到目标主机并安装。安装过程中会提示需要用户确认。



使用`conda`命令验证安装是否成功。

```sh
(base) gary@gary-VirtualBox:/python$ conda --version 
conda 4.13.0
```

需要检测下`conda`的路径是否添加到用户环境参数中。 



### 1.2 配置conda软件源

安装完之后，会有一个**~/.condarc** 参数文件。如果没有，可以使用`conda config --set show_channel_urls yes`创建文件。 在文件中添加清华源。

```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
```

运行 `conda clean -i` 清除索引缓存，保证用的是镜像站提供的索引。



### 1.3 虚拟环境

为避免不同项目相互影响，python程序一般都运行在不同的虚拟环境中。使用`conda`创建并激活虚拟环境。

```sh
conda create -n testEnv python=3.6   # specific python version 
```



```
# 激活
conda activate testEnv 

#退出
conda deactivate
```



### 1.4 虚拟环境anaconda

在虚拟环境中安装jupyter notebook. 

```
conda install jupyter
```

生成jupyter配置文件 **[~/.jupyter/jupyter_notebook_config.py]**。 

```
jupyter notebook --generate-config
```

设置密码

使用ipython生成密码密文

```
In [2]: from notebook.auth import passwd

In [3]: passwd()
Enter password: 
Verify password: 
Out[3]: 'argon2:$argon2id$v=19$m=10240,t=10,p=8$n6dEM/SkxVIHhf71x0CQLg$y7zm6cs+12VrI3vbmVDqxQ'
```

最后在配置文件添加如下内容。

```
c.NotebookApp.allow_remote_access = True #允许远程访问

c.NotebookApp.ip = '*'		# 任意IP
c.NotebookApp.password = u'argon2:$argon2id$v=19$m=10240,t=10,p=8$f+BAJ7PSAv/sIqY7gkgvoQ$d2LMnpuvEVQ/70KxowHKg5OI6lxoTZ+YBdgg2+Bq6/Q' 
c.NotebookApp.open_browser = True #使用默认浏览器打开
c.NotebookApp.port = 10001			# 端口
```



虚拟机运行`jupyter-notebook` 即运行了notebook。本地在浏览器输入相应地址即可访问。 也可以在虚拟环境使用`nohup`后台运行。 



### 1.5 jupyter notebook 字体设置

一般有两种方式：

- css文件修改
- jupyter-themes包安装主体。

这里以修改notebook 的custom.css为例。 我的版本文件是：/home/gary/anaconda3/lib/python3.9/site-packages/notebook/static/custom/custom.css。

```
/* BASICS */

.CodeMirror {
  /* Set height, width, borders, and global font properties here */
  font-family: Consolas;
  height: 300px;
  color: black;
  direction: ltr;
}
```



### 1.6 conda 常见命令

```
activate // 切换到base环境

activate test // 切换到test环境

conda create -n test python=3.7 // 创建一个名为test的环境并指定python版本为3.7(的最新版本)

conda env list // 列出conda管理的所有环境

conda list // 列出当前环境的所有包

conda remove -n test --all // 删除test环境及下属所有包

conda env export > environment.yaml // 导出当前环境的包信息

conda env create -f environment.yaml // 用配置文件创建新的虚拟环境
```





## 2. spyder 

在远程主机`conda`创建的虚拟环境中安装`spyder-kernels`。

```
conda install spyder-kernels
```

在远程主机上运行spyder_kernels。

```
python3 -m spyder_kernels.console --matplotlib='inline' --ip=172.0.0.1 -f=/slave/conda/envs/testEnv/remoteTestEnv.json & 
```

将远程主机的**/slave/conda/envs/testEnv/remoteTestEnv.json** 拷贝的本机，并在本机点击**connect to existing kernel**。 



## reference

https://mirror.tuna.tsinghua.edu.cn/help/anaconda/

https://www.cnblogs.com/ikventure/p/15192158.html