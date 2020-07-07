---
title: 【PHP面试题】不断在文件 hello.txt 头部写入一行 "Hello World" 字符串，要求代码完整。
top: false
cover: false
toc: true
mathjax: true
date: 2020-07-08 00:28:06
password:
summary:
tags:
    - PHP面试
    - PHP基础
    - 文件
categories: PHP
---
## 一、考点
### 1、文件读取/写入操作
#### 1） 文件打开
> fopen()函数：用来打开一个文件，打开时需要指定打开模式

**打开模式：**
```
r :  只读方式打开，并且将文件指针指向文件的开头
r+ ：读写方式打开，并且将文件指针指向文件的开头
w ：写入方式打开，将文件指针指向文件的开头，且将文件的大小清空为0
w+ : 读写模式【如果文件不存在，会自动创建一个】
a : 追加的写入方式，会将文件的指针指向文件的末尾【如果文件不存在，也是会创建一个】
a+ : 读写方式，即读写的追加，将文件指针指向文件末尾【如果文件不存在，也是会创建】
x : 在创建的时候，以写入的方式进行打开，并且会将文件的指针指向文件的开头【如果文件已经存在，会报一个 warning的错误，并且 fopen 返回一个 false；如果文件不存在，才会去创建】
x+ : 创建并以读写的方式打开
b : 打开一个二进制文件
t : 可以透明的将 \t 转化成 \r、\n
```
#### 2） 写入函数
```PHP
fwrite();
fputs();
```

#### 3） 读取函数
```PHP
fread();
fgets(); // 获取一行
fgetc(); // 获取一个字符
```
#### 4） 关闭函数
```PHP
fclose();
```
#### 5） 不需要 fopen() 打开的函数
```PHP
file_get_contents(); 
file_put_contents();
```
#### 6） 其他读取函数
```PHP
file(); // 将整个一个文件读取到一个数组中
readfile(); // 将文件读取出来，并输出到缓冲区
```
#### 7） 访问远程文件
```
在 php.ini中 开启 allow_url_fopen，
	HTTP协议连接只能使用只读，
	FTP协议可以使用只读或者只写。
	
注：只有开启该项，才可以通过 file_get_contents() 或者 file_put_contents() 进行连接或者读取!!!
```
### 2、延伸：目录操作函数、其他文件操作
#### 1）目录操作函数
```PHP
// 名称相关
basename();
dirname();
pathinfo();

// 目录读取
opendir();
readdir();
closedir();
rewinddir();

// 目录删除
// 注意事项：目录中一定要为空【在删除的时候，先去遍历一下，把里面的内容清除掉，先删除文件，再去删除目录】
rmdir();

// 目录创建
mkdir();
```
#### 2） 其他函数
```PHP
// 文件大小
filesize();

// 目录大小
disk_free_space(); // 磁盘剩余空间
disk_total_space(); // 磁盘总空间

// 文件拷贝
copy();

// 删除文件
unlink();

// 文件类型
filetype(); // 获取到的文件类型 file/dir类型【常见的只有这两种】

// 重命名文件或者目录
rename(); // 不光可以重名名，还可以移动目录的位置【相当与 Linux命令mv】

// 文件截取
ftruncate(); // 截取到指定大小

// 文件属性
file_exists();   // 判断文件是否存在
is_readable();   // 是否可读
is_writable();   // 是否可写
is_executable(); // 是否可执行
filectime();     // inode修改时间
fileatime();     // 访问时间
filemtime();     // 整个修改时间

// 文件锁
flock();

// 文件指针
ftell();  // 返回文件指针读/写的位置
fseek();  // 在文件指针中定位
rewind(); // 倒回文件指针的位置
```

## 二、解题方法
> 1、牢记文件操作函数及几种打开模式
2、理解目录的操作步骤
3、尝试练习完成目录的复制和删除函数的编写

**注：在做目录删除的时候，一定要将 . 和 .. 过滤掉，否则会递归的向上删除掉所有的目录！！！**

## 三、真题
### 1、不断在文件 hello.txt 头部写入一行 “Hello World” 字符串，要求代码完整。

#### 1）创建一个 hello.txt 文件，并写入如下内容：
```
This is My Test Content!!!
```
#### 2） 创建一个 test.php 文件，并写入如下内容：

```PHP
// 打开文件
$file = './hello.txt';
$handle = fopen($file, 'r'); // 用 r+ 会把后面的内容覆盖掉

// 将文件的内容读取出来，在开头加入 Hello World
$content = fread($handle, filesize($file));
$content = 'Hello World' . $content;
fclose($handle);

// 将拼接好的字符串写回到文件当中
$handle = fopen($file, 'w');
fwrite($handle, $content);
fclose($handle);
```
#### 3）运行结果：
```
Hello WorldThis is My Test Content!!!
```

### 2、通过PHP函数的方式对目录进行遍历，写出程序。
#### 1） 创建目录 test,及在其目录下创建1、2、3文件；
#### 2） 在 test目录的同级新建一个文件夹 example.php用来遍历目录，如下：
```PHP

// 打开目录
// 读取目录当中的文件
// 如果文件类型是目录，继续打开目录
// 读取子目录的文件
// 如果文件类型是文件，输出目录
// 关闭目录

function loopDir($dir)
{
    $handle = opendir($dir);
    
    // 当目录中的文件都读取完时，才跳出循环【为了避免为0时，跳出循环】
    while (false !== ($file = readdir($handle))) {
        if ($file != '.' && $file != '..') {
            echo $file.'<br>';
            if (filetype($dir.'/'.$file) == 'dir') {
                loopDir($dir.'/'.$file);
            }
        }
    }
}

// 遍历的目录名称
$dir = './test';
loopDir($dir);
```