# Linux tutorials

### 文件操作

```
rm test.txt    #删除文件
```

### 安装软件

```
sudo dpkg -i package.deb    # 从安装包安装
sudo apt-get install git   # 在线安装
```

### 更新系统

```
sudo apt-get update
sudo apt-get upgrade
```

### 为某个文件夹更改权限

`sudo chmod -R 777 directory`

### 解压/压缩文件

.tar只是打包，没有压缩；.tar.gz才进行压缩

```
tar -cvf pack.tar dir_to_compress  压缩为tar文件
tar -xvf pack.tar    解压tar文件

tar -zcvf pack.tar.gz dir_to_compress  压缩为tar.gz文件
tar -zxvf pack.tar    解压tar文件

tar命令有一下参数
-c 压缩文件内容
-x 解压文件中的内容
-z 使用gzip来解压或压缩.tar.gz格式的文件
-v 压缩过程中显示文件
-f 置顶文件名，f 后面立刻紧跟文件名，不能接受其他参数
```

zip文件

```
zip -r pack.zip dir_to_compress
unzip pack.zip
unzip -P password pack.zip   解压带有密码的压缩包
```