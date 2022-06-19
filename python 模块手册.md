# python 模块手册



## 1. os module

os模块提供丰富的系统接口来处理文件和目录。os模块可以忽略平台差异，对程序的可移植性有益。 



### 1.1  os.access()

检验是否有指定权限，有则返回True，否则返回False。

```
os.access(path, mode)
path: 指定的文件或者目录
mode: 	F_OK(是否存在)
		R_OK(是否可读)
		W_OK(是否可写)
		X_OK(是否可执行)
```

示例如下：

```python
>>> os.access('/python/autoDeploy/demo.py', os.F_OK)
True
>>> os.access('/python/autoDeploy/demo.py', os.R_OK)
True
>>> os.access('/python/autoDeploy/demo.py', os.W_OK)
True
>>> os.access('/python/autoDeploy/demo.py', os.X_OK)
False
```





### 1.2 os.getcwd()

获取当前工作目录。

```python
>>> os.getcwd()
'/home/gary'
```



### 1.2 os.path.abspath()

获取当前目录下文件或者文件的绝对路径。不会校验文件是否存在。

示例如下：

```python
>>> os.getcwd()
'/python/autoDeploy'
>>> os.listdir('.')
['demo.py']
>>> os.path.abspath('demo.py')
'/python/autoDeploy/demo.py'
>>> os.path.abspath('non-exist.py')			#注意，即使无文件该方法仍会返回当前目录下的文件。
'/python/autoDeploy/non-exist.py'
>>> os.path.abspath('/tmp/non-exist.py')	#对于填入的绝对目录文件，仍会返回。
'/tmp/non-exist.py'
```



### 1.3 os.path.basename()

返回文件名。也或者说，返回输入文件中分隔符(例如，/)最后一个字符。不会校验文件是否存在。

```python
>>> os.path.basename('/python/autoDeploy/demo.py')
'demo.py'
>>> os.path.basename('/tmp/non-exist.py')
'non-exist.py'
```



### 1.4 os.path.dirname()

返回文件目录名。或者，返回输入文件最后一个分隔符之前的字符。不会校验文件是否存在。

```python
>>> os.path.dirname('/python/autoDeploy/dem.py')
'/python/autoDeploy'
```



### 1.5 os.path.split()

分离文件名和目录。方法`os.path.basename()`和`os.path.dirname()`可以分别看作是本方法输出结果的两个部分。 

```python
>>> os.path.split('/python/autoDeploy/demo.py')
('/python/autoDeploy', 'demo.py')
```



### 1.6 os.path.join()

合并文件名和目录。方法`os.path.split()` 意义相反。

```python
>>> os.path.join('/python/autoDeploy', 'demo.py')
'/python/autoDeploy/demo.py'
```



### 1.7 os.path.splitext()

分离文件名与扩展。

```python
>>> os.path.splitext('/python/autoDeploy/demo.py')
('/python/autoDeploy/demo', '.py')
```



### 1.8 os.chdir()

改变当前工作目录，相当于shell 中的cd。 

```
os.chdir(path)
```

示例如下：

```python
>>> os.getcwd()
'/home/gary'
>>> os.chdir('/python/autoDeploy')
>>> os.getcwd()
'/python/autoDeploy'
```



### 1.9 os.listdir()

列举指定目录下的文件。需要确保目录存在并有足够权限，如果无权限查看则会返回错误**PermissionError**。

```python
>>> os.listdir('/python/autoDeploy')
['demo.py']
```



### 1.10 os.mkdir()

创建目录。

```
os.mkdir(path, mode)
```

有些系统默认权限值0o777。

```python
>>> os.mkdir('/tmp/34', mode=0o777)
>>> os.system('ls -ld /tmp/34')
drwxrwxr-x 2 gary gary 4096 5月  15 15:45 /tmp/34
0
```



### 1.11 os.rmdir()

删除空目录。如果目录不为空，会提示错误`OSError`。删除非空目录，使用`shutil.rmtree()`。



### 1.12 os.rename()

重命名文件或者目录。 



### 1.13 os.path.getsize()

获取文件或者目录大小。



### 1.14 os.path.isfile()

检查输入是否是文件。



### 1.15 os.path.isdir()

检查输入是否是目录。



### 1.16 os.path.exists()

检查输入是否存在。



### 1.17 os.chmod()

改变文件或者目录权限。需要与`stat`模块一起指定定义的权限。

```
os.chmod(path, mode)
权限如下：
	stat.S_ISUID	执行此文件其进程有效用户为文件所有者0o4000
	stat.S_ISGID	执行此文件其进程有效组为文件所在组0o2000
	stat.S_ENFMT	
	stat.S_ISVTX	目录里文件目录只有拥有者才可删除更改0o1000
	stat.S_IREAD	windows下设为只读
	stat.S_IWRITE	windows下取消只读
	stat.S_IEXEC
	stat.S_IRWXU	拥有者有全部权限(权限掩码)0o700
	stat.S_IRUSR	拥有者具有读权限0o400
	stat.S_IWUSR	拥有者具有写权限0o200
	stat.S_IXUSR	拥有者具有执行权限0o100
	stat.S_IRWXG	组用户有全部权限(权限掩码)0o070
	stat.S_IRGRP	组用户有读权限0o040
	stat.S_IWGRP	组用户有写权限0o020
	stat.S_IXGRP	组用户有执行权限0o010
	stat.S_IRWXO	其他用户有全部权限(权限掩码)0o007
	stat.S_IROTH	其他用户有读权限0o004
	stat.S_IWOTH	其他用户有写权限0o002
	stat.S_IXOTH	其他用户有执行权0o001
```

示例如下：

多个权限可以使用**|** 隔开。

```python
>>> os.system('ls -l /tmp/2.txt')
--------w- 1 gary gary 0 5月  15 15:13 /tmp/2.txt
0
>>> os.chmod('/tmp/2.txt',stat.S_IXOTH|stat.S_IWOTH|stat.S_IRWXU)
>>> os.system('ls -l /tmp/2.txt')
-rwx----wx 1 gary gary 0 5月  15 15:13 /tmp/2.txt
0
```



### 1.18 os.system()

执行外部系统命令。 



## 2. shutil module

**shutil**是python中的高级文件处理模块，与**os**模块形成互补关系。**os**主要提供了文件或文件夹的新建、删除、查看等方法，还提供了对文件以及目录的路径操作。**shutil**模块提供了移动、复制、 压缩、解压等操作，恰好与**os**互补，共同一起使用，基本能完成所有文件的操作。是一个非常重要的模块。



### 2.1 shutil.copy()/copy2()

`shutil.copy()`和`shutil.copy2()`均可以复制文件到目标端的文件或者目录。不同的是，**copy**方法仅复制文件不修改元数据；**copy2**方法复制文件后会修改元数据。

```
shutil.copy(fsrc, path)
```

示例如下:

```python
In [8]: shutil.copy('/tmp/srcDir/source.txt', '/tmp/destDir/target.txt')
Out[8]: '/tmp/destDir/target.txt'
```



### 2.2 shutil.copyfileobj()

将源文件内容拷贝到目标文件。如果目标文件有内容，内容将会被覆盖。如果目标文件不存在，则会创建新的目标文件。

```
shutil.copyfileobj(fsrc, fdst [, length=16*1024])
	
	fsrc: 源文件
	fdst: 目标文件
	length: 为缓冲区大小，即fsrc每次读取的长度
```

示例如下：

```python
import shutil 
f1 = open('/tmp/srcDir/source.txt', 'r')
f2 = open('/tmp/destDir/target.txt', 'w+')
shutil.copyfileobj(f1, f2)
f1.close()
f2.close()		# 如果不关闭，则内容不显示。
```



### 2.3 shutil.copyfile()

将一个文件的内容拷贝到另一个文件，目标文件无需存在。

```
shutil.copyfile(src, dst, follow_symlinks)
	
	src: 原文件目录
	dst: 复制至dst文件，若dst文件不存在，将会生成一个dst文件；若存在将会被覆盖
	follow_symlinks: 设置为True时，若src为软连接，则当成文件复制；如果设置为False，复制软连接。默认为True。
```

示例如下：

```python
#file_1不存在，会产生一个
shutil.copyfile('file_0.csv','file_1.csv')
'file_1.csv'
#file_2存在，直接复制
shutil.copyfile('file_0.csv','file_2.csv')
'file_2.csv'
```



### 2.4 shutil.copytree()

复制整个目录文件，不需要的文件类型可以设置不复制。

```
shutil.copytree(oripath, dstpath, ignore=shutil.ignore_patterns("*.doc", "*.csv"))

	ignore: 使用shutil.ignore_patterns方法可以忽略相应类型的文件。
```



### 2.5 shutil.copymode()

拷贝权限，前提是目标文件存在，否则报错。将src文件权限复制至dst文件。文件内容，所有者和组不受影响。



### 2.6 shutil.move()

递归移动文件或者文件夹。

```
shutil.remove(src, dst, copy_function=copy2)
```



### 2.7 shutil.chown()

更改文件或者目录属主属组。 

```
shutil.chown(path, user=None, group=None)

shutil.chown('/tmp/2.txt', user="gary", group="gary")
```



### 2.8 shutil.rmtree()

递归删除目录。

```
shutil.rmtree(path, ignore_errors=False, onerror=None)

	ignore_errors: 默认为False，不忽略错误；当设置为True时，会忽略错误并执行方法。
```



### 2.9 shutil.make_archive()

压缩目录并返回压缩包名称。 支持的压缩格式为： zip, tar, bztar, gztar 和xztar。

```
shutil.make_archive(base_name, format[, root_dir[, base_dir[, verbose[, dry_run[, owner[, group[logger]]]]]]])

	base_name: 被创建的压缩文件名，不必包含压缩格式后缀。如果仅为文件名，则在本工作目录生成文件，否则将压缩文件生成到指定目录。
	format: zip, tar, bztar, gztar 和xztar。
	root_dir/base_dir: 压缩开始的文件目录。
```



## 3. sys module

