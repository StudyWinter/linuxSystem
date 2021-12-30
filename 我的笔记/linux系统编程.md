# linux系统编程

## 一、基础命令

### 1、shell

作用：命令解析器

ubuntu使用的是bash。在终端查看

```
 echo $SHELL
```

![image-20211124210059180](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124210059180-1640853453353.png)

date 显示系统当前时间

![image-20211124210232012](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124210232012-1640853462401.png)

cat /etc/shells  查看当前可使用的shell

![image-20211124210259475](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124210259475-1640853467408.png)

主键盘快捷键：

```
上					Ctrl-p	
下					Ctrl-n	
左					Ctrl-b	
右					Ctrl-f	
Del					Ctrl-d	delete		光标后面的
Home				Ctrl-a				first letter
End					Ctrl-e				end
Backspace	 		Backspace    	delete光标前面的单个字符
清除整行   		Ctrl-u    
删除光标到行末   	Ctrl-k
显示上滚    		Shift-PgUp
显示下滚    		Shift-PgDn
增大终端字体		Ctrl-Shift-+
减小终端字体		Ctrl- -
新打开一个终端 	Ctrl-Alt-T
清屏				Ctrl-l      直接用clear也行

```

### 2、类unix目录

pwd 查看当前所在目录

![image-20211124210416902](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124210416902-1640853497680.png)

Linux系统目录：

```
bin：存放二进制可执行文件
boot：存放开机启动程序
dev：存放设备文件： 字符设备、块设备
home：存放普通用户
etc：用户信息和系统配置文件 passwd、group
lib：库文件。如libc.so.6
root：管理员宿主目录（家目录）
usr：用户资源管理目录  unix software resource
```

其他基本命令

```
ls、cd、which、pwd、mkdir、rmdir、touch、rm、mv、cp、cat、ln、tree、wc、od、du、df
```

Linux系统文件类型： 7/8 种

```
普通文件：-
目录文件：d
字符设备文件：c
块设备文件：b
软连接：l
管道文件：p
套接字：s
未知文件。
```

文件权限说明

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124210858452-1640853506109.png" alt="image-20211124210858452" style="zoom:67%;" />

文件权限  硬链接计数  所有者  所属组 大小  时间  文件名/文件夹名

```
权限具体展开
	-rw-r—r—
	1234567890
	1代表文件类型
	234代表所有者读写执行权限
	567代表同组用户读写执行权限
	890代表其他人读写执行权限
```

隐藏终端中的路径

```
vi ~/.bashrc    打开使用的shell环境配置文件
```

![image-20211124211700305](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124211700305-1640853517075.png)

```
末尾添加 PS1=$  保存退出，重启终端即可
```

![image-20211124211743851](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124211743851-1640853522108.png)

### 3、文件属性、用户、用户组

```
whoami
chmom-----文字设定法（u g o a）、数字设定法（777）
chown-----adduser、deluser
chgrp-----addgroup、delgroup
切换用户su
```

### 4、查找与检索

find 

```
find命令：找文件
	-type 按文件类型搜索  d/p/s/c/b/l/ f:文件
	-name 按文件名搜索

find ./ -name "*file*.jpg"
-maxdepth 指定搜索深度。应作为第一个参数出现。
find ./ -maxdepth 1 -name "*file*.jpg"
-size 按文件大小搜索. 单位：k、M、G
find /home/itcast -size +20M -size -50M
-atime、mtime、ctime 天  amin、mmin、cmin 分钟。
-exec：将find搜索的结果集执行某一指定命令。
find /usr/ -name '*tmp*' -exec ls -ld {} \;
-ok: 以交互式的方式 将find搜索的结果集执行某一指定命令
-xargs：将find搜索的结果集执行某一指定命令。  当结果集数量过大时，可以分片映射。
find /usr/ -name '*tmp*' | xargs ls -ld
-print0：
find /usr/ -name '*tmp*' -print0 | xargs  -0 ls -ld
```

grep

```
grep命令：找文件内容
grep -r 'copy' ./ -n
-n参数：:显示行号
ps aux | grep 'cupsd'  -- 检索进程结果集。
```

### 5、其他命令

man、clear、echo、 alias

![image-20211124212548030](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211124212548030-1640853533014.png)

umask（新创建的文件不具备执行权限）

### 6、进程管理

```
who、ps(ps aux)、jobs、fg、bg、kill、env、top
```

### 7、压缩相关

tar

```
压缩
	1. tar -zcvf 要生成的压缩包名 压缩材料。
	tar zcvf  test.tar.gz  file1 dir2   使用 gzip方式压缩。
	tar jcvf  test.tar.gz  file1 dir2   使用 bzip2方式压缩。

解压：将压缩命令中的 c --> x
	tar zxvf  test.tar.gz   使用 gzip方式解压缩。
	tar jxvf  test.tar.gz   使用 bzip2方式解压缩。
```

rar

```
压缩
	rar a -r  压缩包名（带.rar后缀） 压缩材料。
	rar a -r testrar.rar stdio.h test2.mp3
解压
	unrar x 压缩包名（带.rar后缀）
```

zip

```
压缩
	zip -r 压缩包名（带.zip后缀） 压缩材料。
	zip -r testzip.zip dir stdio.h test2.mp3
解压
	unzip 压缩包名（带.zip后缀）
	unzip  testzip.zip
```

### 8、vim相关

![image-20211125193150602](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125193150602-1640853542343.png)



#### 8.1 跳转 

基本操作

```
i 进入编辑模式，光标前插入字符
a 进入编辑模式，光标后插入字符
o 进入编辑模式，光标所在行的下一行插入
I 进入编辑模式，光标所在行的行首插入
A 进入编辑模式，光标所在行的行末插入字符
O 进入编辑模式，光标所在行的上一行插入字符
s 删除光标所在字符并进入编辑模式
S 删除光标所在行并进入编辑模式

x 删除光标所在字符，工作模式不变
dw 删除光标所在单词，要求光标在首字母上，如果不在首字母，只会删除当前位置到单词末，工作模式不变
D  删除光标所在位置到行末，工作模式不变
0(数字) 光标移到行首，工作模式不变
$ 光标移到行尾，工作模式不变
d0 删除光标所在位置到行首，工作模式不变
d$ 删除光标所在位置到行末，工作模式不变

```

命令模式下的光标移动

```
h 左移
j 下移
k 上移
l 右移


命令模式下行跳转:line-G  缺点是没有回显
末行模式下行跳转:line-回车

跳转首行:gg （命令模式）
跳转末行:G  （命令模式）
```

自动缩进

```
在这之前要进行vimrc修改，不然自动缩进是8个空格
ubuntu的vimrc位置在/etc/vim/vimrc
在文件末尾添加三行：
set tabstop=4    		//设置制表符宽度为4
set softtabstop=4  	// 设置软制表符宽度为4
set shiftwidth=4    // 设置缩进空格数为4
```

大括号跳转

命令模式下，光标处于左大括号时，使用%跳转到对应右大括号，再按%跳回去。其他括号也可以这样

 **gg=G 自动化程序**

#### 8.2 删除

```
替换单个字符:r 命令模式下替换光标选中字符

 
一段删除，即删除指定区域
光标选中要删除的首字符，按v进入可视模式，再使用hjkl移动到要删除的末尾，按d删除

删除整行：dd，删除光标所在行
				n+dd ，删除从光标开始的n行
```

#### 8.3 复制粘贴

```
yy 复制光标所在行
	p  向后粘贴剪切板内容，如果复制整行，这里是粘贴在光标所在位置的下一行
	P  向前粘贴剪切板内容，如果是整行，这里是粘贴在光标所在位置的上一行

这里提一下，上一节里的dd，不是删除，而是剪切，小时的内容去了剪切板，而不是删掉了
p和P粘贴会出现换行，主要原因是复制整行时，会把行末的换行符也复制下来。

n-yy  复制光标所在位置的n行，包括光标所在行
```

#### 8.4 查找和替换

查找

```
/+findname   命令模式下查找
按回车键启动查找后，按n，会自动找下一个，N跳到上一个

查找光标所在单词
光标在目标单词上时，*或者#查找下一个，这里不要求光标必须在首字母上
```

替换：末行模式下进行

```
单行替换  
光标置于待替换行，   :s /待替换词/替换词

全文替换
:%s /待替换词/替换词           这个默认替换每行的首个，一行有多个目标词时，后面的不会变
:%s /待替换词/替换词/g       真正意义上的全局替换

区域替换
:24,35s /待替换词/替换词/g     替换24-35行之间的目标词
```

末行模式下历史命令

```
Ctrl-p 上一条命令
Ctrl-n 下一条命令
```

#### 8.5 其他操作

撤销

```
命令模式下
u  撤销操作
Ctrl-r  反撤销
```

分屏

```
分屏，末行模式下
:sp   水平分屏 
:vsp  竖直分屏
分屏命令+filename，分屏并打开这个文件
分屏后屏幕切换，Ctrl-w-w
使用:q退出光标所在窗口
使用:qall退出所有窗口
```

跳转manpage

```
从vim中跳转manpage，命令模式下
将光标放在待查看单词上，按K，默认看第一卷
n+K，查看第n卷（函数查看3K）
```

其他

```
查看宏定义：命令模式
	光标放在待查看词上，[+d即可查看


vim下使用shell命令：末行模式
	:! + 命令
操作后，会切换至终端显示结果，按Enter后回到vim界面。
```

#### 8.6 vim配置

两个vim配置文件

```
1.	/etc/vim/vimrc
2.	~/.vimrc
```

其中，第二个配置文件会优先加载，属于用户配置

```shell
set nu
autocmd BufNewFile *.cpp,*.[ch],*.sh,*.java exec ":call SetTitle()"
"""定义函数SetTitle，自动插入文件头
func SetTitle()
"如果文件类型为.sh文件
if &filetype == 'sh'
call setline(1,"\#########################################################################")
call append(line("."), "\# File Name: ".expand("%"))
call append(line(".")+1, "\# Author: Winter")
call append(line(".")+2, "\#Created Time:".strftime("%c"))
call append(line(".")+3, "\#########################################################################")
call append(line(".")+4,"\#!/bin/bash")
call append(line(".")+5,"")
else
call setline(1, "/*************************************************************************")
call append(line("."), " > File Name: ".expand("%"))
call append(line(".")+1, " > Author: Winter")
call append(line(".")+2, " > Created Time: ".strftime("%c"))
call append(line(".")+3, " ************************************************************************/")
call append(line(".")+4, "")
endif
if &filetype == 'cpp'
call append(line(".")+5, "#include<iostream>")
call append(line(".")+6, "using namespace std;")
call append(line(".")+7, "")
call append(line(".")+8, "int main(int argc, char* argv[])")
call append(line(".")+9, "{")
call append(line(".")+10, "")
call append(line(".")+11, "     return 0;")
call append(line(".")+12, "}")
call append(line(".")+13, "")
endif
if &filetype == 'c'
call append(line(".")+5, "#include<stdio.h>")
call append(line(".")+6, "#include<stdlib.h>")
call append(line(".")+7, "#include<string.h>")
call append(line(".")+8, "#include<unistd.h>")
call append(line(".")+9, "#include<pthread.h>")
call append(line(".")+10, "")
call append(line(".")+11, "int main(argc, char* argv[])")
call append(line(".")+12, "{")
call append(line(".")+13, "")
call append(line(".")+14, "     return 0;")
call append(line(".")+15, "}")
call append(line(".")+16, "")
endif
"新建文件后，自动定位到文件末尾
autocmd BufNewFile * normal G
endfunc
```

### 9、gcc相关

#### 9.1 gcc编译4步骤

![image-20211125195355380](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125195355380-1640853582277.png)

```
-o 是输出文件名称
gcc -E hello.c -o hello.i
查看vim hello.i

gcc -S hello.c -o hello.s
查看vim hello.s
```

#### 9.2 gcc编译常用参数

当头文件和源码不在一个目录下时，需要指定头文件

```makefile
gcc -I ./hellodir hello.c -o hello
其中-I参数指定头文件所在位置，位置可以在编译文件前，也可以在后面

-I			指定头文件所在目录位置
-c 			只做预处理，编译，汇编。得到二进制文件【看不懂】
-g 			编译时添加调试文件，用于gdb调试
-Wall   显示所有警告信息
-l			指定动态库库名
-L			指定动态库路径
```

```
-D  		向程序中“动态”注册宏定义
```

![image-20211125195857372](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125195857372-1640853590838.png)

![image-20211125195905852](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125195905852-1640853595165.png)

### 10、动态库和静态库

#### 10.1 理论

静态库是指在我们的应用中，有一些公共代码是需要反复使用，就把这些代码编译为“库”文件；在链接步骤中，连

接器将从库文件取得所需的代码，复制到生成的可执行文件中的这种库。

程序编译一般需经预处理、编译、汇编和链接几个步骤。静态库特点是可执行文件中包含了库代码的一份完整拷

贝；缺点就是被多次使用就会有多份冗余拷贝。

静态库和动态库是两种共享程序代码的方式，它们的区别是：静态库在程序的链接阶段被复制到了程序中，和程序

运行的时候没有关系；动态库在链接阶段没有被复制到程序中，而是程序在运行时由系统动态加载到内存中供程序

调用。使用动态库的优点是系统只需载入一次动态库，不同的程序可以得到内存中相同的动态库的副本，因此节省

了很多内存。

**静态库在文件中静态展开，所以有多少文件就展开多少次，非常吃内存，100M展开10次，就是1G**，但是这样的

好处就是静态加载的速度快。

**使用动态库会将动态库加载到内存，10个文件也只需要加载一次，然后这些文件用到库的时候临时**去加载，速度

慢一些，但是很省内存

动态库和静态库各有优劣，根据实际情况合理选用即可。

![image-20211125200650038](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125200650038-1640853604044.png)

#### 10.2 静态库制作

**静态库名字以lib开头，以.a结尾**

例如：libmylib.a

静态库生成指令

ar rcs 库名字 原材料

ar rcs libmylib.a file1.o

##### 步骤一

写好源代码

三个文件：add.c、sub.c、div1.c（加法、减法、除法）

add.c

```c
int add (int a, int b) {
        return a + b;
}
```

sub.c

```c
int sub (int a, int b) {
        return a - b;
}
```

div1.c

```c
int div1 (int a, int b) {
        return a / b;
}
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906201645146-1640853613918.png)

test.c文件

```c
/*************************************************************************
 > File Name: test.c
 > Author: Winter
 > Created Time: 2021年09月06日 星期一 20时21分35秒
 ************************************************************************/
 
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
 
int main(int argc, char* argv[])
{
	int a = 10, b = 5;
	printf("%d + %d = %d \n", a, b, add(a, b)); 
	printf("%d - %d = %d \n", a, b, sub(a, b)); 
	printf("%d / %d = %d \n", a, b, div1(a, b)); 
	return 0;
}
```

##### 步骤二

编译源代码生成.o文件

```makefile
gcc -c add.c -o add.o
gcc -c sub.c -o sub.o
gcc -c div1.c -o div1.o
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906201815675-1640853626738.png)

##### 步骤三

制作静态库

```
ar rcs libMyMath.a add.o sub.o div1.o
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906202020988-1640853638113.png)

静态库的使用：

```
gcc test.c libMyMath.a -o test
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906203048627-1640853643241.png)

上图中，test.c直接使用了mymath库中的add，sub，div1函数，所以在编译时要导入静态库

编译时出现了函数未定义的警告，可以忽略，让系统生成默认的定义。

出现的警告，用编译器隐式声明来解决的

编译器只能隐式声明返回值为int的函数形式：int add(int ,int )；

如果函数不是返回的int，则隐式声明失效，会报错。

在test.c中加入函数声明：
![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906203641324-1640853649374.png)

再次进行编译，就不会报错了：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906203243175-1640853655831.png)

上面这个方法，需要库的使用者知道库里的函数，完事儿一个一个加到代码里，不太行。

一般静态库也会有相应的头文件。

下面使用头文件的方法来加载静态库

```h
#ifndef _MYMATH_H_
#define _MYMATH_H_
 
int add (int,int);
int sub(int,int);
int div1(int,int);
 
 
#endif
```

ifndef、define为头文件守卫，防止在代码中多次include头文件，多次展开静态库，带来的额外开销，这样也不

会报错了，而且更加科学。

再在test.c中加入头文件。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906204457753-1640853663943.png)

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906204718742-1640853670400.png)

下面将静态库和头文件分别放至其他目录下

新建resource、inc、lib目录。

将*.c（test.c除外。图片有误）、*.o放到resource目录下；将*.h（头文件）放到inc；将*.a（静态库）放到lib目录下

运行过程如下

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906205241915-1640853675734.png)

最终目录如下

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906205548420-1640853682824.png)

 接下来重新编译 

```
gcc test.c ./lib/libMyMath.a -o test -I ./inc/
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021090620573978-1640853687215.png)

 -I： 指定头文件所在目录位置。

#### 10.3 动态库制作

写在源代码里的函数，相对main函数偏移是一定的，链接时，回填main函数地址之后，其他源代码里的函数也就

得到了地址。

动态库里的函数会用一个@plt来标识，当动态库加载到内存时，再用加载进去的地址将@plt替换掉。

查看反汇编

```
objdump -dS test > out
```

```
vi out
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/202109062106341-1640853693716.png)

printf是动态库函数，依赖@plt。回填的是函数指针。

制作动态库的步骤

##### 步骤一：生成位置无关的.o文件

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906215237388-1640853700498.png)

***.c文件的内容和静态库里面的内容一样。**

#####  步骤二：制作动态库

gcc -shared -o lib库名.so add.o sub.o div.o

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906215740951-1640853707646.png)

库名：libMyMath.so。【lib是固定的，so是固定的，中间的MyMath是自己定义的】

将生成的动态库libMyMath.so放到新创建的lib路径下。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906220017690-1640853712230.png)

 将MyMath.h头文件放到inc目录下。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906220314319-1640853716919.png)

##### 步骤三：编译程序

文件分布如下：动态库在lib目录下，头文件在inc目录下

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906220423984-1640853721215.png)

 下面编译文件

```
gcc test.c -o test -l MyMath -L ./lib/ -I ./inc/
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906220618968-1640853727167.png)

MyMath是动态库的名称（去掉lib，去掉.so）；

-l: 指定库名

-L 指定动态库路径

-I 指定头文件

##### 步骤四：执行文件，出错

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906220819546-1640853732367.png)

##### 步骤五

出错原因分析：

连接器： 工作于链接阶段，a.out生成前使用，工作时需要 -l 和 -L

动态链接器： 工作于程序运行阶段，工作时需要提供动态库所在目录位置【位置固定】

**法一：通过环境变量**

指定动态库路径并使其生效，然后再执行文件

通过环境变量指定动态库所在位置：export LD_LIBRARY_PATH=动态库路径

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906221058230-1640853737615.png)

当关闭终端，再次执行a.out时，又报错。

这是因为，环境变量是进程的概念，关闭终端之后再打开，是两个进程，环境变量发生了变化。

**法二：永久生效**

写入 终端配置文件。  修改.bashrc  【建议使用绝对路径】

1) vi ~/.bashrc

2) 写入 export LD_LIBRARY_PATH=动态库路径  保存

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021090622164063-1640853757623.png)

 3）. .bashrc/  source .bashrc / 重启 终端  ---> 让修改后的.bashrc生效

4）./a.out 成功！！！

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906221740633-1640853797387.png)

 查看动态库的位置

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906221845924-1640853803891.png)

**法三：拷贝自定义动态库 到 /lib (标准C库所在目录位置)**

放到 /lib或者/lib/x86_64-linux-gnu下

再把~/.bashrc的配置注释掉

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021090622214884-1640853808432.png)

查看动态库位置

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906222336854-1640853815582.png)

**法四： 配置文件法**

1）sudo vi /etc/ld.so.conf

2）写入 动态库绝对路径  保存

3）sudo ldconfig -v  使配置文件生效。

4）./a.out 成功！！！--- 使用 ldd  a.out 查看

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210906223935411-1640853822128.png)

```
具体制作静态库和动态库可以参考：https://blog.csdn.net/Zhouzi_heng/article/details/120105620
```

### 11、GDB调试

**使用gdb之前，要求对文件进行编译时增加-g参数，加了这个参数过后生成的编译文件会大一些，这是因为增加了**

**gdb调试内容**

**不加-g**

![image-20211125205513446](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125205513446-1640853847272.png)

加-g

![image-20211125205531045](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125205531045-1640853852744.png)

gdb+可执行文件 进入

![image-20211125205613924](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125205613924-1640853857382.png)

```
gdb调试工具：   大前提：程序是你自己写的。  ---逻辑错误
```

基础指令

```
	-g：使用该参数编译可以执行文件，得到调试表。
	gdb ./a.out
	list： list 1  列出源码。根据源码指定 行号设置断点。
	b：	b 20	在20行位置设置断点。
	run/r:	运行程序
	n/next: 下一条指令（会越过函数）
	s/step: 下一条指令（会进入函数）
	p/print：p i  查看变量的值。
	continue：继续执行断点后续指令。
	finish：结束当前函数调用。 
	quit：退出gdb当前调试。
```

其他指令

```
run：使用run查找段错误出现位置。
set args： 设置main函数命令行参数 （在 start、run 之前）
run 字串1 字串2 ...: 设置main函数命令行参数
info b: 查看断点信息表
b 20 if i = 5：	设置条件断点。
ptype：查看变量类型。
bt：列出当前程序正存活着的栈帧。
frame： 根据栈帧编号，切换栈帧。
display：设置跟踪变量
undisplay：取消设置跟踪变量。 使用跟踪变量的编号。
break/b n   在第n行设置断点，断点那一行不会执行
```

#### 11.1 gdb调试其他指令

段错误

![image-20211125210023459](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125210023459-1640853870844.png)

解决

![image-20211125210100373](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125210100373-1640853875280.png)

其他指令

```
run：gdb进来直接使用run查找段错误出现位置。
set args： 设置main函数命令行参数 （在 start、run 之前）set args aa bb cc dd
run 字串1 字串2 ...: 设置main函数命令行参数
info b: 查看断点信息表
b 20 if i = 5：	设置条件断点。
ptype：查看变量类型。
display：设置跟踪变量
undisplay：取消设置跟踪变量。 使用跟踪变量的编号。
start：进主函数第一行
finish:结束当前函数调用

bt：列出当前程序正存活着的栈帧。
```

![image-20211125210211842](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125210211842-1640853889113.png)

```
frame： 根据栈帧编号，切换栈帧。
```

![image-20211125210229230](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125210229230-1640853894777.png)

#### 11.2 gdb常见错误

没有符号被读取—编译时没加-g参数

file后面加使用-g编译的文件，可以不用退出，gdb直接读取后进行调试。

### 12、makefile基础规则

什么是makefile？或许很多Winodws的程序员都不知道这个东西，因为那些Windows的IDE都为你做了这个工

作，但我觉得要作一个好的和professional的程序员，makefile还是要懂。这就好像现在有这么多的HTML的编辑

器，但如果你想成为一个专业人士，你还是要了解HTML的标识的含义。特别在Unix下的软件编译，你就不能不自

己写makefile了，**会不会写makefile，从一个侧面说明了一个人是否具备完成大型工程的能力**。因为makefile关

系到了整个工程的编译规则。一个工程中的源文件不计数，其按**类型、功能、模块**分别放在若干个目录中，

makefile定义了一系列的规则来指定，哪些文件需要先编译，哪些文件需要后编译，哪些文件需要重新编译，甚至

于进行更复杂的功能操作，因为makefile就像一个Shell脚本一样，其中也可以执行操作系统的命令。makefile带

来的好处就是——“自动化编译”，一旦写好，只需要一个make命令，整个工程完全自动编译，极大的提高了软件

开发的效率。make是一个命令工具，是一个解释makefile中指令的命令工具，一般来说，大多数的IDE都有这个

命令，比如：Delphi的make，Visual C++的nmake，Linux下GNU的make。可见，makefile都成为了一种在工程

方面的编译方法。

**命名：makefile  Makefile** --- make 命令

**1 个规则**

```
目标：依赖条件
（一个tab缩进）命令

1. 目标的时间必须晚于依赖条件的时间，否则，更新目标
2. 依赖条件如果不存在，找寻新的规则去产生依赖条件。

ALL：指定 makefile 的终极目标。
```

**2 个函数**

```
src = $(wildcard ./*.c): 匹配当前工作目录下的所有.c 文件。将文件名组成列表，赋值给变量 src。  
	src = add.c sub.c div1.c 
obj = $(patsubst %.c, %.o, $(src)): 将参数3中，包含参数1的部分，替换为参数2。 
	obj = add.o sub.o div1.o
clean:	(没有依赖)
	-rm -rf $(obj) a.out	“-”：作用是，删除不存在文件时，不报错。顺序执行结束。
```

**3 个自动变量**

```
$@: 在规则的命令中，表示规则中的目标。
$^: 在规则的命令中，表示所有依赖条件。
$<: 在规则的命令中，表示第一个依赖条件。如果将该变量应用在模式规则中，它可将依赖条件列表中的依赖依次取出，套用模式规则。


模式规则：
		%.o:%.c
		   gcc -c $< -o %@

静态模式规则：
		$(obj):%.o:%.c
		   gcc -c $< -o %@	

伪目标：
		.PHONY: clean ALL

	参数：
		-n：模拟执行make、make clean 命令。
		-f：指定文件执行 make 命令。				xxxx.mk
```

#### 12.1 实现

下面来一步一步升级makefile

第一个版本的Makefile

makefile的依赖是从上至下的，换句话说就是目标文件是第一句里的目标，如果不满足执行依赖，就会继续向下执

行。如果满足了生成目标的依赖，就不会再继续向下执行了。 make会自动寻找规则里需要的材料文件，执行规则

下面的行为生成规则中的目标。

编写一个输出hello world的c文件，再编写makefile文件，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911135525145-1640853915412.png)

hello是目标，hello.c是依赖，gcc hello.c -o hello是具体的规则

**调整**

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911135546742-1640853920815.png)

第1行：hello是目标，hello.o是依赖，gcc hello.o -o hello是具体的规则；

第4行：hello.o是目标，hello.c是依赖，gcc hello.c -o hello.o是具体的规则

这其实是把第一个分开写了。

**执行make指令**

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911140008590-1640853929085.png)

继续修改hello.c，如下

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
int add (int ,int);
int sub (int ,int);
int div1 (int ,int);
 
 
int main(int argc, char* argv[])
{
    int a = 20, b = 10;
    printf("%d + %d = %d \n",a, b, add(a, b));
    printf("%d - %d = %d \n",a, b, sub(a, b));
    printf("%d / %d = %d \n",a, b, div1(a, b));
    printf("%d * %d = %d \n",a, b, mul(a, b));
    return 0;
}
```

add、sub、div1分别是加法、减法、除法。

add.c

```c
int add(int a, int b)
{
    return a + b;
}
```

sub.c 和div1.c类似，不再赘述。

此时要进行编译，则需要多文件联合编译：

```
gcc hello.c add.c sub.c div1.c -o a.out
```

对这个新的代码，写出下面的makefile

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911140611275-1640854084992.png)

执行make

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911140628724-1640854089427.png)

将add.c修改为下图

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911140706641-1640854093477.png) 

 此时，再使用make，发现了问题

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911140829490-1640854097461.png)

可以看到，只修改add.c，但是编译的时候，其他.c文件也重新编译了，这不太灵活。明明只改了一个，全部都重

新编译了。

再修改makefile，先生成.o文件

于是将makefile改写如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021091114100694-1640854153411.png)

 执行make指令如下

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021091114102161-1640854159003.png)

 此时修改sub为下图：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141032572-1640854165099.png)

再次make

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141045413-1640854169702.png)

可以看到，只重新编译了修改过的sub.c和最终目标

> makefile检测原理：
>
> 修改文件后，文件的修改时间发生变化，会出现目标文件的时间早于作为依赖材料的时间，出现这种情况的文件会重新编译。
>
> 修改sub.c后，sub.o的时间就早于sub.c ，a.out的时间也早于sub.o的时间了，于是重新编译这俩文件了。

关于makefile指定目标问题，先修改makefile如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141134772-1640854176251.png)

 只是将hello放在了文件末尾

执行make，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141156632-1640854187196.png)

 这是因为，makefile默认第一个目标文件为终极目标，生成就跑路，这时候可以用ALL来指定终极目标。

指定目标的makefile

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141221300-1640854196079.png)

执行make

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141233461-1640854202653.png)

**第一个版本的makefile**

```makefile
ALL:hello
hello.o:hello.c
        gcc -c hello.c -o hello.o
 
add.o:add.c
        gcc -c add.c -o add.o
 
sub.o:sub.c
        gcc -c sub.c -o sub.o
 
div1.o:div1.c
        gcc -c div1.c -o div1.o
 
hello:hello.o add.o sub.o div1.o
        gcc hello.o add.o sub.o div1.o -o hello
```

继续升级makefile。

```
下来介绍两个函数：

src = $(wildcard *.c)

找到当前目录下所有后缀为.c的文件，赋值给src

wildcard ：函数名

*.c：函数参数

src：返回值

obj = $(patsubst %.c,%.o, $(src))
patsubst：函数名
把src变量里所有后缀为.c的文件替换成.o
```

用这两个函数修改makefile如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141825570-1640854215687.png)

执行，make指令，如下所示：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911141838431-1640854223406.png)

**第二个版本的makefile**

```makefile
src = $(wildcard *.c)                    #add.c sub.c div1.c hello.c
 
obj = $(patsubst %.c, %.o,  $(src))      #把参数3中包含 参数1 的替换成 参数2  add.o sub.o div1.o hello.o
 
 
ALL:hello
hello.o:hello.c
        gcc -c hello.c -o hello.o
 
add.o:add.c
        gcc -c add.c -o add.o
 
sub.o:sub.c
        gcc -c sub.c -o sub.o
 
div1.o:div1.c
        gcc -c div1.c -o div1.o
 
hello:$(obj)
        gcc $(obj) -o hello
```

每次要删除.o文件，很烦，于是改写makefile如下，加了clean部分。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142023926-1640854234021.png)

 rm前面的-，代表出错依然执行。比如，待删除文件集合是5个，已经手动删除了1个，就只剩下4个，然而删除命

令里面还是5个的集合，就会有删除不存在文件的问题，不加这-，就会报错，告诉你有一个文件找不到。加了-就

不会因为这个报错。【**新版不加也不报错**】

于是，重新make，再执行clean：

-n是先看一下执行的语句，不会删除

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142145357-1640854255789.png)

可以看到模拟执行后，会删除哪些文件。确定没有问题，执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142207759-1640854259493.png)

 **第三个版本的makefile**

```makefile
src = $(wildcard *.c)                    #add.c sub.c div1.c hello.c
 
obj = $(patsubst %.c, %.o,  $(src))      #把参数3中包含 参数1 的替换成 参数2  add.o sub.o div1.o hello.o
 

ALL:hello
hello.o:hello.c
        gcc -c hello.c -o hello.o
 
add.o:add.c
        gcc -c add.c -o add.o
 
sub.o:sub.c
        gcc -c sub.c -o sub.o
 
div1.o:div1.c
        gcc -c div1.c -o div1.o
 
hello:$(obj)
        gcc $(obj) -o hello
 
clean:
        -rm -rf $(obj) hello
```

继续升级makefile

> 3个自动变量
>
> $@ ：在规则命令中，表示规则的中的目标【命令的位置，目标和依赖条件不能出现】
>
> $< ：在规则命令中，表示规则中的第一个条件，如果将该变量用在模式规则中，它可以将依赖条件列表中的依赖依次取出，套用模式规则
>
> $^ ：在规则命令中，表示规则中的所有条件，组成一个列表，以空格隔开，如果这个列表中有重复项，则去重

用自动变量修改makefile，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142517868-1640854271422.png)

sub，add这些指令中使用$<和$^都能达到效果，但是为了模式规则，所以使用的$<

执行make，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142553413-1640854279027.png)

  **第四个版本的makefile**

```makefile
src = $(wildcard *.c)                    #add.c sub.c div1.c hello.c
 
obj = $(patsubst %.c, %.o,  $(src))      #把参数3中包含 参数1 的替换成 参数2  add.o sub.o div1.o hello.o
 
 
 
ALL:hello
hello.o:hello.c
        gcc -c $< -o $@
 
add.o:add.c
        gcc -c $< -o $@
 
sub.o:sub.c
        gcc -c $< -o $@
 
div1.o:div1.c
        gcc -c $< -o $@
 
hello:$(obj)
        gcc $^ -o $@
 
clean:
        -rm -rf $(obj) hello
```

**再来，上面这个makefile，可扩展性不行。比如，要添加一个乘法函数，就需要在makefile里面增加乘法函数的**

**部分。不灵活，所以，模式规则就来了**

```makefile
%.o:%.c
    gcc -c $< -o $@
```

修改makefile，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142800246-1640854290862.png)

执行make，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142811743-1640854296130.png)

这时，增加一个mul函数，并添加mul.c文件如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142825236-1640854300758.png)

mul.c如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142834392-1640854306349.png)

直接执行make：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911142848463-1640854311317.png)

增加函数的时候，不用改makefile，只需要增加.c文件，改一下源码，就行。很强势。

**第五个版本的makefile**

```makefile
src = $(wildcard *.c)                    #add.c sub.c div1.c hello.c
 
obj = $(patsubst %.c, %.o,  $(src))      #把参数3中包含 参数1 的替换成 参数2  add.o sub.o div1.o hello.o
 
 
 
ALL:hello
 
%.o:%.c
        gcc -c $< -o $@
 
hello:$(obj)
        gcc $^ -o $@
 
clean:
        -rm -rf $(obj) hello
```

继续优化makefile，**使用静态模式规则，就是指定模式规则给谁用，这里指定模式规则给obj用，以后文件多**

**了，文件集合会有很多个，就需要指定哪个文件集合用什么规则**

```makefile
$(obj):%.o:%.c
    gcc -c $< -o $@
```

修改后makefile如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143146988-1640854321142.png) 

运行如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143157944-1640854326104.png)

再来一个扩展

当前文件夹下有ALL文件或者clean文件时，会导致makefile瘫痪，如下所示，make clean没有工作

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143217262-1640854332119.png)

用伪目标来解决，添加一行   .PHONY: clean ALL

makefile如下所示：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143237829-1640854336418.png)

 再来执行make clean，就不会受到干扰了

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143248726-1640854341911.png)

还有一个扩展，编译时的参数，-g,-Wall这些，可以放在makefile里面，修改后makefile如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143306962-1640854345011.png)

执行makefile，如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143324849-1640854350879.png)

可以直接gdb调试。

**最终版本**

```makefile
src = $(wildcard *.c)
 
obj = $(patsubst %.c, %.o,  $(src))
 
myArgs = -Wall -g
 
ALL:hello
 
$(obj):%.o:%.c
        gcc -c $< -o $@ $(myArgs)
 
hello:$(obj)
        gcc $^ -o $@ $(myArgs)
 
clean:
        -rm -rf $(obj) hello
.PHONY:clean ALL
```

对比第一个版本和最终版，基本看不出来相同点

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210911143538894-1640854359807.png)

```
参考：https://blog.csdn.net/Zhouzi_heng/article/details/120235296
```

#### 12.2 练习

编写一个 makefile 可以将其所在目录下的所有独立 .c 文件编译生成同名可执行文件。

例如，有add.c、sub.c等等

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
 
int main(int argc, char* argv[])
{
        int a = 10, b = 20;
        printf("%d + %d = %d\n",a , b, a + b);
        return 0;
}
```

编写makefile

```makefile
src = $(wildcard *.c)                    #拿到所有*.c
target  = $(patsubst %.c, %, $(src))     #将src中所有%.c替换成%
 
ALL:$(target)
 
%:%.c
        gcc $< -o $@
 
clean:
        -rm -rf $(target)
 
.PHONY:clean ALL
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210915112542228-1640854369580.png)

清除 

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210915112615500-1640854375137.png)

```
参考：https://blog.csdn.net/Zhouzi_heng/article/details/120304969
```

## 二、文件IO

### 1、系统调用

操作系统实现并提供给外部应用程序的编程接口；

作用：完成应用程序从User区到kernel区的权级切换；

write：系统函数----系统调用的浅封装；

sys_write：系统调用

应用程序----》标准函数----》系统调用----》驱动----》驱动----》硬件

#### 1.1 什么是系统调用

系统调用函数属于操作系统的一部分，是为了提供给用户进行操作的接口（API函数），使得用户态运行的进程与硬件设备(如CPU、磁

盘、打印机、显示器)等进行交互。

- 例如常见的系统调用 等等`write read open` …

C 标准函数和系统函数调用关系。一个 helloworld 如何打印到屏幕 。

![image-20211125191328503](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211125191328503-1640854382848.png)

#### 1.2 什么是库函数

1.库函数可分为两类，一类是C语言标准库函数，一类是编译器特定的库函数。

2.库函数可以理解为是对系统调用函数的一层封装。尽管系统函数执行效率是比较高效而精简的，但有时我们需要对获取的信息进行更复

杂的处理，或更人性化的需要，我们把这些处理过程封装成一个函数，再将许多这类的函数放在一个文件（库）一般放在 .lib文件。最后

再供程序员使用。

- `#include<stdio.h>`使用的时候包含头文件就可以使用其中的库函数了
- 例如常见的库函数`printf fwrite fread fopen`…等等

#### 1.3 将hello写入到文件1.txt流程

1.首先fopen打开文件 fwrite参数附上要写入的内容

2.文本内容来到C标准缓冲区

3.如果满足条件就刷新C标准缓冲区，调用系统函数write进行写（补充：满了就会自动刷新）

4.write却只是把要写入的内容写到内核缓冲区

5.如果内核缓冲区满足条件就刷新内核缓冲区，系统调用sys_write将缓冲区内容写入到磁盘（补充：有个进程会定时刷新内核缓冲区）

6.此时有进程读取1.txt文件内容，发现内核缓冲区就有这个文件内容，就直接从内核缓冲区读取。

#### 1.4 为什么要有缓冲区（补充）

**定义**：缓冲区就是内存里的一块区域，把数据先存内存里，然后一次性写入硬盘中的文件，类似于数据库的批量操作。

**好处**：减少对硬盘的直接操作，硬盘的执行速度为毫秒级别，内存为纳秒级别。在硬盘直接操作读写效率太低。

#### 1.5 内核缓冲区和C标准缓冲区的区别

  C语言标准库函数fopen()每打开一个文件时候，其都会对应一个**单独**一个缓冲区，而内核缓冲区是**公用的**。

### 2 open()文件操作函数

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920112931560-1640854398800.jpg)

#### 2.1 函数原型

manpage 第二卷，open函数如下，有两个版本的【man 2 open】

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20210914104045155-1640854406377.png" alt="image-20210914104045155"  />

```c
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

#### 2.2 参数描述

**返回一个文件描述符，理解为整数，出错返回-1**。本质是一个已经打开的结构体。

pathname		文件路径

flags 		权限控制，只读，只写，读写。  

```
必选：O_RDONLY:只读、O_WRONLY:只写、O_RDWR:读写
可选：
O_APPEND:追加
O_EXCL:判断文件是否存在
O_TRUNC:截取文件（清空操作）
O_NONBLOCK:非阻塞
第三个参数:取决O_CREAT:创建
取值8进制数，用来描述文件的 访问权限。 rwx    0664
创建文件最终权限 = mode & ~umask
```

第二个open【创建文件】

多了一个mode参数，用来指定文件的权限，数字设定法 0644 -rw-r--r-

文件权限 = mode & ~umask

先打开一个存在的文件

```c
#include <stdio.h>
#include<unistd.h>
#include <fcntl.h>

int main(int argc, char* argv[])
{
        int fd = open("./leetcode.txt",O_RDONLY);
        printf("fd = %d\n",fd);
        close(fd);
        return 0;
}
```

![image-20210914104355147](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20210914104355147-1640854416045.png)

返回值是3

再打开一个不存在

```c
#include <stdio.h>
#include<unistd.h>
#include <fcntl.h>

int main(int argc, char* argv[])
{
        int fd = open("./leetcoe.txt",O_RDONLY|O_CREAT,0644);  // rw--r--r-
        printf("fd = %d\n",fd);
        close(fd);
        return 0;
}
```

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20210914105425085-1640854423505.png" alt="image-20210914105425085" style="zoom:67%;" />

（3）以写方式打开只读文件（权限问题）

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
int main(int argc, char* argv[])
{
        int fd = open("leetcode.123",O_WRONLY);
        printf("fd = %d, errno = %d:%s\n",fd,errno,strerror(errno));
        close(fd);
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920143516545-1640854432719.png" alt="img" style="zoom:80%;" />

（4）以只写方式打开目录

```c
#include <stdio.h>
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
int main(int argc, char* argv[])
{
        int fd = open("myDir",O_WRONLY);
        printf("fd = %d, errno = %d:%s\n",fd,errno,strerror(errno));
        close(fd);
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920143741460-1640854450734.png)

测一下O_TRUNC

```c
#include<stdio.h>
#include<unistd.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd = open("./leetcoe.txt",O_RDONLY|O_CREAT|O_TRUNC,0644);  // rw--r--r-
        printf("fd = %d\n",fd);
        close(fd);
        return 0;
}
```

close函数

```c
int close(int fd);
```

### 3、文件描述符

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920145523668-1640854458732.png)

```
 ulimit -a
```

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920145549428-1640854464589.png" alt="img" style="zoom:80%;" />

操作系统实现并提供给外部应用程序的编程接口；

作用：完成应用程序从User区到kernel区的权级切换；

write：系统函数----系统调用的浅封装；

sys_write：系统调用

应用程序----》标准函数----》系统调用----》驱动----》驱动----》硬件

### 4、read和write函数

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920150816796-1640854473246.jpg)

#### 4.1 read函数原型

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920151318323-1640854589971.png" alt="img" style="zoom: 67%;" />

#### 4.2 read参数使用

- `.fd` 表示改文件的文件描述符，open的返回值
- .`buf` 指缓冲区，读取的数据存放位置
- .`count` 传入缓冲区的字节大小【使用sizeof非strlen】
- .返回值　成功返回读出的字节数 失败返回-1
- （值得注意的是也可能是以非阻塞的方式读一个设备文件和网络文件，后面网络编程常见）

#### 4.3 write函数原型

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920151353203-1640854596899.png" alt="img" style="zoom:67%;" />

#### 4.4 write参数使用

- `fd`  表示改文件的文件描述符，open的返回值
- `buf` 写入的文本内容
- `count` 写入的数据长度【使用srtlen非sizeof】
- 返回值 　成功返回写入的字节数 失败返回-1

#### 4.5 read和write实现cp

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>
#include<unistd.h>
#include<pthread.h>
 
int main(int argc, char* argv[])
{
        char buff[1024];
        int n = 0;
        // 以只读的方式打开argv[1]
        int fd1 = open(argv[1], O_RDONLY);
        if (fd1 == -1)
        {
                perror("open argv[1] error");
                exit(1);
        }
        // 以读写的方式打开argv[2],如果不存在，则创建，文件权限为 -rw-rw-r--
        int fd2 = open(argv[2], O_RDWR|O_CREAT|O_TRUNC, 0664);
 
        if (fd2 == -1)
        {
                perror("open argv[2] error");
                exit(1);
        }
        // 从fd1中读数据存到到buff中，每次读1024个，并赋值给n，如果不足1024,读多少就是多少，当读的数据为0时，读完文件
        while ((n = read(fd1, buff, 1024)) != 0)
        {
                if (n < 0)
                {
                        perror("read error");
                        exit(1);
                }
                // 将读的数据以实际大小n从buff中取出，写到fd2中
                write(fd2, buff, n);
        }
 
 
        // close
        close(fd1);
        close(fd2);
 
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920152104483-1640854615510.png" alt="img" style="zoom:67%;" />

### 5、错误处理函数

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920153007946-1640854620341.png" alt="img" style="zoom: 67%;" />

**perror**

![image-20211126193918163](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211126193918163-1640854627522.png)

**strerror**

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211126193955211-1640854631100.png" alt="image-20211126193955211" style="zoom:80%;" />

### 6、阻塞和非阻塞

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021092015370134-1640854636733.png)

阻塞、非阻塞：  是设备文件、网络文件的属性。

产生阻塞的场景。 读设备文件。读网络文件。（读常规文件无阻塞概念。）

```c
/dev/tty -- 终端文件。

open("/dev/tty", O_RDWR|O_NONBLOCK) --- 设置 /dev/tty 非阻塞状态。(默认为阻塞状态)
```

#### 6.1 阻塞读

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
 
int main(int argc, char* argv[])
{
        char buff[10];
        int n = read(STDIN_FILENO, buff, 10);
        if (n < 0) {
                perror("read STDIN_FILENO");
                exit(1);
        }
 
        write(STDOUT_FILENO, buff, n);
        return 0;
}
```

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920154143603-1640854647793.png)

运行程序后，进入等待状态（阻塞），当有输入时，程序才会接着执行，将输入的数据读入，并输出。

#### 6.2 非阻塞超时等待

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<errno.h>
#include<fcntl.h>
 
#define MSG_TRY "try again\n"
#define MSG_TIMEOUT "time out\n"
 
 
int main(void)
{
        char buff[10];
        int fd, n ,i;
        // 设置非阻塞读
        fd = open("/dev/tty", O_RDONLY|O_NONBLOCK);
 
        if (fd < 0) {
                perror("open /dev/tty");
                exit(1);
        }
        printf("open /dev/tty ok..... %d\n", fd);
 
        for (int i = 0; i < 5; i++) {
                // 读入数据
                n = read(fd, buff, 10);
                // 有数据读入，跳出循环
                if (n > 0) {
                        break;
                }
                if (errno != EAGAIN) {
                        perror("read /edv/tty");
                        exit(1);
                } else {
                        // 没有数据输入，则输出提示
                        write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
                        sleep(2);
                }
        }
        // i等于5时，循环5次，没有读到数据，等待超时
        if (i == 5) {
                write(STDOUT_FILENO, MSG_TIMEOUT, strlen(MSG_TIMEOUT));
        } else {
                // i != 5，即读到数据，将读到的数据输出
                write(STDOUT_FILENO, buff, n);
        }
        close(fd);
        return 0;
}
```

 等待超时。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920155544593-1640854669451.png)

有数据读入，读入数据，并输出，结束程序。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920155629942-1640854674399.png)

### 7、lseek函数

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920161933310-1640854678413.jpg" alt="img" style="zoom:80%;" />

#### 7.1 函数原型

```c
int lseek(int fd, off_t offset, int whence);
```

**文件的读写使用的是同一偏移位置。**

#### 7.2 参数使用

```
fd 　　表示改文件的文件描述符，open的返回值
offset 　表示偏移量
whence 指出偏移的方式
　　 whence参数补充说明
　 SEEK_SET:偏移到文件头+ 设置的偏移量
　 SEEK_CUR：偏移到当前位置+设置的偏移量
　 SEEK_END：偏移到文件尾置+设置的偏移量
```

#### 7.3 读文件

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd, n;
        char msg[] = "It's a test for lseek\n";
        char ch;
        // 以读写的方式打开文件，如果不存在，则创建，权限 -rw-rw-r-
        fd = open("lseek.txt", O_RDWR|O_CREAT, 0664);
        if (fd < 0) {
                perror("open lseek.txt error");
                exit(1);
        }
        // 向文件中写入数据，此时光标在最后
        write(fd, msg, strlen(msg));
        
        // 先注释这行
        //lseek(fd, 0, SEEK_SET);
 
        while ((n = read(fd, &ch, 1))) {
                if (n < 0) {
                        perror("read error");
                        exit(1);
                }   
                // 将读出的数据在标准输出显示
                write(STDOUT_FILENO, &ch, n);
        }
        close(fd);
 
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920163254661-1640854696057.png)

执行后没有数据输出，因为光标在最后位置。**放开注释lseek(fd, 0, SEEK_SET);**

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021092016371742-1640854700375.png)

#### 7.4 读文件大小

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd = open(argv[1], O_RDWR);
        if (fd == -1) {
                perror("read error");
                exit(1);
        }
 
        int length = lseek(fd, 0, SEEK_END);
        printf("file size is %d\n",length);
        close(fd);
        return 0;
}
```

**这里要注意lseek函数返回值的意义**

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920164835527-1640854708836.png)

#### 7.5 填充大小

将7.4中的文件从2900填充为3000，差100字节。

修改代码如下：

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd = open(argv[1], O_RDWR);
        if (fd == -1) {
                perror("read error");
                exit(1);
        }
        // 从文件结束位置再偏移100
        int length = lseek(fd, 100, SEEK_END);
        printf("file size is %d\n",length);
        close(fd);
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920165407617-1640854716045.png)

下面再用ls命令查看

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920165438982.png" alt="img" style="zoom:80%;" />

它的大小不是3000。

**原因是，要使文件大小真正拓展，必须引起IO操作。**

修改后的扩展文件代码如下：

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd = open(argv[1], O_RDWR);
        if (fd == -1) {
                perror("read error");
                exit(1);
        }
 
        int length = lseek(fd, 99, SEEK_END);
        printf("file size is %d\n",length);
        write(fd, "$", 1);
        close(fd);
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920170227876-1640854731385.png)

这里2999和3000是因为lseek读取到偏移差的时候，还没有写入最后的‘$’符号。

查看文件

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920170343649.png)

末尾那一大堆^@，是文件空洞，如果自己写进去的也想保持队形，就写入“\0”。

#### 7.6 补充

拓展文件直接使用truncate，简单粗暴

使用 truncate 函数，直接拓展文件。 int ret = truncate("robot.txt", 5000);

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int length = truncate("robot.txt", 5000);
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920171415305-1640854746375.png" alt="img" style="zoom:67%;" />

### 8、fcntl函数

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920171840667-1640854754545.png" alt="img" style="zoom:67%;" />

函数原型

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920171934980-1640854759353.png)

**这个函数超复杂，这里知识简单学习**

fcntl用来改变一个【已经打开】的文件的 访问控制属性

重点掌握两个参数的使用： F_GETFL 和 F_SETFL

```
fcntl：
	int (int fd, int cmd, ...)
    fd		文件描述符
    cmd		命令，决定了后续参数个数
 
获取文件状态： F_GETFL
设置文件状态： F_SETFL
```

#### 8.1 修改属性

终端文件默认是阻塞读的，这里用fcntl将其更改为非阻塞读。

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<errno.h>
 
#define MSG_TRY "try again\n"
 
int main(int argc, char* argv[])
{
        char buff[10];
        int flags, n;
 
        flags = fcntl(STDIN_FILENO, F_GETFL);    // 获取stdin的属性信息
        if (flags == 1) {
                perror("fcntl error");
                exit(1);
        }
        // 用或位运算符修改属性
        flags |= O_NONBLOCK;
        // 再将属性赋回去，此时stdin就是非阻塞
        int ret = fcntl(STDIN_FILENO, F_SETFL, flags);
        if (ret == -1) {
                perror("fcntl error");
                exit(1);
        }
 
tryagain:
        n = read(STDIN_FILENO, buff, 10);
        if (n < 0) {
                // 出错
                if (errno != EAGAIN) {
                        perror("read /dev/tty");
                        exit(1);
                }
                sleep(3);
                write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
                goto tryagain;
        }
        write(STDOUT_FILENO, buff, n);
 
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920172619248-1640854773265.png)

可以看到，是非阻塞读取。 

## 三、文件系统

### 1、文件存储

**知识体系**

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920195548951-1640854778091.jpg" alt="img" style="zoom:67%;" />

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920195058117-1640854784329.png" alt="img" style="zoom:67%;" />

#### 1.1 dentry

目录项，其本质依然是结构体，重要成员变量有两个 {文件名， inode， ...}，而文件内容(data)保存在磁盘盘块中。

#### **1.2 inode**

其本质为结构体，存储文件的属性信息。如：权限、类型、大小、时间、用户、盘块位置……也叫作文件属性管理结构，大多数的 inode 

都存储在磁盘上。少量常用、近期使用的 inode 会被缓存到内存中。

#### 1.3 硬链接

硬链接和原文件有相同的inode号，有不相同的dentry

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920200145525-1640854795931.png" alt="img" style="zoom: 50%;" />

当断开硬连接时，只是把inode的指向断了。

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920200237806-1640854800410.png" alt="img" style="zoom:50%;" />

### 2、文件操作

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920201824231-1640854804850.jpg" alt="img" style="zoom:67%;" />

#### 2.1 stat函数

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920202503568-1640854812427.png" alt="img" style="zoom:67%;" />

```
获取文件属性， (从 inode 结构体中获取)
获取文件大小： statbuf.st_size
获取文件类型： statbuf.st_mode
获取文件权限： statbuf.st_mode
```

**获取文件大小**

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/stat.h>
 
int main(int argc, char* argv[])
{
        struct stat sbuff;
        int ret = stat(argv[1], &sbuff);
        if (ret == -1) {
                perror("stat  error");
                exit(1);
        }
        printf("file size = %ld\n",sbuff.st_size);
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021092020302190-1640854821833.png" alt="img" style="zoom:80%;" />

**获取文件类型**

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/stat.h>
 
int main(int argc, char* argv[])
{
        struct stat sbuff;
        int ret = stat(argv[1], &sbuff);
        if (ret == -1) {
                perror("stat  error");
                exit(1);
        }
        if (S_ISREG(sbuff.st_mode)) {
                printf("It's a regular file\n");
        } else if (S_ISDIR(sbuff.st_mode)) {
                printf("It's a dir\n");
        } else if (S_ISFIFO(sbuff.st_mode)) {
                printf("It's a pipe\n");
        } else if (S_ISLNK(sbuff.st_mode)) {
                printf("It's a link\n");
        }
 
        return 0;
}
```

这里判断文件类型用S_ISXXX，在man 2 stat里面查看案例

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021092020334689-1640854832066.png" alt="img" style="zoom:67%;" />

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920204517699-1640854837147.png" alt="img" style="zoom:67%;" />

**可以看到这里sss.s是一个连接。stat会拿到符号链接指向那个文件或目录的属性。不想穿透符号就用lstat。**

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210920204712522-1640854843277.png)

#### 2.2 link和unlink函数

硬链接数就是dentry数目；

link就是用来创建硬链接的；

link可以用来实现mv命令。

**函数原型**

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925102257989-1640854848130.png" alt="img" style="zoom:67%;" />

用这个来实现mv，用oldpath来创建newpath，完事儿删除oldpath就行。

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
 
int main(int argc, char* argv[])
{
        link(argv[1], argv[2]);
        unlink(argv[1]);
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925102616835-1640854855357.png" alt="img" style="zoom:67%;" />

unlink是删除一个文件的目录项dentry，使硬链接数-1

unlink函数的特征：**清除文件时，如果文件的硬链接数到0了，没有dentry对应，但该文件仍不会马上被释放，要等到所有打开文件的进**

**程关闭该文件，系统才会挑时间将该文件释放掉。**

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
	int fd, ret;
	char* p = "test of unlink\n";
	char* p2 = "after write something\n";
    // 以读写的方式打开temp.txt文件，如果不存在，则创建该文件，并清空文件。文件的权限是 -rx-r--r-
	fd = open("temp.txt", O_CREAT|O_RDWR|O_TRUNC, 0664);
    // 判断打开情况
	if (fd < 0) {
		perror(" open error");
		exit(1);
	}
	// 把p所指的字符串写到文件中
	ret = write(fd, p, strlen(p));
    // 判断写的情况
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Hi I'm printf\n");
 
    // 再把p2所指的字符串写到文件中
	ret = write(fd, p2, strlen(p2));
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Entry anykey continue\n");
	getchar();
	
    
    // 文件具备被释放的条件
	ret = unlink("temp.txt");
	if (ret == -1) {	
		perror("unlink error");
		exit(1);
	} 
 
	close(fd);
	return 0;
}
```

编译程序并运行，程序阻塞，此时打开新终端查看临时文件temp.txt如下：

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925103831499-1640854869342.png)

可以看到，临时文件没有被删除，这是因为当前进程没结束。输入字符使当前进程结束后，temp.txt就不见了。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925103921367-1640854904094.png)

下面开始搞事，在程序中加入段错误成分，段错误在unlink之前，由于发生段错误，程序后续删除temp.txt的dentry部分就不会再执行，

temp.txt就保留了下来，这是不科学的。修改代码如下：

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
	int fd, ret;
	char* p = "test of unlink\n";
	char* p2 = "after write something\n";
    // 以读写的方式打开temp.txt文件，如果不存在，则创建该文件，并清空文件。文件的权限是 -rx-r--r-
	fd = open("temp.txt", O_CREAT|O_RDWR|O_TRUNC, 0664);
    // 判断打开情况
	if (fd < 0) {
		perror(" open error");
		exit(1);
	}
	// 把p所指的字符串写到文件中
	ret = write(fd, p, strlen(p));
    // 判断写的情况
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Hi I'm printf\n");
 
    // 在这里添加代码，会出现段错误
    p2[3] = 'a';
    // 再把p2所指的字符串写到文件中
	ret = write(fd, p2, strlen(p2));
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Entry anykey continue\n");
	getchar();
	
    
    // 文件具备被释放的条件
	ret = unlink("temp.txt");
	if (ret == -1) {	
		perror("unlink error");
		exit(1);
	} 
 
	close(fd);
	return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925104500496-1640854916280.png)

解决办法是检测fd有效性后，立即释放temp.txt，由于进程未结束，虽然temp.txt的硬链接数已经为0，但还不会立即释放，仍然存在，

要等到程序执行完才会释放。这样就能避免程序出错导致临时文件保留下来。

因为文件创建后，硬链接数立马减为0，即使程序异常退出，这个文件也会被清理掉。这时候的内容是写在内核空间的缓冲区。

代码如下：

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
	int fd, ret;
	char* p = "test of unlink\n";
	char* p2 = "after write something\n";
    // 以读写的方式打开temp.txt文件，如果不存在，则创建该文件，并清空文件。文件的权限是 -rx-r--r-
	fd = open("temp.txt", O_CREAT|O_RDWR|O_TRUNC, 0664);
    // 判断打开情况
	if (fd < 0) {
		perror(" open error");
		exit(1);
	}
 
    // ========================文件具备被释放的条件
	ret = unlink("temp.txt");
	if (ret == -1) {	
		perror("unlink error");
		exit(1);
	} 
 
	// 把p所指的字符串写到文件中
	ret = write(fd, p, strlen(p));
    // 判断写的情况
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Hi I'm printf\n");
 
    // 在这里添加代码，会出现段错误
    p2[3] = 'a';
    // 再把p2所指的字符串写到文件中
	ret = write(fd, p2, strlen(p2));
	if (ret == -1) {	
		perror(" write error");
	}
 
	printf("Entry anykey continue\n");
	getchar();
 
	close(fd);
	return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925105442378-1640854932264.png)

可以发现，虽然程序有bug，但是不会temp.txt文件。这里设计到隐式回收。

**隐式回收：**

当进程结束运行时，所有进程打开的文件会被关闭，申请的内存空间会被释放。系统的这一特性称之为隐式回收系统资源。

比如上面那个程序，要是没有在程序中关闭文件描述符，没有隐式回收的话，这个文件描述符会保留，多次出现这种情况会导致系统文件

描述符耗尽。所以隐式回收会在程序结束时收回它打开的文件使用的文件描述符。

### 3、目录操作

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925110442328-1640854938583.png" alt="img" style="zoom: 67%;" />

#### 3.1 文件目录rwx权限差异

readlink读文件连接本身

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/2021092511073528-1640854945435.png)

 用vim读目录

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925111050764-1640854951167.png)

其内容是该目录文件下的所有子文件的目录项dentry。

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925111107234-1640854955528.png)

文件与目录rwx的不同

|      | r                                       | w                               | x                      |
| ---- | --------------------------------------- | ------------------------------- | ---------------------- |
| 文件 | 文件可以被查看（cat、more、less......） | 文件内容可以被修改（vim......） | 可以运行文件           |
| 目录 | 目录可以被浏览（ls、tree......）        | 目录可以被创建、修改、删除      | 目录可以被打开，进入cd |

#### 3.2 目录操作函数

opendir

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925112328619-1640854964410.png" alt="img" style="zoom:80%;" />

closedir

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925113134391-1640854969725.png" alt="img" style="zoom:67%;" />

readdir

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925113215978-1640854974738.png" alt="img" style="zoom:67%;" />

没有写目录操作，因为目录写操作就是创建文件。可以用touch

下面用目录操作函数实现一个ls操作：

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<dirent.h>        // 加头文件
 
int main(int argc, char* argv[])
{
        DIR* dp;            // 接收opendir的返回值
        struct dirent* sdp; // 接收readdir的返回值
        dp = opendir(argv[1]);
        if (dp == NULL) {
                perror("opendir error");
                exit(1);
        }
        while ((sdp = readdir(dp)) != NULL) {
                printf("%s \t",sdp->d_name);
        }
 
        printf("\n");
        closedir(dp);
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20210925113715318-1640854983129.png)

要隐藏这个.和..的话，在输出文件名的时候判定一下，只输出不是.和..的就行了。 

#### 3.3 递归遍历目录

任务需求：使用opendir closedir readdir stat实现一个递归遍历目录的程序

输入一个指定目录，默认为当前目录。递归列出目录中的文件，同时显示文件大小。

**思路分析**

1.判断命令行参数，获取用户要查询的目录名。

2.判断用户指定的是否是目录。 stat  S_ISDIR(); --> 封装函数 isFile() {  }。

3.打开目录-》读目录-》关闭目录。

**先写个简易版的，可以判定文件，读取文件大小**

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/stat.h>
 
// 判断文件函数
void isFile(char* name) {
        int res = 0;
        struct stat sbuff;
        // 获取文件属性
        res = stat(name, &sbuff);
        if (res == -1) {
                perror("stat error\n");
                return;
        }
        // 如果是目录
        if (S_ISDIR(sbuff.st_mode)) {
 
        }
        // 不是目录
        printf("%s\t%ld\n",name, sbuff.st_size);
 
        return;
}
 
int main(int argc, char* argv[])
{
        // 1判断参数
        if (argc == 1) {
                isFile(".");
        } else {
                isFile(argv[1]);
        }
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20211001200443884-1640854998139.png" alt="img" style="zoom:67%;" />

下面完善功能，把对目录的递归处理补全，如下

```c
#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/stat.h>
#include<dirent.h>
 
void isFile(char* name);
// 读目录
void readDir(char* dir) {
        // open read close
        char path[256];
        DIR* dp;
        dp = opendir(dir);
 
        if (dp == NULL) {
                perror("opendir error");
                return;
        }
        struct dirent* sdp;
        while ((sdp = readdir(dp)) != NULL) {
                // 去掉.和..
                if (strcmp(sdp->d_name, ".") == 0 || strcmp(sdp->d_name, "..") == 0) {
                        continue;
                }
                // 拼接路径
                sprintf(path, "%s/%s", dir, sdp->d_name);
                isFile(path);
        }
 
        // 关闭文件
        closedir(dp);
 
        return;
}
 
// 判断文件函数
void isFile(char* name) {
        int res = 0;
        struct stat sbuff;
 
        res = stat(name, &sbuff);
        if (res == -1) {
                perror("stat error\n");
                return;
        }
        // 是目录
        if (S_ISDIR(sbuff.st_mode)) {
                readDir(name);
        }
        printf("%10s\t\t%ld\n",name, sbuff.st_size);
 
        return;
}
 
int main(int argc, char* argv[])
{
        // 1判断参数
        if (argc == 1) {
                isFile(".");
        } else {
                isFile(argv[1]);
        }
        return 0;
}
```

执行

![img](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/20211001200633294-1640855012811.png)

### 4、重定向dup和dup2

用来做重定向，本质就是复制文件描述符

#### 4.1 函数原型

![image-20211126205043035](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211126205043035-1640855019013.png)

```
int dup(int oldfd);		文件描述符复制。
		oldfd: 已有文件描述符
		返回：新文件描述符，这个描述符和oldfd指向相同内容。

int dup2(int oldfd, int newfd); 文件描述符复制，oldfd拷贝给newfd。返回newfd
```

#### 4.2 函数实现

一个小例子，给一个旧的文件描述符，返回一个新文件描述符

```c
/*************************************************************************
 > File Name: dup.c
 > Author: Winter
 > Created Time: 2021年10月01日 星期五 20时10分56秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd = open(argv[1], O_RDWR);     // 0 1 2 ----3
        if (fd == -1) {
                perror("open error\n");
                exit(1);
        }
        int newFd = dup(fd);
        printf("newFd = %d\n", newFd);       // 4
        return 0;
}
```

执行

![image-20211126205204093](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211126205204093-1640855029483.png)

dup基本就这样了，后续使用也就起个保存作用。

![image-20211126205223458](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211126205223458-1640855033764.png)

可以往文件里面写数据。

```c
/*************************************************************************
 > File Name: dup.c
 > Author: Winter
 > Created Time: 2021年10月01日 星期五 20时10分56秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd = open(argv[1], O_RDWR);     // 0 1 2 ----3
        if (fd == -1) {
                perror("open error\n");
                exit(1);
        }
        int newFd = dup(fd);
        write(newFd, "1234567", 7);
        printf("newFd = %d\n", newFd);       // 4
        return 0;
}
```

下面讲dup2 (dupto)：

下面这个例子，将一个已有文件描述符fd1复制给另一个文件描述符fd2，然后用fd2修改fd1指向的文件：

```c
/*************************************************************************
 > File Name: dup.c
 > Author: Winter
 > Created Time: 2021年10月01日 星期五 20时10分56秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd1 = open(argv[1], O_RDWR);     // 0 1 2 ----3

        int fd2 = open(argv[1], O_RDWR);     // 0 1 2 ----3
        int fdres = dup2(fd1, fd2);
        printf("fdres = %d\n", fdres);       // 4

        int res = write(fd2, "123456789", 9);

        printf("res = %d\n", res);       //
        return 0;
}
```

执行

![image-20211128144242618](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128144242618-1640855047456.png)

查看

![image-20211128144406788](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128144406788-1640855051924.png)

上面那个例子，fd1是打开hello.c的文件描述符，fd2是打开hello2.c的文件描述符

**用dup2将fd1复制给了fd2，于是在对fd2指向的文件进行写操作时，实际上就是对fd1指向的hello.c进行写操**

**作。**

这里需要注意一个问题，由于hello.c和hello2.c都是空文件，所以直接写进去没关系。但如果hello.c是非空的，写

进去的内容默认从文件头部开始写，会覆盖原有内容。

**dup2也可以用于标准输入输出的重定向。**

下面这个例子，将输出到STDOUT的内容重定向到文件里：

```c
/*************************************************************************
 > File Name: dup.c
 > Author: Winter
 > Created Time: 2021年10月01日 星期五 20时10分56秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd1 = open(argv[1], O_RDWR);     // 0 1 2 ----3

        int fd2 = open(argv[2], O_RDWR);     // 0 1 2 ----3
        int fdres = dup2(fd1, fd2);
        printf("fdres = %d\n", fdres);       // 4

        int res = write(fd2, "123456789", 9);

        printf("res = %d\n", res);       //
        dup2(fd1, STDOUT_FILENO);
        printf("____________-----------------------------886");

        return 0;
}
```

执行

![image-20211128144645203](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128144645203-1640855064685.png)

这个程序，将fd1的内容复制给了fd2，使得原来指向hello2.c的fd2也指向了hello.c

并通过fd2向hello.c里写入了【____________-----------------------------886】。完事儿将标准输出重定向至fd1，就是将要显示在

标准输出的内容，写入了fd1指向的文件，就是hello.c中。

这里有一点和上面程序不同，就是hello.c是处于打开状态的，连续写入两段话，写入【____________----------------------------

-886】的时候，读写指针在这句话末尾，就不会覆盖这句话，所以，没有问题的。这里再强调一下，打开一个文

件，读写指针默认在文件头，如果文件本身有内容，直接写入会覆盖原有内容。

### 5、fcntl实现dup描述符

#### 5.1 函数原型

```
int fcntl(int fd, int cmd, ....)
	cmd: F_DUPFD
	参3:   被占用的，返回最小可用的。
				未被占用的， 返回=该值的文件描述符。
```

用fcntl实现描述符的复制

```c
/*************************************************************************
 > File Name: fcntl.c
 > Author: Winter
 > Created Time: 2021年10月07日 星期四 19时21分44秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd1 = open(argv[1], O_RDWR);
        if (fd1 == -1) {
                perror("open error\n");
        }
        printf("fd1 = %d \n",fd1);

        int fd2 = fcntl(fd1, F_DUPFD, 0);
        printf("fd2 = %d\n", fd2);
        return 0;
}
```

执行

![image-20211128145535517](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128145535517-1640855076589.png)

**对于fcntl中的参数0，这个表示0被占用，fcntl使用文件描述符表中的最小文件描述符返回**

**假设传入0，传一个7，且7未被占用，则会返回7**

所以这个参数可以这样理解，你传入一个文件描述符k，如果k没被占用，则直接用k复制fd1的内容。如果k被占用，则返回描述符表中最

小可用描述符，也就是自己指定一个一志愿，如果行，就返回这个。如果不行，国家给你分配一个最小的。

编译执行，如下：

```c
/*************************************************************************
 > File Name: fcntl.c
 > Author: Winter
 > Created Time: 2021年10月07日 星期四 19时21分44秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>

int main(int argc, char* argv[])
{
        int fd1 = open(argv[1], O_RDWR);
        if (fd1 == -1) {
                perror("open error\n");
        }
        printf("fd1 = %d \n",fd1);

        int fd2 = fcntl(fd1, F_DUPFD, 0);
        printf("fd2 = %d\n", fd2);

        fd2 = fcntl(fd1, F_DUPFD, 7);
        printf("fd2 = %d\n", fd2);
        return 0;
}
```

执行

![image-20211128145804104](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128145804104-1640855087755.png)

## 四、进程

### 1、进程相关概念

> 一些比较基本的概念，如果学过操作系统，这些内容都很常见，可以适当跳过…
>
> - 单道程序设计
> - 多道程序设计
> - 微观串行、宏观并行 时间片
> - 同步 和  异步
> - 并发和并行

#### 1.1 程序和进程

“程序(Program)”是一个静态的概念，一般对应于操作系统中的一个可执行文件。

执行中的程序叫做进程(Process)，是一个动态的概念。现代的操作系统都可以同时启动多个进程。比如：我们在用酷狗听音乐，也可以使

用IDEA写代码，也可以同时用浏览器查看网页。

```
程序：死的。只占用磁盘空间。		                   ——剧本。
进程；活的。运行起来的程序。占用内存、cpu等系统资源。	——戏。
```

#### 1.2 并发

并发是多个任务交替执行的，多个任务之间可能还是串行的。所有的并发处理都有排队等候，唤醒和执行这三个步骤。所以并发是宏观的

观念，在微观上他们都是序列被处理的，只不过资源不会在某一个上被阻塞（一般是通过时间片轮转），所以在宏观上多个几乎同时到达

的请求同时在被处理。如果是同一时刻到达的请求也会根据优先级的不同，先后进入队列排队等候执行。并发针对的是多个请求，比如：

一个CPU，一个web服务，同时涌入多个请求，CPU需要交替切换的执行多个请求，而不是一个请求执行完成之后再执行下一个请求。并

发的实质是一个物理CPU（也可以是多个物理CPU）在若干个程序之间多路复用，并发性是对有限物理资源强制行使 多用户共享以提高效

率。

> 并发和并行：并行是宏观上并发，微观上串行。

#### 1.3 CPU和MMU

![image-20211128151416002](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128151416002-1640855098965.png)

#### 1.4 虚拟内存和物理内存映射关系

**虚拟内存**：虚拟内存是一种逻辑上扩充物理内存的技术。基本思想是用软、硬件技术把内存与外存这两级存储器当做一级存储器来用。虚

拟内存技术的实现利用了**自动覆盖和交换技术**。简单的说就是**将硬盘的一部分作为内存来使用**。

**虚拟地址空间**：在32位的i386 CPU的地址总线的是32位的，也就是说可以寻找到4G的地址空间。我们的程序被CPU执行，就是

0x000000000xFFFFFFFF这一段地址中。**高1G的空间为内核空间，由操作系统调用，低3G的空间为用户空间，由用户使用。**

CPU在寻址的时候，是按照虚拟地址来寻址，然后通过MMU(内存管理单元)将虚拟地址转换为物理地址。因为只有程序的一部分加入到内

存中，所以会出现所寻找的地址不在内存中的情况（CPU产生缺页异常），如果在内存不足的情况下，就会通过页面调度算法来将内存中

的页面置换出来，然后将在外存中的页面加入到内存中，使程序继续正常运行

![image-20211128151704491](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128151704491-1640855107166.png)

#### 1.5 PCB进程控制块

 我们知道，每个进程在内核中都有一个进程控制块（PCB）来维护进程相关的信息，Linux内核的进程控制块是 task_struct 结构体。

位置

```shell
/usr/src/linux-hwe-5.11-headers-5.11.0-38/include/linux/sched.h
```

![image-20211128153015485](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128153015485-1640855114430.png)

其内部成员有很多，我们重点掌握以下部分即可  

```
* 进程 id。系统中每个进程有唯一的 id，在 C 语言中用 pid_t 类型表示，其实就是一个非负整数。
* 进程的状态，有就绪、运行、挂起、停止等状态。
* 进程切换时需要保存和恢复的一些 CPU 寄存器。
* 描述虚拟地址空间的信息。
* 描述控制终端的信息。
* 当前工作目录（Current Working Directory）。
* umask 掩码。
* 文件描述符表，包含很多指向 file 结构体的指针。
* 和信号相关的信息。
* 用户 id 和组 id。
* 会话（Session）和进程组。
* 进程可以使用的资源上限（Resource Limit）。
```

文件id查看指令

```
ps aux
```

![image-20211128153332522](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128153332522-1640855122167.png)

#### 1.6 进程状态

进程基本的状态有 5 种。分别为新建态，就绪态，运行态，阻塞态与终止态。其中新建态为进程准备阶段，常与就绪态结合来看。

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128153447483-1640855128799.png" alt="image-20211128153447483" style="zoom:67%;" />

转换

```
NULL→新建态：执行一个程序，创建一个子进程。
新建态→就绪态：当操作系统完成了进程创建的必要操作，并且当前系统的性能和虚拟内存的容量均允许。
运行态→终止态：当一个进程到达了自然结束点，或是出现了无法克服的错误，或是被操作系统所终结，或是被其他有终止权的进程所终结。
运行态→就绪态：运行时间片到；出现有更高优先权进程。
运行态→等待态：等待使用资源；如等待外设传输；等待人工干预。
就绪态→终止态：未在状态转换图中显示，但某些操作系统允许父进程终结子进程。
等待态→终止态：未在状态转换图中显示，但某些操作系统允许父进程终结子进程。
终止态→NULL：完成善后操作。
```

#### 1.7 环境变量

环境变量， 是指在操作系统中用来指定操作系统运行环境的一些参数。 通常具备以下特征：

① 字符串(本质) ；② 有统一的格式：名=值[:值]； ③ 值用来描述进程环境信息。  

```
echo $PATH   查看环境变量
path环境变量里记录了一系列的值，当运行一个可执行文件时，系统会去环境变量记录的位置里查找这个文件并执行。
echo $TERM  查看终端
echo $LANG  查看语言
env         查看所有环境变量
```

![image-20211128153854237](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128153854237-1640855137423.png)

### 2、进程控制

#### 2.1 fork函数原理

**函数原型**

```c
pid_t fork(void);
```

创建子进程。父子进程各自返回。成功：父进程返回子进程pid。 子进程返回 0.

**On success**, the PID of the child process is returned in the parent, and 0 is returned in the child. 

**On failure**, -1 is returned in the parent,no child process is created, and errno is set appropriately.

两个函数：

```c
pid_t getpid()    // 获取当前进程id
pid_t getppid()   // 获取当前进程的父进程id
```

![image-20211128154614515](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128154614515-1640855147078.png)

父子进程相同：

```
刚fork后。 data段、text段、堆、栈、环境变量、全局变量、宿主目录位置、进程工作目录位置、信号处理方式。
```

父子进程不同：

```
进程id、返回值、各自的父进程、进程创建时间、闹钟、未决信号集。
```

父子进程共享：

```
读时共享、写时复制。———————— 全局变量。
```

1.文件描述符；2.mmap映射区。



![image-20211128154826719](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128154826719-1640855154084.png)

#### 2.2 fork创建子进程

1 fork创建子进程

```c

/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        printf("before fork-----------4\n");
        printf("before fork-----------3\n");
        printf("before fork-----------2\n");
        printf("before fork-----------1\n");

        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                printf("child is created\n");
        } else {
                printf("parent process: my child id %d\n",pid);
        }

        printf("=================end\n");
        return 0;
}
```

执行

![image-20211128160214907](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128160214907-1640855166597.png)

关于这里为啥终端提示符和输出信息混在了一起，循环创建多个子进程（后面第二节）那一节会进行分析，现在先不用管。

**fork之前的代码，父子进程都有，但是只有父进程执行了，子进程没有执行，fork之后的代码，父子进程都有机会执行。**

加上两个函数

```c
pid_t getpid()    // 获取当前进程id
pid_t getppid()   // 获取当前进程的父进程id
```

代码

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        printf("before fork-----------4\n");
        printf("before fork-----------3\n");
        printf("before fork-----------2\n");
        printf("before fork-----------1\n");

        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                printf("child is created, pid = %d, parent-pid = %d\n",getpid(), getppid());
        } else {
                printf("parent process, pid = %d, child pid = %d ,parent-pid = %d\n",getpid(), pid, getppid());
        }

        printf("=================end\n");
        return 0;
}
```

执行

![image-20211128160730424](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128160730424-1640855178442.png)

父进程的id是49155，其子进程的id是49156，其父进程id是4339；

子进程的id是49156，其父进程的id是45155。没问题，可以对应。父进程的父进程id是4339，对应的是bash进程。

**循环创建多个子进程**

![image-20211128161108849](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128161108849-1640855185028.png)

所以，直接用个for循环是要出事情的，因为子进程也会fork新的进程

这里，对调用fork的进程进行判定，只让父进程fork新的进程就行，代码如下：

这里是最终版，让子进程按顺序出现，并且父进程最后出现，需要用sleep休眠。

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        int i;
        pid_t pid;
        for (i = 0; i < 5; i++) {
                pid = fork();
                if (pid == 0) {
                        break;
                }
        }
        if (i == 5) {
                sleep(5);
                printf("I'm parent\n");
        } else {
                sleep(i);
                printf("I'm %dth child\n", i + 1);
        }

        return 0;
}
```

执行

![image-20211128162441148](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128162441148-1640855198595.png)

#### 2.3 父子进程共享

父子进程相同：

```
 刚fork后。 data段、text段、堆、栈、环境变量、全局变量、宿主目录位置、进程工作目录位置、信号处理方式。
```

父子进程不同：

```
进程id、返回值、各自的父进程、进程创建时间、闹钟、未决信号集。
```

父子进程共享：

```
读时共享、写时复制。———————— 全局变量。
1.文件描述符 2. mmap映射区。
```

父子进程之间在fork后，有哪些相同？哪些不同

相同之处

```
全局变量、.data、.text、栈、堆、环境变量、用户id、宿主目录、进程工作目录、信号处理方式
```

不同之处

```
进程id、fork返回值、父进程id、进程运行时间、闹钟、未决信号集
```

似乎，子进程复制了父进程0-3G用户空间内容以及父进程的PCB，但pid不同。

真的每fork一个子进程都要将父进程的0-3G地址空间完全拷贝一份，然后再映射到物理内存吗？

当然不是，父子进程间遵循【**读时共享写时复制**】的原则。这样设计，无论子进程执行父进程的逻辑还是执行自己的逻辑都能节省内存开

销。

**【父子进程不共享全局变量】**

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
int var = 100;
int main(int argc, char* argv[])
{

        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                var = 200;
                printf("var = %d\n", var);
                printf("child is created, pid = %d, parent-pid = %d\n",getpid(), getppid());
        } else {
                var = 300;
                printf("var = %d\n", var);
                printf("parent process, pid = %d, child pid = %d ,parent-pid = %d\n",getpid(), pid, getppid());
        }

        printf("=================end\n");
        return 0;
}
```

执行，发现变量都改变

![image-20211128163902701](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128163902701-1640855211363.png)

注释掉子进程的修改，修改父进程不会影响子进程。

![image-20211128163931715](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128163931715-1640855217508.png)

#### 2.4 父子进程gdb调试

设置父进程调试路径：set follow-fork-mode parent (默认)

![image-20211128164107792](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128164107792-1640855315342.png)

设置子进程调试路径：set follow-fork-mode child

注意，一定要在fork函数调用之前设置才有效。

![image-20211128164132878](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211128164132878-1640855329157.png)

#### 2.5 exec函数族

fork 创建子进程后执行的是和父进程相同的程序（但有可能执行不同的代码分支），子进程往往要调用一种 exec 

函数以执行另一个程序。当进程调用一种 exec 函数时，该进程的用户空间代码和数据完全被新程序替换，从新程

序的启动例程开始执行。调用 exec 并不创建新进程，所以调用 exec 前后该进程的 id 并未改变。

将当前进程的.text、 .data 替换为所要加载的程序的.text、 .data，然后让进程从新的.text第一条指令开始执行，但进程 ID 不变，换核不换壳。  

其实有六种以 exec 开头的函数，统称 exec 函数：  

```c
int execl(const char *path, const char *arg, ...);
int execlp(const char *file, const char *arg, ...);
int execle(const char *path, const char *arg, ..., char *const envp[]);
int execv(const char *path, char *const argv[]);
int execvp(const char *file, char *const argv[]);
int execve(const char *path, char *const argv[], char *const envp[]);
```

![image-20211129192333378](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129192333378-1640855537100.png)

> exec函数族：使进程执行某一程序。成功无返回值，失败返回 -1。

```c
int execlp(const char *file, const char *arg, ...);  // 借助 PATH 环境变量找寻待执行程序
	// 参1： 程序名
	// 参2： argv0
	// 参3： argv1
	// ...： argvN
	// 哨兵：NULL
    
int execl(const char *path, const char *arg, ...);		// 自己指定待执行程序路径。
int execvp();
```

```shell
ps ajx --> pid ppid gid sid
```

![image-20211129192628510](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129192628510-1640855546221.png)

##### 2.5.1 execlp和ececl函数

```c
int execlp(const char *file, const char *arg, …)   // p是path。
	// 成功，无返回，失败返回-1
	// 参数1：要加载的程序名字，该函数需要配合PATH环境变量来使用，当PATH所有目录搜素后没有参数1则返回出错。
```

**该函数通常用来调用系统程序。如ls、date、cp、cat命令。execlp这里面的p，表示要借助环境变量来加载可执行文件**

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
        //      execlp("ls", "-l", "-a", NULL);  // error
                execlp("ls", "ls" ,"-l", "-a", NULL);  // NULL must
                perror("exec error");
                exit(1);
        } else if (pid > 0){
                sleep(1);
                printf("I'm parent, my pid id %d\n",getpid());
        }
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129193144110-1640855560395.png" alt="image-20211129193144110" style="zoom:80%;" />

于是子进程就能随意调用可执行程序了，这个可执行程序可以是系统的，也可以是自定义的。

下面使用execl来让子程序调用自定义的程序。

```c
int execl(const char *path, const char *arg, …)
// 这里要注意，和execlp不同的是，第一个参数是路径，不是文件名。
```

这个路径用相对路径和绝对路径都行。

test.c

```c
/*************************************************************************
 > File Name: test.c
 > Author: Winter
 > Created Time: 2021年10月10日 星期日 21时30分23秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        printf("hello world\n");
        return 0;
}
```

exec代码如下：

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                execl("./test", "./test", NULL);  // NULL must
                perror("exec error");
                exit(1);
        } else if (pid > 0){
                sleep(1);
                printf("I'm parent, my pid id %d\n",getpid());
        }
        return 0;
}
```

执行

![image-20211129193529565](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129193529565-1640855594055.png)

这就很强势了。

用execl也能执行ls这些，把路径给出来就行，但是这样麻烦，所以对于系统指令一般还是用execlp

用execl执行ls命令

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid = fork();
        if (pid == -1) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                execl("/bin/ls", "ls", "-l", NULL);  // NULL must
                perror("exec error");
                exit(1);
        } else if (pid > 0){
                sleep(1);
                printf("I'm parent, my pid id %d\n",getpid());
        }
        return 0;
}
```

执行

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129193729311-1640855633079.png" alt="image-20211129193729311" style="zoom:80%;" />

##### 2.5.2 exec函数族特性

写一个程序，使用execlp执行进程查看，并将结果输出到文件里。

要用到open, execlp, dup2

代码如下：

```c
/*************************************************************************
 > File Name: exec_ps.c
 > Author: Winter
 > Created Time: 2021年10月10日 星期日 21时50分32秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd = open("ps.out", O_WRONLY|O_CREAT|O_TRUNC, 0644);
        if (fd < 0) {
                perror("open error");
        }
        // 重定向
        dup2(fd, STDOUT_FILENO);
        execlp("ps", "ps", "aux", NULL);
        close(fd);
        return 0;
}
```

执行

![image-20211129194214755](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129194214755-1640855715552.png)

exec函数族一般规律：

> exec函数一旦调用成功，即执行新的程序，不返回。只有失败才返回，错误值-1，所以通常我们直接在exec函数调用后直接调用
>
> perror()，和exit()，无需if判断。
>
> l(list)       命令行参数列表
>
> p(path)       搜索file时使用path变量
>
> v(vector)      使用命令行参数数组
>
> e(environment)    使用环境变量数组，不适用进程原有的环境变量，设置新加载程序运行的环境变量。
>
> 事实上，只有execve是真正的系统调用，其他5个函数最终都调用execve，是库函数，所以execve在man手册第二节，其它函数在
>
> man手册第3节。

#### 2.6 孤儿进程和僵尸进程

**孤儿进程**：父进程先于子进终止，子进程沦为“孤儿进程”，会被 init 进程领养。

```shell
ps ajx
```

![image-20211129194831765](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129194831765-1640855739624.png)

**父进程id、进程id、组进程id、会话进程id**

模拟父进程终止，子进程循环，子进程称为孤儿进程。

```c
/*************************************************************************
 > File Name: orphan.c
 > Author: Winter
 > Created Time: 2021年10月11日 星期一 19时50分50秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid = fork();
				// 子进程执行逻辑
        if (pid == 0) {
                while (1) {
                        printf("I'm child, my parent is %d\n",getppid());
                        sleep(1);
                }
        } else if (pid > 0) {
          // 父进程执行逻辑
                printf("I'm parent,my pid is %d\n",getpid());
                sleep(9);
                printf("---------------I'm going to die-------------------\n");
        } else if (pid == -1) {
                perror("fork error\n");
                exit(1);
        }
        return 0;
}
```

执行

![image-20211129195540739](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129195540739-1640855751201.png)

运行orphan进行，父进程pid是28364，子进程pid是28365。父进程的父进程是27069。

当orphan中父进程结束后，父进程变成1568。

![image-20211129195759965](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129195759965-1640855758843.png)

查看1568

![image-20211129195901803](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129195901803-1640855765896.png)

这里/lib/systemd/systemd --user是”孤儿收养院“。

杀掉子进程：kill -9 28365

**僵尸进程**: 子进程终止，父进程没有及时回收，子进程残留资源（PCB）存放于内核中，变成僵尸（Zombie）进程。

kill 对其无效。**这里要注意，每个进程结束后都必然会经历僵尸态，时间长短的差别而已。**

- 如何解决问题呢，此时杀死父进程，父进程转变成init。init发现子进程是僵尸，自动回收。

> 什么是僵尸进程？Unix进程模型中，进程是按照父进程产生子进程，子进程产生子子进程这样的方式创建出完成各项相互协作功能的
>
> 进程的。当一个进程完成它的工作终止之后，它的父进程需要调用wait()或者waitpid()系统调用取得子进程的终止状态。如果父进程
>
> 没有这么做的话，会产生什么后果呢？此时，子进程虽然已经退出了，但是在系统进程表中还为它保留了一些退出状态的信息，如果
>
> 父进程一直不取得这些退出信息的话，这些进程表项就将一直被占用，此时，这些占着茅坑不拉屎的子进程就成为“僵尸进
>
> 程”（zombie）。系统进程表是一项有限资源，如果系统进程表被僵尸进程耗尽的话，系统就可能无法创建新的进程。
>
> 那么，孤儿进程又是怎么回事呢？孤儿进程是指这样一类进程：在进程还未退出之前，它的父进程就已经退出了，一个没有了父进程
>
> 的子进程就是一个孤儿进程（orphan）。既然所有进程都必须在退出之后被wait()或waitpid()以释放其遗留在系统中的一些资源，那
>
> 么应该由谁来处理孤儿进程的善后事宜呢？这个重任就落到了init进程身上，init进程就好像是一个民政局，专门负责处理孤儿进程的
>
> 善后工作。每当出现一个孤儿进程的时候，内核就把孤儿进程的父进程设置为init，而init进程会循环地wait()它的已经退出的子进
>
> 程。这样，当一个孤儿进程“凄凉地”结束了其生命周期的时候，init进程就会代表党和政府出面处理它的一切善后工作。
>
> 
>
> 这样来看，孤儿进程并不会有什么危害，真正会对系统构成威胁的是僵尸进程。那么，什么情况下僵尸进程会威胁系统的稳定呢？设
>
> 想有这样一个父进程：它定期的产生一个子进程，这个子进程需要做的事情很少，做完它该做的事情之后就退出了，因此这个子进程
>
> 的生命周期很短，但是，父进程只管生成新的子进程，至于子进程退出之后的事情，则一概不闻不问，这样，系统运行上一段时间之
>
> 后，系统中就会存在很多的僵尸进程，倘若用ps命令查看的话，就会看到很多状态为Z的进程。严格地来说，僵尸进程并不是问题的
>
> 根源，罪魁祸首是产生出大量僵尸进程的那个父进程。因此，当我们寻求如何消灭系统中大量的僵尸进程时，答案就是把产生大量僵
>
> 尸进程的那个元凶枪毙掉（通过kill发送SIGTERM或者SIGKILL信号）。枪毙了元凶进程之后，它产生的僵尸进程就变成了孤儿进程，
>
> 这些孤儿进程会被init进程接管，init进程会wait()这些孤儿进程，释放它们占用的系统进程表中的资源，这样，这些已经“僵尸”的孤
>
> 儿进程就能瞑目而去了。

子进程终止时，子进程残留资源PCB存放于内核中，PCB记录了进程结束原因，进程回收就是回收PCB。回收僵尸进程，得kill它的父进

程，让孤儿院去回收它。

```c
/*************************************************************************
 > File Name: orphan.c
 > Author: Winter
 > Created Time: 2021年10月11日 星期一 19时50分50秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid = fork();
				// 子进程执行逻辑
        if (pid == 0) {
                printf("I'm child,my pid = %d, my parent = %d\n",getpid(), getppid());
                sleep(9);
                printf("---------------I'm going to die-------------------\n");
        } else if (pid > 0) {
          // 父进程执行逻辑
                while (1) {
                        printf("I'm parent, my pid = %d, my son pid = %d\n",getpid(), pid);
                        sleep(1);
                }
        } else if (pid == -1) {
                perror("fork error\n");
                exit(1);
        }

        return 0;
}
```

执行

![image-20211129201414208](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129201414208-1640855827090.png)

子进程pid是38715，父进程pid是38714。

执行完后，子进程运行结束，父进程循环打印语句。

![image-20211129201615778](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129201615778-1640855911739.png)

子进程是38715成为僵尸进程。如何杀掉僵尸化子进程呢?杀掉父进程。

![image-20211129201723134](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129201723134-1640855917812.png)

#### 2.7 wait和waitpid函数

```c
	// wait函数：	回收子进程退出资源， 阻塞回收任意一个。
pid_t wait(int *status)
	// 参数：（传出） 回收进程的状态。
	// 返回值：成功： 回收子进程的pid
	// 			 失败： -1， errno

	// 函数作用1：	阻塞等待子进程退出【等子进程死亡，回收】
	// 函数作用2：	清理子进程残留在内核的 pcb 资源
	// 函数作用3：	通过传出参数，得到子进程结束状态
```

**一个进程终止时会关闭所有文件描述符，释放在用户空间分配的内存，但它的PCB还保留着，内核在其中保存了一些信息：如果是正常终止则保存着退出状态，如果是异常终止则保存着导致该进程终止的信号是哪个。这个进程的父进程可以调用wait或者waitpid获取这**

**些信息，然后彻底清除掉这个进程。我们知道一个进程的退出状态可以在shell中用特殊变量$？查看，因为shell是它的父进程，当它**

**终止时，shell调用wait或者waitpid得到它的退出状态，同时彻底清除掉这个进程。**

##### 2.7.1 wait回收子进程和异常终止信号

下面这个例子，使用wait来阻塞回收子进程

```c
/*************************************************************************
 > File Name: wait001.c
 > Author: Winter
 > Created Time: 2021年11月29日 星期一 20时39分33秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        pid_t pid, wpid;
        int status;

        pid = fork();
        if (pid == 0) {
                printf("I'm  child, my pid = %d, my parent pid = %d\n", getpid(), getppid());
                sleep(9);
                printf("I'm child, I'm going to die\n");
        } else if (pid > 0) {
                // 父进程回收子进程
                wpid = wait(&status);
                if (wpid == -1) {
                        perror("wait error\n");
                        exit(1);
                }
                printf("parent wait finish:%d\n", wpid);
        } else if (pid == -1) {
                perror("fork error\n");
                exit(1);
        }
        return 0;
}
```

执行

![image-20211129204601717](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129204601717-1640855938105.png)

改进

```c
/*************************************************************************
 > File Name: orphan.c
 > Author: Winter
 > Created Time: 2021年10月11日 星期一 19时50分50秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        pid_t pid, wpid;
        int status;

        pid = fork();
        if (pid == 0) {
                printf("I'm child,my pid = %d, my parent = %d\n",getpid(), getppid());
                sleep(10);
                printf("---------------I'm going to die-------------------\n");
                // 特殊值，演示正常退出的返回值
                return 73;
        } else if (pid > 0) {
                // 父进程回收子进程   如果子进程未终止，父进程会阻塞在这个函数上
                // wpid = wait(NULL);        // 不关心子进程结束原因
                wpid = wait(&status);
                if (wpid == -1) {
                        perror("wait error\n");
                        exit(1);
                }
                // 为真
                if (WIFEXITED(status)) {
                        // 说明子进程正常终止
                        printf("child exit with  %d\n",WEXITSTATUS(status));
                }
                // 为真，说明子进程被信号终止
                if (WIFSIGNALED(status)) {
                        printf("child is killed signal with  %d\n",WTERMSIG(status));
                }
                printf("parent wait finish:%d\n",wpid);

        } else if (pid == -1) {
                perror("fork error\n");
                exit(1);
        }

        return 0;
}
```

status的作用：

```
获取子进程正常终止值：

WIFEXITED(status) --》 为真 --》调用 WEXITSTATUS(status) --》 得到 子进程 退出值。

获取导致子进程异常终止信号：

WIFSIGNALED(status) --》 为真 --》调用 WTERMSIG(status) --》 得到 导致子进程异常终止的信号编号。
```

```
man 2 wait    # 查看函数
```

![image-20211129202939448](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129202939448-1640859222252.png)

执行

![image-20211129203041826](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129203041826-1640859230549.png)

异常退出

![image-20211129203402049](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129203402049-1640859238211.png)

##### 2.7.2 waitpid回收子进程

```c
// waitpid函数：	指定某一个进程进行回收。可以设置非阻塞。			
waitpid(-1, &status, 0) == wait(&status);
	pid_t waitpid(pid_t pid, int *status, int options)
	// 参数：
	// pid：指定回收某一个子进程pid
	//			> 0: 待回收的子进程pid
	//			-1：任意子进程
	//			0：同组的子进程。
	
  // status：（传出） 回收进程的状态。
	// options：WNOHANG 指定回收方式为，非阻塞。

	// 返回值：
	// 			> 0 : 表成功回收的子进程 pid
	//			0 : 函数调用时， 参3 指定了WNOHANG。并且，没有子进程结束。
	//	  	-1: 失败。errno
```

一次wait/waitpid函数调用，只能回收一个子进程。上一个例子，父进程产生了5个子进程，wait会随机回收一个，捡到哪个算哪个。

父进程先回收，子进程后结束

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        int i;
        pid_t pid, wpid;
        for (i = 0; i < 5; i++) {
                pid = fork();
                if (pid == 0) {
                        break;
                }
        }
        if (i == 5) {
        //      sleep(5);
        //      wait(NULL);    // 一次wait和waitpid调用，只能回收一个子进程，无差别回收
                wpid = waitpid(-1, NULL, WNOHANG);
                if (wpid == -1) {
                        perror("waitpid error\n");
                        exit(1);
                }
                printf("I'm parent, with a child finish:%d\n", wpid);
        } else {
                sleep(i);
                printf("I'm %dth child\n", i + 1);
        }

        return 0;
}
```

执行

![image-20211129205301377](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129205301377-1640859252750.png)

父进程休眠，等待子进程结束，回收，即放开注释的sleep(5)，打印pid。

![image-20211129205925162](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129205925162-1640859259840.png)

好像总是回收第一个?

指定某一个进程回收。

**(1)阻塞回收**

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        int i;
        pid_t pid, wpid, tpid;
        for (i = 0; i < 5; i++) {
                pid = fork();
                if (pid == 0) {   // 循环期间，子进程不fork
                        break;
                }
                if (i == 2) {            // 父进程执行
                        tpid = pid;      // 保存子进程pid
                        printf("----------pid = %d\n", tpid);
                }
        }
        if (i == 5) {
        //      sleep(5);
        //      wait(NULL);    // 一次wait和waitpid调用，只能回收一个子进程，无差别回收
                printf("-----parent, before waitpid pid = %d\n", tpid);
                wpid = waitpid(tpid, NULL, 0);    // 指定一个进程回收, 阻塞回收  
                if (wpid == -1) {
                        perror("waitpid error\n");
                        exit(1);
                }
                printf("I'm parent, with a child finish:%d\n", wpid);
        } else {
                sleep(i);
                printf("I'm %dth child, pid = %d\n", i + 1, getpid());
        }

        return 0;
}
```

执行

![image-20211129210815125](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129210815125-1640859272365.png)

这里会和中断提示符混在一起，并且不会结束。

**(2)非阻塞+时延：这样终端提示符就不会混在输出里。**

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        int i;
        pid_t pid, wpid, tpid;
        for (i = 0; i < 5; i++) {
                pid = fork();
                if (pid == 0) {   // 循环期间，子进程不fork
                        break;
                }
                if (i == 2) {            // 父进程执行
                        tpid = pid;      // 保存子进程pid
                        printf("----------pid = %d\n", tpid);
                }
        }
        if (i == 5) {
                sleep(5);
        //      wait(NULL);    // 一次wait和waitpid调用，只能回收一个子进程，无差别回收
                printf("-----parent, before waitpid pid = %d\n", tpid);
                wpid = waitpid(tpid, NULL, WNOHANG);  // 指定一个进程回收, 不阻塞  
                if (wpid == -1) {
                        perror("waitpid error\n");
                        exit(1);
                }
                printf("I'm parent, with a child finish:%d\n", wpid);
        } else {
                sleep(i);
                printf("I'm %dth child, pid = %d\n", i + 1, getpid());
        }

        return 0;
}
```

执行

![image-20211129210103189](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129210103189-1640859284687.png)

##### 2.7.3 waitpid回收多个子进程

  wait、waitpid 一次调用，回收一个子进程。

   想回收多个。while

```c
/*************************************************************************
 > File Name: fork.c
 > Author: Winter
 > Created Time: 2021年10月09日 星期六 19时42分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/wait.h>
int main(int argc, char* argv[])
{
        int i;
        pid_t pid, wpid;
        for (i = 0; i < 5; i++) {
                pid = fork();
                if (pid == 0) {   // 循环期间，子进程不fork
                        break;
                }
        }
        if (i == 5) {
/*                // 使用阻塞方回收子进程
                while((wpid = waitpid(-1, NULL, 0))) {
                        printf("wait child %d\n",wpid);
                }*/
                // 非阻塞
                while((wpid = waitpid(-1, NULL,WNOHANG)) != -1) {
                        if (wpid > 0) {
                                printf("wait child %d\n",wpid);
                        } else if (wpid == 0){
                                sleep(1);
                                continue;
                        }
                }
        } else {
                sleep(i);
                printf("I'm %dth child, pid = %d\n", i + 1, getpid());
        }

        return 0;
}
```

​	执行

![image-20211129211437137](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211129211437137-1640859365614.png)

2.7.4 暴力回收子进程

```
kill -9 父进程号
```

## 五、进程间通信

### 1 常见方式

IPC(InterProcess Communication)进程间通信。

![image-20211130190735405](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130190735405-1640859372598.png)

进程间通信的常用方式，特征：

```
管道：简单
信号：开销小
mmap映射：非血缘关系进程间
socket（本地套接字）：稳定
```

### 2 管道

![image-20211130190955677](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130190955677-1640859381221.png)

管道：实现原理： 内核借助环形队列机制，使用内核缓冲区实现。

  特质； 

```
1. 伪文件
2. 管道中的数据只能一次读取。
3. 数据在管道中，只能单向流动。
```

  局限性：

```
1. 自己写，不能自己读。
2. 数据不可以反复读。
3. 半双工通信。
4. 血缘关系进程间可用。
```

#### 2.1 管道的基本用法

pipe函数

```c
	// pipe函数：	创建，并打开管道。
	int pipe(int fd[2]);
	// 参数：	fd[0]: 读端。
	//        fd[1]: 写端。
	// 返回值： 成功： 0
	//        失败： -1 errno
```

管道通信原理

![image-20211130191646809](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130191646809-1640859391845.png)

创建一个管道，可读可写，再创建子进程，同样可读可写。

![image-20211130191859369](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130191859369-1640859397869.png)

关闭父进程从管道的读操作，关闭子进程对管道的操作，即对管道的操作是单向的。

一个管道通信的示例，**父进程往管道里写，子进程从管道读**，然后打印读取的内容：

```c
/*************************************************************************
 > File Name: pipe.c
 > Author: Winter
 > Created Time: 2021年10月14日 星期四 21时18分54秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<error.h>

int main(int argc, char* argv[])
{
        int res;
        int fd[2];
        char* str = "hello pipe\n";
        char buff[1024];

        res = pipe(fd);
        if (res == -1) {
                perror("pipe error\n");
                exit(1);
        }
        // 有管道了,父进程写，子进程读
        pid_t pid = fork();
        if (pid > 0 ){
                // 父进程
                close(fd[0]);   // 关闭读端
                write(fd[1], str, strlen(str));
                sleep(3);
                close(fd[1]);
        } else if (pid == 0) {
                // 子进程
                close(fd[1]);  // 关闭写端
                res = read(fd[0], buff, sizeof(buff));
                printf("child read res = %d\n",res);
                write(STDOUT_FILENO, buff, res);
                close(fd[0]);
        } else {
                perror("fork error");
                exit(1);
        }
        return 0;
}
```

执行

![image-20211130192256676](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130192256676-1640859413519.png)

#### 2.2 管道读写行为

读管道

```
读管道：
		1. 管道有数据，read返回实际读到的字节数。
		2. 管道无数据：	1）无写端，read返回0 （类似读到文件尾）
									2）有写端，read阻塞等待。
```

写管道

```
	1. 无读端， 异常终止。 （SIGPIPE导致的）
	2. 有读端：	1） 管道已满， 阻塞等待
						 2） 管道未满， 返回写出的字节个数。
```

#### 2.3 父子进程通信练习

使用管道实现父子进程间通信，完成：ls | wc -l 假定父进程实现ls，子进程实现wc ls命令正常会将结果集写到stdout，但现在会写入管道      

写端wc -l命令正常应该从stdin读取数据，但此时会从管道的读端读。

```c
/*************************************************************************
 > File Name: pipe.c
 > Author: Winter
 > Created Time: 2021年10月14日 星期四 21时18分54秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<error.h>
#include<fcntl.h>
void sys_err(const char* str) {
        perror(str);
        exit(1);
}

int main(int argc, char* argv[])
{
        pid_t pid;    // 进程用
        int res = 0;  // 返回结果
        int fd[2];    // 管道读写

        // 创建管道
        if (pipe(fd) == -1) {
                sys_err("pipe error\n");
        }

        pid = fork();
        if (pid < 0) {
                sys_err("fork error\n");
        } else if (pid > 0) {
                // 父进程执行
                close(fd[1]);      					// 关闭读端
                dup2(fd[0], STDIN_FILENO);   // 将 参数2 重定向 参数1
                execlp("wc", "wc", "-l",  NULL);
                sys_err("execlp wc error\n");
        } else {
                close(fd[0]);
                dup2(fd[1], STDOUT_FILENO);
                execlp("ls", "ls", NULL);
                sys_err("execlp error\n");
        }

        return 0;
}
```

执行

![image-20211130193928832](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130193928832-1640859431268.png)

#### 2.4 兄弟间进程通信

练习题：兄弟进程间通信

兄：ls

弟：wc -l

父：等待回收子进程

**要求，使用循环创建N个子进程模型创建兄弟进程，使用循环因子i标识，注意管道读写行为**

```c
/*************************************************************************
 > File Name: pipe.c
 > Author: Winter
 > Created Time: 2021年10月14日 星期四 21时18分54秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<error.h>
#include<sys/wait.h>
#include<fcntl.h>
void sys_err(const char* str) {
        perror(str);
        exit(1);
}

int main(int argc, char* argv[])
{
        pid_t pid;    // 进程用
        int res = 0;  // 返回结果
        int fd[2];    // 管道读写
        int i;
        // 创建管道
        if (pipe(fd) == -1) {
                sys_err("pipe error\n");
        }
        // 循环创建2个子进程
        for (i = 0; i < 2; i++) {
                pid = fork();    // 创建进程
                if (pid < 0) {
                        sys_err("fork error\n");
                }
                // 子进程退出
                if (pid == 0) {
                        break;
                }
        }
        // 用i来标识子进程
        // 父进程
        if (i == 2) {
                // 父进程不使用进程，关掉
                close(fd[0]);
                close(fd[1]);
                wait(NULL);
                wait(NULL);
        } else if (i == 0) {
                // 兄进程
                close(fd[0]);
                dup2(fd[1], STDOUT_FILENO);
                execlp("ls", "ls", NULL);
                sys_err("execlp error\n");
        } else if (i == 1) {
                // 弟进程
                close(fd[1]);      // 关闭读端
                dup2(fd[0], STDIN_FILENO);   // 将 参数2 重定向 参数1
                execlp("wc", "wc", "-l",  NULL);
                sys_err("execlp wc error\n");
        }
        return 0;
}
```

执行

![image-20211130194201245](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130194201245-1640859454705.png)

测试：

```
是否允许，一个pipe有一个写端多个读端   可以
是否允许，一个pipe有多个写端一个读端   可以
```

管道默认大小4096

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130194416014-1640859461546.png" alt="image-20211130194416014" style="zoom:80%;" />

#### 2.5 多个写端操作管道

**允许pipe中有一个写端，多个读端。**

下面是一个父进程读，俩子进程写的例子，也就是一个读端多个写端。需要调控写入顺序才行。

```c
/*************************************************************************
 > File Name: pipe.c
 > Author: Winter
 > Created Time: 2021年10月14日 星期四 21时18分54秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<error.h>
#include<sys/wait.h>
#include<fcntl.h>
void sys_err(const char* str) {
        perror(str);
        exit(1);
}

int main(int argc, char* argv[])
{
        pid_t pid;    // 进程用
        int res = 0;  // 返回结果
        int fd[2];    // 管道读写
        int i;
        char* buff[1024];
        // 创建管道
        if (pipe(fd) == -1) {
                sys_err("pipe error\n");
        }
        // 循环创建2个子进程
        for (i = 0; i < 2; i++) {
                pid = fork();    // 创建进程
                if (pid < 0) {
                        sys_err("fork error\n");
                }
                // 子进程退出
                if (pid == 0) {
                        break;
                }
        }
        // 用i来标识子进程
        // 父进程
        if (i == 2) {
                // 父进程关闭写端,保留读端
                close(fd[1]);
                sleep(1);
                int n = 0;
                n = read(fd[0], buff, 1024);
                write(STDOUT_FILENO, buff, n);

                for (int k = 0; k < 2; k++) {
                        wait(NULL);
                }
        } else if (i == 0) {
                // 兄进程
                close(fd[0]);     // 关闭读端
                write(fd[1], "0 hello\n", strlen("0 hello\n"));
        } else if (i == 1) {
                // 弟进程
                close(fd[0]);      // 关闭读端
                write(fd[1], "1 hello\n", strlen("1 hello\n"));
        }
        return 0;
}
```

执行

![image-20211130194759300](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130194759300-1640859479871.png)

### 3 fifo

管道的特点：

```
  优点：简单，相比信号，套接字实现进程通信，简单很多。
  缺点：1.只能单向通信，双向通信需建立两个管道
    		2.只能用于有血缘关系（父子，兄弟）的进程间通信。该问题后来使用fifo命名管道解决。
```

![image-20211130195402408](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130195402408-1640859488432.png)

**fifo管道：可以用于无血缘关系的进程间通信。**

命名管道： mkfifo

![image-20211130195451158](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130195451158-1640859495879.png)

无血缘关系进程间通信：

```
读端，open fifo O_RDONLY
写端，open fifo O_WRONLY
```

fifo操作起来像文件

下面的代码创建一个fifo：

```c
/*************************************************************************
 > File Name: testfifl.c
 > Author: Winter
 > Created Time: 2021年10月17日 星期日 21时02分59秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/stat.h>
int main(int argc, char* argv[])
{
        int res = mkfifo("myTestFifo", 0664);
        if (res == -1) {
                perror("mkfifo error\n");
        }

        return 0;
}
```

执行

![image-20211130195935816](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130195935816-1640859505636.png)

如图，管道就通过程序创建出来了。

#### 3.1 fifo实现非血缘关系进程间通信

下面这个例子，一个写fifo，一个读fifo，操作起来就像文件一样的：

fifo_w.c

```c
/**************************************************************************
 > File Name: fifo_w.c
 > Author: Winter
 > Created Time: 2021年10月17日 星期日 21时14分49秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd;
        char buff[4096];
        if (argc < 2) {
                printf("Enter like this: ./a.out fifoname\n");
        }

        fd = open(argv[1], O_WRONLY);
        if (fd < 0) {
                perror("fifo error\n");
                exit(1);
        }

        int i = 0;
        while (1) {
                sprintf(buff, "hello  %d\n",i++);

                write(fd, buff, strlen(buff));
                sleep(1);
        }
        close(fd);
        return 0;
}
```

fifo_r.c

```c
/*************************************************************************
 > File Name: fifo_r.c
 > Author: Winter
 > Created Time: 2021年10月17日 星期日 21时19分15秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
int main(int argc, char* argv[])
{
        int fd, len;
        char buff[4096];
        if (argc < 2) {
                printf("Enter like this: ./a.out fifoname\n");
        }
        fd = open(argv[1], O_RDONLY);
        if (fd < 0) {
                perror("fifo error\n");
                exit(1);
        }

        while (1) {
                len = read(fd, buff, sizeof(buff));
                write(STDOUT_FILENO, buff, len);
                sleep(1);
        }
        close(fd);
        return 0;
}
```

如图

![image-20211130200311464](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130200311464-1640859525458.png)

编译执行，如图：

![image-20211130200514630](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130200514630-1640859531701.png)

测试一个写端多个读端的时候，由于数据一旦被读走就没了，所以多个读端的并集才是写端的写入数据。

#### 3.2 文件用于进程间通信

  打开的文件是内核中的一块缓冲区。多个无血缘关系的进程，可以同时访问该文件。

![image-20211130200653357](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130200653357-1640859539740.png)

**文件通信这个，有没有血缘关系都行，只是有血缘关系的进程对于同一个文件，使用的同一个文件描述符，没有血缘关系的进程，对同**

**一个文件使用的文件描述符可能不同。这些都不是问题，打开的是同一个文件就行。**

### 4 mmap

#### 4.1 函数原型

存储映射I/O(Memory-mapped I/O) 使一个磁盘文件与存储空间中的一个缓冲区相映射。于是从缓冲区中取数据，就相当于读文件中的相

应字节。与此类似，将数据存入缓冲区，则相应的字节就自动写入文件。这样，就可在不使用read和write函数的情况下，使地址指针完

成I/O操作。

使用这种方法，首先应该通知内核，将一个指定文件映射到存储区域中。这个映射工作可以通过mmap函数来实现。

![image-20211130200920472](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130200920472-1640859547716.png)

**mmap函数原型**

```c
void *mmap(void *addr, size_t length, int prot, int flags, int fd, off_t offset);		 // 创建共享内存映射
	// 参数：
	//		addr：	指定映射区的首地址。通常传NULL，表示让系统自动分配
	//  	length：共享内存映射区的大小。（<= 文件的实际大小）
	// 		prot：		共享内存映射区的读写属性。PROT_READ、PROT_WRITE、PROT_READ|PROT_WRITE
	//   	flags：	标注共享内存的共享属性。MAP_SHARED、MAP_PRIVATE
	//  	fd:	用于创建共享内存映射区的那个文件的 文件描述符。
	//  	offset：默认0，表示映射文件全部。偏移位置。需是 4k 的整数倍。
	// 返回值：
	// 		成功：映射区的首地址。
	// 		失败：MAP_FAILED (void*(-1))， errno

	//	flags里面的shared意思是修改会反映到磁盘上，private表示修改不反映到磁盘上
```

**munmap函数原型**

```c
int munmap(void *addr, size_t length);		// 释放映射区。
	// addr：mmap 的返回值
	// length：大小
```

#### 4.2 mmap建立映射区

下面这个示例代码，使用mmap创建一个映射区（共享内存），并往映射区里写入内容：

```c
/*************************************************************************
 > File Name: mmap.c
 > Author: Winter
 > Created Time: 2021年10月18日 星期一 20时30分00秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<fcntl.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/mman.h>
int main(int argc, char* argv[])
{
        char* p = NULL;

        // 打开一个文件
        int fd = open("testmap", O_RDWR|O_CREAT|O_TRUNC, 0644);
        if (fd == -1) {
                perror("open error\n");
                exit(1);
        }
/*
        // 此时上面文件大小为0，拓展文件大小
        lseek(fd, 10, SEEK_END);
        write(fd, "1", 1);     // 文件大小11

        上面这两个函数等于ftruncate()函数
*/

        ftruncate(fd, 20);
        int len = lseek(fd, 0, SEEK_END);   // 有IO操作
  	
  			// mmap在这里
        p = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
        if (p == MAP_FAILED) {
                perror("mmap error\n");
                exit(1);
        }
        // 使用p对文件进行读写操作
        strcpy(p, "hello mmap");

        // 读操作
        printf("-----%s\n",p);

        // 释放内存
        int res = munmap(p, len);
        if (res == -1) {
                perror("munmap error\n");
                exit(1);
        }
        return 0;
}
```

执行

![image-20211130201921282](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130201921282-1640859580806.png)

查看testmap

```shell
od -tcx testmap
```

![image-20211130202211326](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130202211326-1640859589662.png)

#### 4.3 mmap使用注意事项

使用注意事项：

**1 用于创建映射区的文件大小为 0，实际指定非0大小创建映射区，出 “总线错误”。**

```c
int len = 20; 
p = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
```

![image-20211130202452110](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130202452110-1640859598084.png)

**2 用于创建映射区的文件大小为 0，实际指定0大小创建映射区， 出 “无效参数”。**

```c
int len = 0; 
p = mmap(NULL, len, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
```

 **3 用于创建映射区的文件读写属性为，只读（只写）。映射区属性为 读、写。 出 “无效参数（权限不允许）”。**

![image-20211130202620640](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130202620640-1640859604133.png)

4 创建映射区，需要read权限。当访问权限指定为 “共享”MAP_SHARED时， mmap的读写权限，应该 <=文件的open权限。 只写不]

行。两个都是只读【段错误】

5 文件描述符fd，在mmap创建映射区完成即可关闭。后续访问文件，用 地址访问。

6 offset 必须是 4096的整数倍。（MMU 映射的最小单位 4k ）

7 对申请的映射区内存，不能越界访问。 

8 **munmap用于释放的 地址，必须是mmap申请返回的地址。**

9 映射区访问权限为 “私有”MAP_PRIVATE, 对内存所做的所有修改，只在内存有效，不会反应到物理磁盘上。

10 映射区访问权限为 “私有”MAP_PRIVATE, 只需要open文件时，有读权限，用于创建映射区即可。

**mmap函数的保险调用方式：**

```c
fd = open("文件名"， O_RDWR);
mmap(NULL, 有效文件大小， PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
```

总结

1. 创建映射区的过程中，隐含着一次对映射文件的读操作

2. 当MAP_SHARED时，要求：映射区的权限应该<=文件打开的权限（出于对映射区的保护）。而MAP_PRIVATE则无所谓，因为mmap中的权限是对内存的限制

3. 映射区的释放与文件关闭无关。只要映射建立成功，文件可以立即关闭

4. **特别注意，当映射文件大小为0时，不能创建映射区。所以：用于映射的文件必须要有实际大小！！**mmap使用时常常会出现总线错

   误，通常是由于共享文件存储空间大小引起的。如，400字节大小的文件，在建立映射区时，offset4096字节，则会报出总线错误

5. munmap传入的地址一定是mmap返回的地址。坚决杜绝指针++操作

6. 文件偏移量必须为4K的整数倍

7. mmap创建映射区出错概率非常高，一定要检查返回值，确保映射区建立成功再进行后续操作。

#### 4.4 父子进程间通信

父子进程使用 mmap 进程间通信：

```
父进程 先 创建映射区。 open（ O_RDWR） mmap( MAP_SHARED );
指定 MAP_SHARED 权限
fork() 创建子进程。
一个进程读， 另外一个进程写。
```

下面这段代码，父子进程mmap通信，共享内存是一个int变量：

```c
/*************************************************************************
 > File Name: fork_mmap.c
 > Author: Winter
 > Created Time: 2021年10月20日 星期三 21时07分53秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<sys/mman.h>
#include<sys/wait.h>
#include<fcntl.h>
int var = 10;
int main(int argc, char* argv[])
{
        int fd = open("temp", O_RDWR|O_CREAT|O_TRUNC, 0644);
        int* p = NULL;
        pid_t pid;
        if (fd < 0) {
                perror("open error\n");
                exit(1);
        }
        ftruncate(fd, 4);         // 将文件大小拓展为4

        p = (int*) mmap(NULL, 4, PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
        if (p == MAP_FAILED) {
                perror("mmap error\n");
                exit(1);
        }
        close(fd);               // 映射区创建完毕，关闭文件

        pid = fork();            // 创建子进程
        if (pid == 0) {
                // 子进程处理
                *p = 11000;       // 写共享内存
                var = 20;         // 修改全局变量
                printf("child *p = %d, var = %d\n",*p, var);
        } else if (pid > 0) {
                // 父进程处理
                sleep(1);
                printf("parent *p = %d, var = %d\n",*p, var);  // 读共享内存
                wait(NULL);     // 回收子进程
                // 回收
                int res = munmap(p, 4);
                if (res == -1) {
                        perror("munmmap error\n");
                        exit(1);
                }
        } else {
                perror("fork error\n");
                exit(1);
        }

        return 0;
}
```

执行【读时共享，写时复制】

![image-20211130203928033](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130203928033-1640859623069.png)

**如图，子进程修改p的值，也反映到了父进程上，这是因为共享内存定义为shared的。**

**如果将共享内存定义为private，运行结果如下**：

![image-20211130204044426](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130204044426-1640859629118.png)

父子进程使用mmap进程间通信。

父进程先创建映射区，O_RDWR，指定MAP_SHARED,fork创建子进程，一个读一个写。

#### 4.5 无血缘关系进程间mmap通信

两个进程 打开同一个文件，创建映射区。

指定flags 为 MAP_SHARED。

 一个进程写入，另外一个进程读出。

【注意】：**无血缘关系进程间通信。mmap：数据可以重复读取。**

 															 **fifo：数据只能一次读取。**

下面是两个无血缘关系的通信代码，先是写进程：

```c
/*************************************************************************
 > File Name: mmap_w.c
 > Author: Winter
 > Created Time: 2021年10月22日 星期五 20时43分01秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/mman.h>

struct Stu{
        int id;
        char name[20];
        char gender;
};

int main(int argc, char* argv[])
{
        struct Stu student = {1, "xiaoming", 'M'};
        struct Stu* p = NULL;
/*      if (argc < 2) {
                printf("please input a.out sharedFile......\n");
                exit(1);
        }*/
        int fd = open("stuFile", O_RDWR|O_CREAT|O_TRUNC, 0664);
        if (fd == -1) {
                perror("open error\n");
                exit(1);
        }
        // 扩大内存
        ftruncate(fd, sizeof(student));
        p = mmap(NULL, sizeof(student), PROT_READ|PROT_WRITE, MAP_SHARED, fd, 0);
        if (p == MAP_FAILED) {
                perror("mmap error\n");
                exit(1);
        }
        close(fd);

        while (1) {
                memcpy(p, &student, sizeof(student));
                student.id++;
                sleep(1);
        }
        munmap(p, sizeof(student));
        return 0;
}
```

然后是读进程：

```c
/*************************************************************************
 > File Name: mmap_w.c
 > Author: Winter
 > Created Time: 2021年10月22日 星期五 20时43分01秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/mman.h>

struct Stu{
        int id;
        char name[20];
        char gender;
};

int main(int argc, char* argv[])
{
        struct Stu student;
        struct Stu* p = NULL;
/*      if (argc < 2) {
                printf("please input a.out sharedFile......\n");
                exit(1);
        }
*/      int fd = open("stuFile", O_RDONLY);
        if (fd == -1) {
                perror("open error\n");
                exit(1);
        }

        p = mmap(NULL, sizeof(student), PROT_READ, MAP_SHARED, fd, 0);
        if (p == MAP_FAILED) {
                perror("mmap error\n");
                exit(1);
        }
        close(fd);

        while (1) {
                printf("id = %d, name = %s, gender = %c\n",p->id, p->name, p->gender);
                sleep(1);
        }
        munmap(p, sizeof(student));
        return 0;
}
```

执行

![image-20211130204611209](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130204611209-1640859650577.png)

多个写端一个读端也没问题，打开多个写进程即可，完事儿读进程会读到所有写进程写入的内容。

这里要注意一个，内容被读走之后不会消失，所以如果读进程的读取时间间隔短，它会读到很多重复内容，就是因为写进程没来得及写入

新内容。

#### 4.6 mmap匿名映射区

**匿名映射：只能用于 血缘关系进程间通信。**

```c
	p = (int *)mmap(NULL, 40, PROT_READ|PROT_WRITE, MAP_SHARED|MAP_ANONYMOUS, -1, 0);
```

![image-20211130204902753](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130204902753-1640859657570.png)

较老的系统 类unix

![image-20211130204918730](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211130204918730-1640859662553.png)

```
/dev/zero------->聚宝盆
/dev/null------->文件黑洞
```

## 六、信号

### 1 信号的概念

信号在我们的生活中随处可见， 如：古代战争中摔杯为号；现代战争中的信号弹；体育比赛中使用的信号枪......

**它们都有共性： 1. 简单 2. 不能携带大量信息 3. 满足某个特设条件才发送。**  

信号是信息的载体， Linux/UNIX 环境下，古老、经典的通信方式， 现下依然是主要的通信手段。  

Unix 早期版本就提供了信号机制，但不可靠，信号可能丢失。 Berkeley 和 AT&T 都对信号模型做了更改，增加

了可靠信号机制。但彼此不兼容。 POSIX.1 对可靠信号例程进行了标准化。  

### 2 信号的机制

A 给 B 发送信号， B 收到信号之前执行自己的代码，收到信号后，不管执行到程序的什么位置，都要暂停运行，

去处理信号，处理完毕再继续执行。与硬件中断类似——异步模式。但信号是软件层面上实现的中断，早期常被称

为“软中断”。  

信号的特质：由于信号是通过软件方法实现，其实现手段导致信号有很强的延时性。但对于用户来说，这个延

迟时间非常短，不易察觉。 

>  **每个进程收到的所有信号，都是由内核负责发送的，内核处理。**

### 3 与信号相关的事件和状态

产生信号:

```
1 按键产生，如： Ctrl+c、 Ctrl+z、 Ctrl+\
2 系统调用产生，如： kill、 raise、 abort
3 软件条件产生，如：定时器 alarm
4 硬件异常产生，如：非法访问内存(段错误)、除 0(浮点数例外)、内存对齐出错(总线错误)
5 命令产生，如： kill 命令  
```

**递达**：递送并且到达进程。

**未决**：产生和递达之间的状态。主要由于阻塞(屏蔽)导致该状态。  

信号的处理方式:

```
1 执行默认动作
2 忽略(丢弃)
3 捕捉(调用户处理函数)  
```

Linux 内核的进程控制块 PCB 是一个结构体， task_struct, 除了包含进程 id，状态，工作目录，用户 id，组 id，

文件描述符表，还包含了信号相关的信息，**主要指阻塞信号集和未决信号集**。  

**阻塞信号集(信号屏蔽字)**： 将某些信号加入集合，对他们设置屏蔽，当屏蔽 x 信号后，再收到该信号，该信号

的处理将推后(解除屏蔽后)  。

**未决信号集:**  

```
1 信号产生，未决信号集中描述该信号的位立刻翻转为 1，表信号处于未决状态。当信号被处理对应位翻转回为 0。这一时刻往往非常短暂。
2 信号产生后由于某些原因(主要是阻塞)不能抵达。这类信号的集合称之为未决信号集。在屏蔽解除前，信号一直处于未决状态。
```

![image-20211201192529670](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201192529670-1640859675047.png)

未决信号集：**本质：位图。用来记录信号的处理状态。该信号集中的信号，表示，已经产生，但尚未被处理。**

![image-20211201192649739](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201192649739-1640859681300.png)

### 4 信号四要素

```shell
kill -l     #查看当前系统中常规信号
```

![image-20211201192735808](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201192735808-1640859687064.png)

1-31:常规信号；34-64：实时信号。

**信号使用之前，应先确定其4要素，而后再用！！！**

```
（1）编号
（2）名称
（3）信号对应事件
（4）信号默认处理动作。
```

![image-20211201193031401](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201193031401-1640859692874.png)

- **默认动作**

1. Term：终止进程；
2. Ign： 忽略信号 (默认即时对该种信号忽略操作)；
3. Core：终止进程，生成 Core 文件。(查验进程死亡原因， 用于 gdb 调试)；
4. Stop：停止（暂停）进程；
5. Cont：继续运行进程。

### 5 Linux常规信号汇总

1、SIGHUP: 当用户退出 shell 时，由该 shell 启动的所有进程将收到这个信号，默认动作为终止进程；

2、**SIGINT**：当用户按下了<Ctrl+C>组合键时，用户终端向正在运行中的由该终端启动的程序发出此信号。默认动

作为终止进程；

3、SIGQUIT：当用户按下<ctrl+\\>组合键时产生该信号，用户终端向正在运行中的由该终端启动的程序发出些信

号。默认动作为终止进程;

4、SIGILL：CPU 检测到某进程执行了非法指令。默认动作为终止进程并产生 core 文件;

5、SIGTRAP：该信号由断点指令或其他 trap 指令产生。默认动作为终止里程 并产生 core 文件;

6、SIGABRT: 调用 abort 函数时产生该信号。默认动作为终止进程并产生 core 文件;

7、SIGBUS：非法访问内存地址，包括内存对齐出错，默认动作为终止进程并产生 core 文件;

8、SIGFPE：在发生致命的运算错误时发出。不仅包括浮点运算错误，还包括溢出及除数为 0 等所有的算法错误。

默认动作为终止进程并产生 core 文件;

9、**SIGKILL**：无条件终止进程。本信号不能被忽略，处理和阻塞。默认动作为终止进程。它向系统管理员提供了

可以杀死任何进程的方法;

10、SIGUSE1：用户定义 的信号。即程序员可以在程序中定义并使用该信号。默认动作为终止进程;

11、SIGSEGV：指示进程进行了无效内存访问。默认动作为终止进程并产生 core 文件;

12、SIGUSR2：另外一个用户自定义信号，程序员可以在程序中定义并使用该信号。默认动作为终止进程;

13、SIGPIPE：Broken pipe 向一个没有读端的管道写数据。默认动作为终止进程;

14、SIGALRM: 定时器超时，超时的时间 由系统调用 alarm 设置。默认动作为终止进程;

15、SIGTERM：程序结束信号，与 SIGKILL 不同的是，该信号可以被阻塞和终止。通常用来要示程序正常退出;

执行 shell 命令 Kill 时，缺省产生这个信号。默认动作为终止进程;

16、SIGSTKFLT：Linux 早期版本出现的信号，现仍保留向后兼容。默认动作为终止进程;

17、**SIGCHLD**：子进程状态发生变化时，父进程会收到这个信号。默认动作为忽略这个信号;

18、SIGCONT：如果进程已停止，则使其继续运行。默认动作为继续/忽略;

19、**SIGSTOP**：停止进程的执行。信号不能被忽略，处理和阻塞。默认动作为暂停进程;

20、**SIGTSTP**：停止终端交互进程的运行。按下<ctrl+z>组合键时发出这个信号。默认动作为暂停进程;

21、SIGTTIN：后台进程读终端控制台。默认动作为暂停进程;

22、SIGTTOU: 该信号类似于 SIGTTIN，在后台进程要向终端输出数据时发生。默认动作为暂停进程;

23、SIGURG：套接字上有紧急数据时，向当前正在运行的进程发出些信号，报告有紧急数据到达。如网络带外

数据到达，默认动作为忽略该信号;

24、SIGXCPU：进程执行时间超过了分配给该进程的 CPU 时间 ，系统产生该信号并发送给该进程。默认动作为

终止进程;

25、SIGXFSZ：超过文件的最大长度设置。默认动作为终止进程;

26、SIGVTALRM：虚拟时钟超时时产生该信号。类似于 SIGALRM，但是该信号只计算该进程占用 CPU 的使用时

间。默认动作为终止进程;

27、SGIPROF：类似于 SIGVTALRM，它不公包括该进程占用 CPU 时间还包括执行系统调用时间。默认动作为终

止进程;

28、SIGWINCH：窗口变化大小时发出。默认动作为忽略该信号;

29、SIGIO：此信号向进程指示发出了一个异步 IO 事件。默认动作为忽略;

30、SIGPWR：关机。默认动作为终止进程;

31、SIGSYS：无效的系统调用。默认动作为终止进程并产生 core 文件;

32、SIGRTMIN ～ (64) SIGRTMAX：LINUX 的实时信号，它们没有固定的含义（可以由用户自定义）。所有的实

时信号的默认动作都为终止进程。

### 6 信号的产生

#### 6.1 终端按键产生信号

1. Ctrl + c → 2) SIGINT（终止/中断） “INT” ----Interrupt
2. Ctrl + z → 20) SIGTSTP（暂停/停止） “T” ----Terminal 终端。
3. Ctrl + \ → 3) SIGQUIT（退出）

#### 6.2 硬件异常产生信号

1. 除 0 操作 → 8) SIGFPE (浮点数例外) “F” -----float 浮点数。
2. 非法访问内存 → 11) SIGSEGV (段错误)。总线错误 → 7) SIGBUS

#### 6.3 kill 函数/命令产生信号

##### 6.3.1 函数原型

```c
int kill(pid_t pid, int sig); 
		// 成功：0；失败：-1 (ID 非法，信号非法，普通用户杀 init 进程等权级问题)，设置 errno
		// pid > 0: 发送信号给指定的进程。
		// pid = 0: 发送信号给 与调用 kill 函数进程属于同一进程组的所有进程。
		// pid < 0: 取|pid|发给对应进程组。
		// pid = -1：发送给进程有权限发送的系统中所有进程。
		// sig：不推荐直接使用数字，应使用宏名，因为不同操作系统信号编号可能不同，但名称一致
```

##### 6.3.2 子进程kill父进程

```c
/*************************************************************************
 > File Name: kill.c
 > Author: Winter
 > Created Time: 2021年10月25日 星期一 20时54分24秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>
int main(int argc, char* argv[])
{
        pid_t pid = fork();
        if (pid > 0) {
                // 父进程
                printf("parent, pid = %d\n",getpid());
                while (1);
        } else if (pid == 0) {
                // 子进程
                printf("child, pid = %d, ppid = %d\n", getpid(), getppid());
                kill(getppid(), SIGKILL);
//              kill(getppid(), SIGSEGV);
        } else {
                perror("fork error\n");
        }

        return 0;
}
```

执行

![image-20211201195139832](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201195139832-1640859710013.png)

##### 6.3.3 kill 命令

```shell
kill -SIGKILL pid     # kill 命令产生信号
kill -9 -groupname    # 杀一个进程组
```

#### 6.4 软件条件产生信号

##### 6.4.1 alarm函数原型

设置定时器(闹钟)。在指定 seconds 后，内核会给当前进程发送 14）SIGALRM 信号。进程收到该信号，默认动作终止。

**每个进程都有且只有唯一个定时器**

```c
unsigned int alarm(unsigned int seconds);  // 返回 0 或剩余的秒数，无失败。
																					 // 常用：取消定时器 alarm(0)，返回旧闹钟余下秒数。
```

 time 命令 ： 查看程序执行时间。  

**实际时间 = 用户时间 + 内核时间 + 等待时间。 --》 优化瓶颈 IO**

```c
/*************************************************************************
 > File Name: alarm.c
 > Author: Winter
 > Created Time: 2021年10月29日 星期五 19时30分27秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        int i;
        alarm(1);
        for (i = 0; ; i++) {
                printf("%d\n",i);
        }
        return 0;
}
```

执行

```shell
./alarm
```

![image-20211201200159551](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201200159551-1640859719567.png)

即计算机1s钟可以数多少个数。

##### 6.4.2 使用 time 命令查看程序执行的时间（优化）

执行

```shell
time ./alarm
```

结果

![image-20211201200531318](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201200531318-1640859725188.png)

将结果重定向到out中

```shell
time ./test > out
```

执行

(将结果输入到out) 明显提高性能

![image-20211201201934334](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201201934334-1640859730563.png)

结论：程序运行的瓶颈在于 IO，优化程序，首选优化 IO。

##### 6.4.3 setitimer函数原型

函数原型

![image-20211201201134985](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201201134985-1640859735387.png)

setitimer函数

```c
int setitimer(int which, 
							const struct itimerval *new_value, 
							struct itimerval *old_value);
	// 返回值：
	// 成功： 0
	// 失败： -1 errno
```

参数1：which

```c
which：	ITIMER_REAL：       // 采用自然计时。 ——> SIGALRM
				 ITIMER_VIRTUAL:    // 采用用户空间计时（进程占CPU时间）  ---> SIGVTALRM
				 ITIMER_PROF:       // 采用内核+用户空间计时 ---> SIGPROF
```

参数2：new_value：结构体设置间隔时间和单次时间

```c
new_value：    // 定时秒数
		          // 类型：
  						struct itimerval {
               				struct timeval {
               					time_t      tv_sec;         /* seconds */
               					suseconds_t tv_usec;        /* microseconds */
           				}it_interval;                    // 周期定时秒数
                
               				 struct timeval {
               					time_t      tv_sec;         
               					suseconds_t tv_usec;        
           				}it_value;                        // 第一次定时秒数  
           			 };
```

参数3：old_value：**传出参数，上次定时剩余时间。**

```c
e.g.:
			struct itimerval new_t;	
			struct itimerval old_t;	

			new_t.it_interval.tv_sec = 0;
			new_t.it_interval.tv_usec = 0;
			new_t.it_value.tv_sec = 1;
			new_t.it_value.tv_usec = 0;

			int ret = setitimer(&new_t, &old_t);  定时1秒
```

使用setitimer定时，向屏幕打印信息：

```c
/*************************************************************************
 > File Name: setitimer.c
 > Author: Winter
 > Created Time: 2021年10月29日 星期五 19时56分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>
#include<sys/time.h>
void myFun(int signo) {
        printf("hello world\n");
}

int main(int argc, char* argv[])
{
        struct itimerval it, oldit;
        signal(SIGALRM, myFun);      // 注册SIGALRM信号的捕捉处理函数

        it.it_value.tv_sec = 2;      // 第一次打印信息是间隔2s
        it.it_value.tv_usec = 0;

        it.it_interval.tv_sec = 5;    // 之后都是每隔5s打印一次
        it.it_interval.tv_usec = 0;

        if (setitimer(ITIMER_REAL, &it, &oldit) == -1) {
                perror("setitimer error\n");
                exit(1);
        }
        while (1);
        return 0;
}
```

执行

![image-20211201202251334](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201202251334-1640859747741.png)

第一次信息打印是两秒间隔，之后都是5秒间隔打印一次。可以理解为第一次是有个定时器，什么时候触发打印，之后就是间隔时间。

实现alarm

```c
/*************************************************************************
 > File Name: setitimer.c
 > Author: Winter
 > Created Time: 2021年10月29日 星期五 19时56分57秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>
#include<sys/time.h>
unsigned int  myAlarm(unsigned int sec) {
        struct itimerval it, oldit;

        it.it_value.tv_sec = sec;
        it.it_value.tv_usec = 0;

        it.it_interval.tv_sec = 0;
        it.it_interval.tv_usec = 0;

        if (setitimer(ITIMER_REAL, &it, &oldit) == -1) {
                perror("setitimer error\n");
                exit(1);
        }
        return oldit.it_value.tv_sec;
}

int main(int argc, char* argv[])
{
        myAlarm(1);
        int i;
        for (i = 0; ;i ++) {
                printf("%d\n",i);
        }

        return 0;
}
```

执行

![image-20211201202455373](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201202455373-1640859756647.png)

### 7 信号集操作函数+原理（重）

**控制原理**：内核通过读取未决信号集来判断信号是否应被处理。信号屏蔽字 mask 可以影响未决信号集。

而我们可以在应用程序中自定义 set 来改变 mask。已达到屏蔽指定信号的目的。（重点）

![image-20211201203248784](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201203248784-1640859761902.png)

阻塞信号集、未决信号集默认0

信号集操作函数：

```c
sigset_t set; 															 	// 自定义信号集。typedef unsigned long sigset_t;
sigemptyset(sigset_t *set);									 	// 将某个信号集清 0 成功：0；失败：-1
sigfillset(sigset_t *set);	                 	// 将某个信号集置 1 成功：0；失败：-1
sigaddset(sigset_t *set, int signum);	       	// 将某个信号加入信号集 成功：0；失败：-1
sigdelset(sigset_t *set, int signum);				 	// 将某个信号清出信号集 成功：0；失败：-1
sigismember（const sigset_t *set，int signum); // 判断某个信号是否在信号集中 返回值：在集合：1；不在：0；出错：-1
```

#### 7.1 sigprocmask 函数

用来**屏蔽信号**、**解除屏蔽**也使用该函数。其本质，读取或修改进程的信号屏蔽字(PCB 中)

```c
int sigprocmask(int how, const sigset_t *set, sigset_t *oldset);
		// how:	SIG_BLOCK:		设置阻塞
		//		 	SIG_UNBLOCK:	取消阻塞
		// 			SIG_SETMASK:	用自定义set替换mask。
		// set：	自定义set
		// oldset：旧有的 mask。 
```

#### 7.2 sigpending 函数

**查看未决信号集**

```c
int sigpending(sigset_t *set);
		// set： 传出的 未决信号集。
```

#### 7.3 信号操作函数使用原理分析

上来全部是0。

**自己写的set**

**信号屏蔽字mask**

![image-20211201204711973](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201204711973-1640859770851.png)

用到的函数

```c
sigset_t set, mySet;
sigemptyset(&set);
sigaddset(&set, SIGINT)
sigprocmask(SIG_BLOCK, &set);
sigpending(&mySet);
```

信号列表：

![image-20211201205003200](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201205003200-1640859777284.png)

**其中9号和19号信号比较特殊，只能执行默认动作，不能忽略捕捉，不能设置阻塞。**

下面这个小例子，利用自定义集合，来设置信号阻塞，我们输入被设置阻塞的信号，可以看到未决信号集发生变化：

```c
/*************************************************************************
 > File Name: sigfunc.c
 > Author: Winter
 > Created Time: 2021年11月01日 星期一 20时42分54秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>

void printSet(sigset_t* set) {
        int i;
        for (i = 1; i < 32; i++) {
                if(sigismember(set, i)) {
                        putchar('1');
                } else {
                        putchar('0');
                }
        }
        printf("\n");
}

int main(int argc, char* argv[])
{
        // 自定义信号集
        sigset_t set, oldSet, pedSet;
        int res = 0;
        sigemptyset(&set);          // 情况信号集
        sigaddset(&set, SIGINT);    // ctrl + c
        sigaddset(&set, SIGQUIT);   // ctrl + /
        sigaddset(&set, SIGBUS);
        sigaddset(&set, SIGKILL);   // kill只能默认
        res = sigprocmask(SIG_BLOCK, &set, &oldSet);   // 设置到阻塞信号集里
        if (res == -1) {
                perror("sigprocmask error\n");
        }
        while (1) {
                // 查看未决信号集
                res = sigpending(&pedSet);
                if (res == -1) {
                        perror("sigpending error\n");
                }
                printSet(&pedSet);
                sleep(1);
        }
        return 0;
}
```

执行

![image-20211201205147161](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201205147161-1640859787870.png)

可以看到，在输入Ctrl+C之后，进程捕捉到信号，但由于设置阻塞，没有处理，未决信号集对应位置变为1.

**其中9号和19号信号比较特殊，只能执行默认动作，不能忽略捕捉，不能设置阻塞。**

![image-20211201205208572](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201205208572-1640859793588.png)

### 8 信号捕捉

#### 8.1 signal 函数

**作用**：注册一个信号捕捉函数（注册而非创建）
**原型**：

```c
sighandler_t signal(int signum, sighandler_t handler);
	// signum ：待捕捉信号
	// handler：捕捉信号后的操纵函数
typedef void (*sighandler_t)(int);
	// 函数指针类型 需要注意的就是需要传入一个整形参数 不管用不用
```

返回值：

![image-20211201205457050](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201205457050-1640859801226.png)

信号捕捉特性：

```
1 捕捉函数执行期间，信号屏蔽字 由 mask --> sa_mask , 捕捉函数执行结束。 恢复回mask
2 捕捉函数执行期间，本信号自动被屏蔽(sa_flgs = 0).
3 捕捉函数执行期间，被屏蔽信号多次发送，解除屏蔽后只处理一次！
```

一个信号捕捉的小例子

```c
/*************************************************************************
 > File Name: signal.c
 > Author: Winter
 > Created Time: 2021年11月06日 星期六 16时05分52秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>
void signalCatch(int signum) {
        printf("catch you %d\n",signum);
}

int main(int argc, char* argv[])
{
        // 注册信号捕捉函数, 当信号SIGINT触发时，调用signalCatch
        signal(SIGINT, signalCatch);

        while(1);
        return 0;
}
```

执行

![image-20211201205841751](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201205841751-1640859810628.png)

#### 8.2 sigaction实现信号捕捉

sigaction也是**注册**一个信号捕捉函数

```c
int sigaction(int signum, const struct sigaction *act,
                     struct sigaction *oldact);
```

结构体

```c
 struct sigaction {
               void     (*sa_handler)(int);
               void     (*sa_sigaction)(int, siginfo_t *, void *);
               sigset_t   sa_mask;
               int        sa_flags;
               void     (*sa_restorer)(void);
           };
```

返回值

![image-20211201210145872](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201210145872-1640859817920.png)

下面的小例子，使用sigaction捕捉两个信号：

```c
/*************************************************************************
 > File Name: signal.c
 > Author: Winter
 > Created Time: 2021年11月06日 星期六 16时05分52秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>

// 捕捉函数，回调函数【内核调】
void signalCatch(int signum) {
        if (signum == SIGINT) {
                printf("catch you %d\n",signum);
                sleep(10);
        } else if (signum == SIGQUIT) {
                printf("--------catch you %d---------\n",signum);
        }
}

int main(int argc, char* argv[])
{
        struct sigaction act, oldact;

        act.sa_handler = signalCatch;    // 设置回调函数
        sigemptyset(&act.sa_mask);       // 设置屏蔽字，只在signalCatch工作时有效，清空sa_mask屏蔽字
        act.sa_flags = 0;                // 默认值

        // 注册信号捕捉函数, 当信号SIGINT触发时，调用signalCatch
        int res = sigaction(SIGINT, &act, &oldact);
        if (res == -1) {
                perror("sigaction error\n");
        }
        res = sigaction(SIGQUIT, &act, &oldact);
        if (res == -1) {
                perror("sigaction error\n");
        }

        while(1);
        return 0;
}
```

执行

![image-20211201210304819](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201210304819-1640859826542.png)

如图，两个信号都捕捉到了，并且输出了对应字符串。

#### 8.3 信号捕捉的特性

特性

```
捕捉函数执行期间，信号屏蔽字 由 mask --> sa_mask , 捕捉函数执行结束。 恢复回mask
捕捉函数执行期间，本信号自动被屏蔽(sa_flgs = 0).
捕捉函数执行期间，被屏蔽信号多次发送，解除屏蔽后只处理一次！
```

![image-20211201210433262](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201210433262-1640859834182.png)

![image-20211201210441700](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201210441700-1640859840268.png)

#### 8.4 内核实现信号捕捉简析

> 值得说明的是第四步到达第五步，因为函数处理完需要返回（类似于返回值的感觉），在函数处理之前我们
>
> 实在内核空间，所以还要返回一次。

![image-20211201210600498](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201210600498-1640859846062.png)

### 9 SIGCHLD信号（面试常问）

#### 9.1 SIGCHLD的产生条件

SIGCHLD的产生条件：

```
子进程终止时
子进程接收到SIGSTOP
子进程处于停止态，接收到SIGCONT后唤醒时
```

#### 9.2 借助 SIGCHLD 信号回收子进程(重)

> 子进程结束运行，其父进程会收到 SIGCHLD 信号。该信号的默认处理动作是忽略。
>
> 可以捕捉该信号，在捕捉函数中完成子进程状态的回收。

注意

> 有几个代码中需要注意的问题 [ 重要]
>
> （1）开始设置的信号阻塞，是为了防止子进程结束了，父进程还没有走到注册捕获信号函数。这样就会导致
>
> 子进程结束信号还是会执行默认的事件–忽略。所以多个了阻塞和解除阻塞
>
> （2）自定义事件do_sig_child中的while是为了防止，同一时间多个子进程同时结束，同时发送信号，就会导
>
> 致性质二【XXX 信号捕捉函数执行期间，XXX 信号自动被屏蔽。】。最多两个信号事件执行，多个子进程信
>
> 号被忽略了。
>
> （3）父进程结束的时候我们要加个循环，防止父进程先与子进程结束，导致捕捉信号执行自定义函数的
>
> 功能失效。

```c
/*************************************************************************
 > File Name: catchChild.c
 > Author: Winter
 > Created Time: 2021年11月06日 星期六 19时06分11秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<signal.h>
#include<sys/wait.h>

// 信号捕捉函数
void catchchild(int num) {
        pid_t wpid ;
        while ((wpid = wait(NULL))  != -1) {
                printf("--------catch child,id = %d\n",wpid);
        }
        return;
}

int main(int argc, char* argv[])
{
        pid_t pid;
        int i;
        // 创建5个子进程
        for (i = 0; i < 5; i++) {
                if ((pid = fork()) == 0) {
                        break;
                }
        }
        // 父进程回收子进程
        if (i == 5) {
                struct sigaction act, oldact;
                act.sa_handler = catchchild;    // 信号捕捉函数
                sigemptyset(&act.sa_mask);      // 设置捕捉函数执行期间屏蔽字  
                act.sa_flags = 0;               // 设置默认属性, 本信号自动屏蔽 
                // 父进程注册信号捕捉函数，回收子进程
                sigaction(SIGCHLD, &act, &oldact);
                printf("I'm parent, pid = %d \n", getpid());
                while (1);
        } else {
                printf("I‘m child ,pid = %d \n", getpid());
        }
        return 0;
}
```

执行

![image-20211201211436812](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211201211436812-1640859859334.png)

不清楚这里为什么会出现段错误。

**还有一个问题需要注意，这里有可能父进程还没注册完捕捉函数，子进程就死亡了，解决这个问题的方法，首先是**

**让子进程sleep，但这个不太科学。在fork之前注册也行，但这个也不是很科学。**

**最科学的方法是在int i之前设置屏蔽，等父进程注册完捕捉函数再解除屏蔽。这样即使子进程先死亡了，信号也因**

**为被屏蔽而无法到达父进程。解除屏蔽过后，父进程就能处理累积起来的信号了。**

### 10 中断系统调用

系统调用可分为两类：慢速系统调用和其他系统调用。  

- 慢速系统调用：可能会使进程永远阻塞的一类。如果在阻塞期间收到一个信号，该系统调用就被中断,不再

  继续执行(早期)；也可以设定系统调用是否重启。如， read、 write、 pause、 wait...  

- 其他系统调用： getpid、 getppid、 fork...


结合 pause，回顾慢速系统调用：

慢速系统调用被中断的相关行为，实际上就是 pause 的行为： 如， read  

① 想中断 pause，信号不能被屏蔽。

② 信号的处理方式必须是捕捉 (默认、忽略都不可以)

③ 中断后返回-1， 设置 errno 为 EINTR(表“被信号中断” )  

可修改 sa_flags 参数来设置被信号中断后系统调用是否重启。 SA_INTERRURT 不重启。 SA_RESTART 重启。  

sa_flags 还有很多可选参数， 适用于不同情况。 如：捕捉到信号后，在执行捕捉函数期间，不希望自动阻塞该

信号，可将 sa_flags 设置为 SA_NODEFER，除非 sa_mask 中包含该信号。

## 七、守护进程、线程

### 1 概念和会话

进程组，也称之为作业。 BSD 于 1980 年前后向 Unix 中增加的一个新特性。代表一个或多个进程的集合。每个

进程都属于一个进程组。在 waitpid 函数和 kill 函数的参数中都曾使用到。操作系统设计的进程组的概念，是为了

简化对多个进程的管理。  

当父进程，创建子进程的时候，默认子进程与父进程属于同一进程组。进程组 ID==第一个进程 ID(组长进程)。

所以，组长进程标识：其进程组 ID==其进程 ID 。

**可以使用 kill -SIGKILL -进程组 ID(负的)来将整个进程组内的进程全部杀死 。**

组长进程可以创建一个进程组，创建该进程组中的进程，然后终止。只要进程组中有一个进程存在，进程组就

存在，与组长进程是否终止无关。  

进程组生存期：进程组创建到最后一个进程离开(终止或转移到另一个进程组)  。

一个进程可以为自己或子进程设置进程组 ID 。

#### 1.1 创建会话

会话：多个进程组的集合。

创建一个会话需要注意以下 6 点注意事项：

```
1. 调用进程不能是进程组组长，该进程变成新会话首进程(session header)
2. 该进程成为一个新进程组的组长进程。
3. 需有 root 权限 (ubuntu 不需要)
4. 新会话丢弃原有的控制终端，该会话没有控制终端
5. 该调用进程是组长进程，则出错返回
6. 建立新会话时，先调用 fork, 父进程终止，子进程调用 setsid()  
```

**getsid函数**

获取进程所属的会话 ID

```c
pid_t getsid(pid_t pid);   // 成功：返回调用进程的会话 ID；失败： -1，设置 errno
 													 // pid 为 0 表示察看当前进程 session ID
```

ps ajx 命令查看系统中的进程。

参数 a 表示不仅列当前用户的进程，也列出所有其他用户的进程；

参数 x 表示不仅列有控制终端的进程，也列出所有无控制终端的进程；

参数 j 表示列出与作业控制相关的信息。  

**setsid函数**

创建一个会话，并以自己的 ID 设置进程组 ID，同时也是新会话的 ID。  

```c
pid_t setsid(void);        // 成功：返回调用进程的会话 ID；失败： -1，设置 errno
```

三合一：gid、sid、pid

![image-20211202192423400](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202192423400-1640859871704.png)

测试

```c
/*************************************************************************
 > File Name: session.c
 > Author: Winter
 > Created Time: 2021年11月09日 星期二 21时13分39秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int main(int argc, char* argv[])
{
        pid_t pid;
        if ((pid = fork()) < 0) {
                perror("fork error\n");
                exit(1);
        } else if (pid == 0) {
                printf("child process PID = %d\n", getpid());           // 进程id
                printf("Group ID of child GID = %d\n", getpgid(0));     // 进程组id
                printf("session ID of child SID = %d\n", getsid(0));    // 会话id

                sleep(10);
                setsid();      // 子进程不是组长进程，可以成为新会话首进程，且成为组长进程，该进程组长id即为会话
                printf("Change:\n");

                printf("child process PID = %d\n", getpid());           // 进程id
                printf("Group ID of child GID = %d\n", getpgid(0));     // 进程组id
                printf("session ID of child SID = %d\n", getsid(0));    // 会话id

                sleep(20);
                exit(0);
        }
        return 0;
}
```

执行

![image-20211202192833075](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202192833075-1640859881660.png)

### 2 守护进程

#### 2.1 什么是守护进程

1 在linux系统中，我们会发现在系统启动的时候有很多的进程就已经开始跑了，也称为服务，这也是我们所说的

守护进程。

2 守护进程（daemon）是生存期长的一种进程，没有控制终端。

3 它们常常在系统引导装入时启动，仅在系统关闭时才终止。

4 UNIX系统有很多守护进程，守护进程程序的名称通常以字母“d”结尾：例如，syslogd 就是指管理系统日志的守

护进程

5 通过ps进程查看器 ps -efj 的输出实例，内核守护进程的名字出现在方括号中，大致输出如下：

![image-20211202193309517](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202193309517-1640859889966.png)

守护进程：

> daemon进程。通常运行于操作系统后台，脱离控制终端。一般不与用户直接交互。周期性的等待某个事件
>
> 发生或周期性执行某一动作。
>
> 不受用户登录注销影响。通常采用以d结尾的命名方式。
>
> 创建守护进程，最关键的一步是调用setsid函数创建一个新的Session，并成为Session Leader。

#### 2.2 守护进程创建步骤

> 1. fork子进程，让父进程终止。
>
> 2. 子进程调用 setsid() 创建新会话
>
> 3. 通常根据需要，改变工作目录位置 chdir()， **防止目录被卸载**。（U盘）
>
> 4. 通常根据需要，重设umask文件权限掩码，影响新文件的创建权限。 022 -- 755 0345 --- 432  r---wx-w-  422
>
> 5. 通常根据需要，关闭/重定向（0/1/2） dev/null 文件描述符
>
> 6. 守护进程 业务逻辑。while（）

#### 2.3 守护进程代码实现（重点）

主要是理解一些概念，重点参考一下文献。

```c
/*************************************************************************
 > File Name: daemon.c
 > Author: Winter
 > Created Time: 2021年11月17日 星期三 19时26分01秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<fcntl.h>
#include<sys/stat.h>
int main(int argc, char* argv[])
{
        pid_t pid;          // 进程
        int res;            // 返回值
        int fd;             // 文件描述符
        pid = fork();
        if (pid > 0) {      // 父进程
                exit(1);
        } else {            // 子进程
                pid = setsid();
                if (pid == -1) {
                        perror("setsid error\n");
                        exit(1);
                }
                res = chdir("/home/winter/linuxStudy");   // 改变工作目录位置
                if (res == -1) {
                        perror("chdir error\n");
                        exit(1);
                }
                umask(0022);                    		 // 改变文件访问权限掩码
                close(STDIN_FILENO);            		 // 关闭文件描述符0
                fd = open("/dev/null", O_RDWR);      // 读写权限打开  fd就是0
                if (fd == -1) {
                        perror("open error\n");
                        exit(1);
                }
                dup2(fd, STDOUT_FILENO);             // 重定向，将STDOUT_FILENO指向fd
                dup2(fd, STDERR_FILENO);

                // 模拟守护进程业务
                while (1);
        }
        return 0;
}
```

执行

![image-20211202194322831](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202194322831-1640859905867.png)

这个daemon进程就不会受到用户登录注销影响。

要想终止，就必须用kill命令.

### 3 线程

#### 3.1 什么是线程

1. 轻量级进程(light-weight process)，也有 PCB，创建线程使用的底层函数和进程一样，都是 clone

2. 从内核里看进程和线程是一样的，都有各自不同的 PCB，但是 PCB 中指向内存资源的三级页表是相同的（下

   图区别进程）

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202194734084-1640859912348.png" alt="image-20211202194734084" style="zoom:80%;" />

3 进程可以蜕变成线程

4 线程可看做寄存器和栈的集合

5 在 linux 下，线程最是小的执行单位；进程是最小的分配资源单位

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202194845966-1640859921361.png" alt="image-20211202194845966" style="zoom:80%;" />

> 参考：《Linux 内核源代码情景分析》
>      对于进程来说，相同的地址(同一个虚拟地址)在不同的进程中，反复使用而不冲突。原因是他们虽虚拟址
>
> 一样，但，页目录、页表、物理页面各不相同。相同的虚拟址，映射到不同的物理页面内存单元，最终访问
>
> 不同的物理页面。
>
>   但！线程不同！两个线程具有各自独立的 PCB，但共享同一个页目录，也就共享同一个页表和物理页面。所
>
> 以两个 PCB 共享一个地址空间。
>     实际上，无论是创建进程的 fork，还是创建线程的 pthread_create，底层实现都是调用同一个内核函数 
>
> clone。
>
> ​    如果复制对方的地址空间，那么就产出一个“进程”；如果共享对方的地址空间，就产生一个“线程”。
>
> 因此：Linux 内核是不区分进程和线程的。只在用户层面上进行区分。所以，线程所有操作函数 pthread_* 是库函数，而非系统调用。

线程概念：

```
进程：有独立的 进程地址空间。有独立的pcb。  分配资源的最小单位。
线程：有独立的pcb。没有独立的进程地址空间。 最小单位的执行。
ps -Lf 进程id   ---> 线程号。LWP --》cpu 执行的最小单位。
```

<img src="linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202195047627-1640859928175.png" alt="image-20211202195047627" style="zoom:80%;" />

![image-20211202195107287](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202195107287-1640859938801.png)

![image-20211202195838876](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202195838876-1640859942328.png)

```shell
ps -Lf 进程号		             # 查看进程的线程
```

![image-20211202195238775](D:\typora\instal\Typora\document\linux系统编程.assets\image-20211202195238775.png)

#### 3.2 线程共享资源

1. 文件描述符表
2. 每种信号的处理方式
3. 当前工作目录
4. 用户 ID 和组 ID
5. **内存地址空间** (.text/.data/.bss/heap/共享库)

#### 3.3 线程间非共享资源

1. 线程 id
2. 处理器现场和栈指针(内核栈)
3. **独立的栈空间**(用户空间栈)
4. errno 变量
5. **信号屏蔽字**
6. 调度优先级

#### 3.4 线程的优缺点

**优点：**

1. 提高程序并发性
2. 开销小
3. 数据通信、共享数据方便

**缺点：**

1. 线程不稳定（第三方库函数实现）
2. 线程调试困难
3. 等待使用共享资源时造成程序运行速度变慢，主要是一些独占性的资源
4. 线程的死锁，较长时间的等待或者资源竞争造成死锁

#### 3.5 线程控制原语

> 编译的时候记得后面 `-l pthread` 毕竟第三方库实现。

##### **3.5.1 pthread_self 函数**

**作用**：获取线程 ID。其作用对应进程中 getpid() 函数。

```c
pthread_t pthread_self(void);    // 成功返回本线程id
```

##### **3.5.2 pthread_create 函数**

创建一个新线程，其作用，对应进程中fork()函数。

```c
int pthread_create(pthread_t *thread, 
                   const pthread_attr_t *attr, 
                   void *(*start_routine) (void *), 
                   void *arg); 
	// 成功返回0，失败返回errno
	// 参数一：表示传出参数，表示创建的子线程id
	// 参数二：线程属性，传NILL表使用默认属性
	// 参数三：函数指针，指向线程主函数(线程体)，该函数运行结束，则线程结束。
	// 参数四：参数三函数的参数，空传NULL
```

测试

```c
/*************************************************************************
 > File Name: pthread_create.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 20时24分47秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 子线程回调函数
void *tfn(void* arg) {
        printf("pthread: pid = %d, tid = %lu\n",getpid(), pthread_self());
        return NULL;
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        printf("main: pid = %d, tid = %lu\n",getpid(), pthread_self());

        int res =  pthread_create(&tid, NULL, tfn, NULL);                // 创建一个线程
        if (res < 0) {
                perror("pthread_create error\n");
                exit(1);
        }
        return 0;
}
```

执行

![image-20211202200915908](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202200915908-1640859958193.png)

可以看到，子线程的打印信息并未出现。原因在于，主线程执行完之后，就销毁了整个进程的地址空间，于是子线

程就无法打印。简单粗暴的方法就是让主线程睡1秒，等子线程执行。

```c
/*************************************************************************
 > File Name: pthread_create.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 20时24分47秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 子线程回调函数
void *tfn(void* arg) {
        printf("pthread: pid = %d, tid = %lu\n",getpid(), pthread_self());
        return NULL;
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        printf("main: pid = %d, tid = %lu\n",getpid(), pthread_self());

        int res =  pthread_create(&tid, NULL, tfn, NULL);                // 创建一个线程
        if (res < 0) {
                perror("pthread_create error\n");
                exit(1);
        }
        sleep(1);                // 在这里添加休眠
        return 0;
}
```

执行

![image-20211202201052690](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202201052690-1640859980813.png)

##### **3.5.3 循环创建多个子线程**

```c
/*************************************************************************
 > File Name: pthread_more.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 20时48分44秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 子线程回调函数
void* tfn(void* arg) {
        int i = (int)arg;
        sleep(i);
        printf("-----I'm %d th thread: pid = %d, tid = %lu\n",i + 1, getpid(), pthread_self());
        return NULL;
}

int main(int argc, char* argv[])
{
        int i;
        int res;
        pthread_t tid;     // 线程id
        for (i = 0; i < 5; i++) {
                res = pthread_create(&tid, NULL, tfn, (void*)i);     // 创建线程
                if (res != 0) {
                        perror("pthread_create error\n");
                        exit(1);
                }
        }
        printf("-------main: pid = %d, tid = %lu\n", getpid(), pthread_self());
        sleep(i);
        return 0;
}
```

执行

![image-20211202201355900](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202201355900-1640860005501.png)

编译时会出现类型强转的警告，指针4字节转int的8字节，不过不存在精度损失，忽略就行。

##### 3.5.4 线程间全局变量

```c
/*************************************************************************
 > File Name: pthread_global.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 21时13分04秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int var = 10;

// 子线程回调函数
void* tfn(void* arg) {
        var = 100;
        printf("pthread: pid = %d, tid = %lu\n", getpid(), pthread_self());
        return NULL;
}
int main(int argc, char* argv[])
{
        printf("At first var = %d\n", var);
        pthread_t tid;       // 子线程id
        int res = pthread_create(&tid, NULL, tfn, NULL);
        if (res != 0) {
                perror("pthread error\n");
                exit(1);
        }
        sleep(1);
        printf("After pthread_create, var = %d\n", var);
        return 0;
}
```

执行

![image-20211202201903223](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202201903223-1640860014424.png)

**可以看到，子线程里更改全局变量后，主线程里也跟着发生变化。**

##### 3.5.5 pthread_exit退出

**作用**：将单个线程退出。

```c
void pthread_exit(void *retval); 
	// 参数：retval 表示线程退出状态，通常传 NULL
```

比较

```c
exit();          // 退出当前进程。
return:  				 // 返回到调用者那里去。
pthread_exit():  // 退出当前线程。
```

**重点**

1 exit函数

```c
// 如果在回调函数里加一段代码：
if(i == 2)
{
		exit(0);
}	
// 看起来好像是退出了第三个子线程，然而运行时，发现后续的4,5也没了。这是因为，exit是退出进程。
```

2 return

```c
// 修改一下，换成：
if(i == 2)
{
	return NULL;
}
// 这样运行一下，发现后续线程不会凉凉，说明return是可以达到退出线程的目的。
// 然而真正意义上，return是返回到函数调用者那里去，线程并没有退出。
```

3

```c
// 再修改一下，再定义一个函数func，直接返回那种
void *func(void){
	return NULL;
}

if(i == 2)
{
	func();
}
// 运行，发现1,2,3,4,5线程都还在，说明没有达到退出目的。
```

4 

```c
// 再次修改：
void *func(void){
	pthread_exit(NULL);
	return NULL;
}

if(i == 2)
{
	func();
}
```

编译运行，发现3没了，看起来很科学的样子。

pthread_exit表示将当前线程退出。放在函数里，还是直接调用，都可以。

##### 3.5.6 pthread_join 函数（重）

**作用**：阻塞等待线程退出，获取线程退出状态其作用，对应进程中 waitpid() 函数

补充：任意线程得到其他线程的pid都可以回收，没有父线程回收子线程的说法。而进程需要父进程回收子进程。

```c
int pthread_join(pthread_t thread, void **retval);	
		// 阻塞 回收线程。
		// thread: 待回收的线程id
		// retval：传出参数。 回收的那个线程的退出值。
		// 线程异常借助，值为 -1。
		// 返回值：成功：0
		//失败：errno
```

下面这个是回收线程并获取子线程返回值的小例子：

```c
/*************************************************************************
 > File Name: pthread_join.c
 > Author: Winter
 > Created Time: 2021年11月19日 星期五 19时35分02秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

struct thrd {
        int var;
        char str[256];
};


// 回调函数
void* tfn(void* arg) {
        struct thrd* tval;
        tval = malloc(sizeof(struct thrd));      // 申请空间
        tval->var = 100;
        strcpy(tval->str ,"hello thread");       // 拷贝数据
        return (void*) tval;
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        struct thrd* retval;

        int res = pthread_create(&tid, NULL, tfn, NULL);
        if (res != 0) {
                perror("pthread_create error\n");
                exit(1);
        }
        res = pthread_join(tid, (void**)(&retval));
        if (res != 0) {
                perror("pthread_join error\n");
                exit(1);
        }
        printf("Child thread exit with var = %d, str = %s\n", retval->var, retval->str);

        pthread_exit(NULL);
}

```

执行

![image-20211202203514563](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202203514563-1640860043437.png)

使用pthread_join函数将循环创建的多个子线程回收

**这里tid要使用数组来存**

```c
/*************************************************************************
 > File Name: pthrd_loop_join.c
 > Author: Winter
 > Created Time: 2021年11月19日 星期五 20时11分50秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

int var = 100;

void* tfn(void* arg) {
        int i;
        i = (int)arg;
        if (i == 1) {
                var = 111;
                printf("I'm %dth pthread tid = %lu, var = %d\n", i, pthread_self(), var);
                return (void*)var;
        } else if (i == 3) {
                var = 333;
                printf("I'm %dth pthread tid = %lu, var = %d\n", i, pthread_self(), var);
                return (void*)var;
        } else {
                printf("I'm %dth pthread tid = %lu, var = %d\n", i, pthread_self(), var);
                return (void*)var;
        }

        return NULL;

}

int main(int argc, char* argv[])
{
        pthread_t tid[5];
        int i;
        int* res[5];

        // 循环创建多个子进程
        for (i = 0; i < 5; i++) {
                pthread_create(&tid[i], NULL, tfn, (void*)i);
        }

        // 循环回收多个子进程
        for (i = 0; i < 5; i++) {
                pthread_join(tid[i], (void**)(&res[i]));
                printf("--------------%d 's res = %d\n", i, (int)(res[i]));
        }

        // 输出主线程
        printf("I'm main pthread tid = %lu, var = %d\n", pthread_self(), var);
        return 0;
}

```

执行

![image-20211202203801584](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202203801584-1640860053234.png)

##### 3.5.7 pthread_cancel函数

**作用**：杀死(取消)线程，其作用，对应进程中 kill() 函数。

> 线程的取消并不是实时的，而有一定的延时。需要等待线程到达某个取消点(检查点)。
>
> 取消点：是线程检查是否被取消，并按请求进行动作的一个位置。通常是一些系统调用
>
> creat，open，pause， close，read，write… 执行命令 man 7 pthreads
>
> 可以查看具备这些取消点的系统调用列表。也可参阅 APUE.12.7 取消选项小节。
>
> 可粗略认为一个系统调用(进入内核)即为一个取消点。如线程中没有取消点，可以通过调pthread_testcancel
>
> 函数自行设置一个取消点

```c
int pthread_cancel(pthread_t thread);		
		// 杀死一个线程。  需要到达取消点（保存点）
		// thread: 待杀死的线程id
		// 返回值：成功：0
		//       失败：errno
```

如果，子线程没有到达取消点， 那么 pthread_cancel 无效。

我们可以在程序中，手动添加一个取消点。使用 pthread_testcancel();

成功被 pthread_cancel() 杀死的线程，返回 -1.使用pthead_join 回收。

小例子，主线程调用pthread_cancel杀死子线程

```c
/*************************************************************************
 > File Name: pthread_cancel.c
 > Author: Winter
 > Created Time: 2021年11月21日 星期日 17时27分33秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 回调函数
void* tfn(void* arg) {
        while(1) {
                printf("thread: pid = %d, tid = %lu\n", getpid(), pthread_self());
                sleep(1);
        }
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        int res = pthread_create(&tid, NULL, tfn, NULL);    // 创建线程
        if (res != 0) {
                fprintf(stderr, "pthread_create error:%s\n",strerror(res));
                exit(1);
        }
        printf("main: pid = %d, tid = %lu\n", getpid(), pthread_self());
        sleep(5);                                          // 父线程睡5s
        res = pthread_cancel(tid);                         // 终止线程
        if (res != 0) {
                fprintf(stderr, "pthread_cancel error:%s\n",strerror(res));
                exit(1);
        }
        while (1);                                         // 不退出
        pthread_exit((void*)0);
}
```

执行

![image-20211202204210665](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202204210665-1640860064290.png)

可以看到，主线程确实kill了子线程。

这里要注意一点，pthread_cancel工作的必要条件是进入内核，如果tfn真的奇葩到没有进入内核，则

pthread_cancel不能杀死线程，**此时需要手动设置取消点**，就是pthread_testcancel()

##### 3.5.8 pthread_detach 函数

**作用**：实现线程分离，线程结束后，自动释放资源。无需pthread_join() 回收资源。

```c
int pthread_detach(pthread_t thread);		
		// 设置线程分离
		// thread: 待分离的线程id
		// 返回值：成功：0
		//      	失败：errno	
```

> 线程分离状态：指定该状态，线程主动与主控线程断开关系。线程结束后，其退出状态不由其他线程获取，
>
> 而直接自己自动释放。网络、多线程服务器常用。
>
> 进程若有该机制，将不会产生僵尸进程。僵尸进程的产生主要由于进程死后，大部分资源被释放，一点残留
>
> 资源仍存于系统中，导致内核认为该进程仍存在。
>
> 也可使用 pthread_create 函数参 2(线程属性)来设置线程分离。

下面这个例子，使用detach分离线程，照理来说，分离后的线程会自动回收：

```c
/*************************************************************************
 > File Name: pthread_create.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 20时24分47秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 子线程回调函数
void *tfn(void* arg) {
        printf("pthread: pid = %d, tid = %lu\n",getpid(), pthread_self());
        return NULL;
}

int main(int argc, char* argv[])
{
        pthread_t tid;

        int res =  pthread_create(&tid, NULL, tfn, NULL);                // 创建一个线程
        if (res != 0) {
//              printf("pthread_create error:%s\n", strerror(res));
                fprintf(stderr, "pthread_create error:%s\n", strerror(res));
                exit(1);
        }
        res = pthread_detach(tid);                                      // 设置线程分离，分离完的程序可以自动回收
        if (res != 0) {
//              printf("pthread_detach error:%s\n", strerror(res));
                fprintf(stderr, "pthread_detach error:%s\n", strerror(res));
                exit(1);
        }
        sleep(1);
        res = pthread_join(tid, NULL);                                  // 回收子线程
        printf("join res = %d\n", res);
        if (res != 0) {
//              printf("pthread_join error:%s\n", strerror(res));
                fprintf(stderr, "pthread_join error:%s\n", strerror(res));
                exit(1);
        }

        printf("main: pid = %d, tid = %lu\n",getpid(), pthread_self());
        pthread_exit((void*)0);
}

```

这里是最终版，使用fprintf函数和strerror函数。

执行

![image-20211202204827258](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202204827258-1640860099974.png)

#### 3.6 线程进程控制原语比对

| 进程   | 线程             |
| ------ | ---------------- |
| fork   | pthread_create   |
| exit   | pthread_exit     |
| wait   | pthread_join     |
| kill   | pthread_cancel   |
| getpid | pthread_self     |
|        | pthread_detach() |

#### 3.7 线程分离属性设置

线程属性：

  设置分离属性。

```c
pthread_attr_t attr;  	                                     // 创建一个线程属性结构体变量
pthread_attr_init(&attr);	                                   // 初始化线程属性
pthread_attr_setdetachstate(&attr,  PTHREAD_CREATE_DETACHED);// 设置线程属性为 分离态
pthread_create(&tid, &attr, tfn, NULL);                      // 借助修改后的 设置线程属性 创建为分离态的新线程
pthread_attr_destroy(&attr);	                               // 销毁线程属性
```

调整线程状态，使线程创建出来就是分离态，代码如下：

```c
/*************************************************************************
 > File Name: pthread_create.c
 > Author: Winter
 > Created Time: 2021年11月18日 星期四 20时24分47秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 子线程回调函数
void *tfn(void* arg) {
        printf("pthread: pid = %d, tid = %lu\n",getpid(), pthread_self());
        return NULL;
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        pthread_attr_t attr;                   // 结构体变量
        int res = pthread_attr_init(&attr);    // 创建分离
        if (res != 0) {
                fprintf(stderr, "pthread_attr_init error :%s\n", strerror(res));
                exit(1);
        }

        if (res != 0) {
                fprintf(stderr, "pthread_attr_init error :%s\n", strerror(res));
                exit(1);
        }
        res = pthread_attr_setdetachstate(&attr, PTHREAD_CREATE_DETACHED);    // 设置线程属性为分离属性
        if (res != 0) {
                fprintf(stderr, "pthread_attr_setdetachstate error :%s\n", strerror(res));
                exit(1);
        }


        res =  pthread_create(&tid, &attr, tfn, NULL);                // 创建一个线程
        if (res != 0) {
                fprintf(stderr, "pthread_create error: %s\n", strerror(res));
                exit(1);
        }


        res = pthread_attr_destroy(&attr);    // 回收分离
        if (res != 0) {
                fprintf(stderr, "pthread_attr_destroy error :%s\n", strerror(res));
                exit(1);
        }
        sleep(1);                             // 保证子进程结束

        res = pthread_join(tid, NULL);        // 阻塞回收，分离成功，这里应该回收失败
        if (res != 0) {
                fprintf(stderr, "pthread_join error :%s\n", strerror(res));
                exit(1);
        }

        printf("main: pid = %d, tid = %lu\n",getpid(), pthread_self());

        pthread_exit((void*)0);
}
```

执行

![image-20211202205934662](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202205934662-1640860114669.png)

如图，pthread_join报错，说明线程已经自动回收，设置分离成功。

#### 3.8 线程属性注意事项（重）

1 主线程退出其他线程不退出，主线程应调用 pthread_exit

2 避免僵尸线程
	pthread_join

​	pthread_detach

​	pthread_create 指定分离属性

​	被 join 线程可能在 join 函数返回前就释放完自己的所有内存资源，所以不应当返回被回收线程栈中的值;

3 malloc 和 mmap 申请的内存可以被其他线程释放

4 应避免在多线程模型中调用 fork 除非，马上 exec，子进程中只有调用 fork 的线程存在，其他线程在子进程

中均 pthread_exit

5 信号的复杂语义很难和多线程共存，应避免在多线程引入信号机制

## 八、线程同步

### 1 同步的概念

所谓同步， 即同时起步，协调一致。不同的对象， 对“同步” 的理解方式略有不同。 如，设备同步，是指在两个设

备之间规定一个共同的时间参考； 数据库同步， 是指让两个或多个数据库内容保持一致，或者按需要部分保持

一致； 文件同步， 是指让两个或多个文件夹里的文件保持一致。等等 

而编程中、 通信中所说的同步与生活中大家印象中的同步概念略有差异。“同” 字应是指协同、协助、互相配合。 

主旨在协同步调，按预定的先后次序运行。  

### 2 线程同步的概念

同步即协同步调，按预定的先后次序运行。

线程同步，指一个线程发出某一功能调用时，在没有得到结果之前，该调用不返回。同时其它线程为保证数据一致

性，不能调用该功能。  

“同步”的目的，是为了避免数据混乱，解决与时间有关的错误。实际上，不仅线程间需要同步，进程间、信号间等

等都需要同步机制。因此， 所有“多个控制流，共同操作一个共享资源” 的情况，都需要同步。  

### 3 为什么要有线程同步

![image-20211202210852041](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202210852041-1640860124744.png)

- 共享资源，多个线程都可对共享资源操作 [容易产生冲突]
- 线程操作共享资源的先后顺序不确定
- cpu处理器对存储器的操作一般不是原子操作

> 举个例子：
>        两个线程都把全局变量增加1，这个操作平台需要三条指令完成。
>
> ​		从内存读到寄存器 →寄存器的值加1 →将寄存器的值写会内存。
>
> 如果此时线程A取值在寄存器修改还未写入内存，线程B就从内存取值就会导致两次操作实际上只修改过
>
> 一次。或者说后一次线程做的事情覆盖前一次线程做的事情。实际上就执行过一次线程。

### 4 互斥量 mutex

#### 4.1 基本概念

- Linux 中提供一把互斥锁 mutex（也称之为互斥量）。

- 每个线程在对资源操作前都尝试先加锁，成功加锁才能操作，操作结束解锁。

- 资源还是共享的，线程间也还是竞争的，但通过“锁”就将资源的访问变成互斥操作，而后与时间有关的错误也

  不会再产生了。

![image-20211202211057470](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202211057470-1640860131257.png)

锁的使用：

建议锁！对公共数据进行保护。所有线程【应该】在访问公共数据**前先拿锁再访问**。但，锁本身不具备强制性。

#### 4.2 借助互斥锁管理共享数据实现同步

主要应用函数：

```c
pthread_mutex_init
pthread_mutex_destory  
pthread_mutex_lock    
pthread_mutex_trylock  
pthread_mutex_unlock   
```

以上5个函数的返回值都是：成功返回0，失败返回错误号

pthread_mutex_t 类型，**其本质是一个结构体**。为简化理解，应用时可忽略其实现细节，简单当成整数看待

pthread_mutex_t mutex；变量mutex只有两种取值：0,1

![image-20211202211938806](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202211938806-1640860163424.png)

**使用mutex(互斥量、互斥锁)一般步骤【重点】**

```
pthread_mutex_t 类型。 
1 pthread_mutex_t lock; 创建锁
2 pthread_mutex_init; 初始化   1
3 pthread_mutex_lock;加锁   1-- --> 0
4 访问共享数据（stdout)  
5 pthrad_mutext_unlock();解锁   0++ --> 1
6 pthead_mutex_destroy；销毁锁
```

**pthread_mutex_init函数**

```c
int pthread_mutex_init(pthread_mutex_t *restrict mutex, 
												const pthread_mutexattr_t *restrict attr)
// 这里的restrict关键字，表示指针指向的内容只能通过这个指针进行修改
// restrict关键字：
// 用来限定指针变量。被该关键字限定的指针变量所指向的内存操作，必须由本指针完成。
```

初始化互斥量：

```c
pthread_mutex_t mutex;

pthread_mutex_init(&mutex, NULL);       						 // 第1种：动态初始化。
pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;   // 第2种：静态初始化。
```

**pthread_mutex_destroy 函数**

销毁一个互斥锁

```c
int pthread_mutex_destroy(pthread_mutex_t *mutex);
```

**pthread_mutex_lock 函数**

加锁。 可理解为将 mutex--（或 -1），操作后 mutex 的值为 0 。

```c
int pthread_mutex_lock(pthread_mutex_t *mutex);
```

**pthread_mutex_unlock 函数**

解锁。 可理解为将 mutex ++（或 +1），操作后 mutex 的值为 1。

```c
int pthread_mutex_unlock(pthread_mutex_t *mutex);
```

**pthread_mutex_trylock 函数**

尝试加锁

```c
int pthread_mutex_trylock(pthread_mutex_t *mutex);
```

**加锁与解锁**

lock 与 unlock：

​	lock 尝试加锁，如果加锁不成功，线程阻塞，阻塞到持有该互斥量的其他线程解锁为止。  

​	unlock 主动解锁函数， 同时将阻塞在该锁上的所有线程全部唤醒，至于哪个线程先被唤醒，取决于优先级、调

度。默认：先阻塞、先唤醒  

​	例如： T1 T2 T3 T4 使用一把 mutex 锁。 T1 加锁成功，其他线程均阻塞，直至 T1 解锁。 T1 解锁后， T2 T3 

T4 均被唤醒，并自动再次尝试加锁。  

​	可假想 mutex 锁 init 成功初值为 1。 lock 功能是将 mutex--。而 unlock 则将 mutex++。

lock 与 trylock：  

​	lock 加锁失败会阻塞，等待锁释放。

​	trylock 加锁失败直接返回错误号（如： EBUSY），不阻塞。  

**使用锁实现互斥访问共享区：**

```c
/*************************************************************************
 > File Name: pthread_sync_test.c
 > Author: Winter
 > Created Time: 2021年11月22日 星期一 19时15分01秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 定义互斥全局锁
pthread_mutex_t mutex;

// 回调函数
void* tfn(void* arg) {
        srand(time(NULL));
        while (1) {
                pthread_mutex_lock(&mutex);    // 加锁
                printf("hello");
                sleep(rand() % 3);      // 模拟长时间操作共享资源，导致CPU易主，产生与时间有关的错误
                printf("world\n");
                pthread_mutex_unlock(&mutex);    // 解锁
                sleep(rand() % 3);
        }
        return NULL;
}

int main(int argc, char* argv[])
{
        pthread_t tid;
        srand(time(NULL));

        int res = pthread_mutex_init(&mutex, NULL);       // 初始化互斥锁
        if (res != 0) {
                fprintf(stderr, "pthread_mutex_init error: %s\n",strerror(res));
                exit(1);
        }


        res = pthread_create(&tid, NULL, tfn, NULL);
        if (res != 0) {
                fprintf(stderr, "pthread_create error: %s\n",strerror(res));
                exit(1);
        }
        while (1) {
                pthread_mutex_lock(&mutex);    // 加锁
                printf("HELLO");
                sleep(rand() % 3);      // 模拟长时间操作共享资源，导致CPU易主，产生与时间有关的错误
                printf("WORLD\n");
                pthread_mutex_unlock(&mutex);    // 解锁
                sleep(rand() % 3);
        }
        pthread_join(tid, NULL);


        res = pthread_mutex_destroy(&mutex);       // 销毁互斥锁
        if (res != 0) {
                fprintf(stderr, "pthread_mutex_destroy error: %s\n",strerror(res));
                exit(1);
        }
        pthread_exit((void*)0);
}
```

执行

![image-20211202211415998](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211202211415998-1640860180822.png)

可以看到，主线程和子线程在访问共享区时就没有交叉输出的情况了。

##### 4.2.1 互斥锁的使用技巧

注意事项：

**尽量保证锁的粒度， 越小越好。（访问共享数据前，加锁。访问结束【立即】解锁。）**

 互斥锁，**本质是结构体。 我们可以看成整数。** 初值为 1。（**pthread_mutex_init() 函数调用成功。**）

**加锁： --操作， 阻塞线程**。

**解锁： ++操作， 唤醒阻塞在锁上的线程**。

try锁：尝试加锁，成功--。失败，返回。同时设置错误号 EBUSY

##### 4.2.2 try锁

加锁：--操作，阻塞线程

解锁：++操作，唤醒阻塞在锁上的线程

try锁：尝试加锁，成功--，加锁失败直接返回错误号(如EBUSY)，不阻塞。

### 5 读写锁

与互斥量类似，但读写锁允许更高的并行性。其特性为： **写独占，读共享。**

#### 5.1 读写锁状态

特别强调： 读写锁只有一把， 但其具备两种状态：

1 读模式下加锁状态 (读锁)；

2 写模式下加锁状态 (写锁)  。

#### 5.2 读写锁特性

1 读写锁是“写模式加锁”时， 解锁前，所有对该锁加锁的线程都会被阻塞。  

2 读写锁是“读模式加锁”时， 如果线程以读模式对其加锁会成功；如果线程以写模式加锁会阻塞。  

3 读写锁是“读模式加锁”时， 既有试图以写模式加锁的线程，也有试图以读模式加锁的线程。那么读写锁

会阻塞随后的读模式锁请求。优先满足写模式锁。 **读锁、写锁并行阻塞， 写锁优先级高**  。

读写锁也叫共享-独占锁。当读写锁以读模式锁住时，它是以共享模式锁住的；当它以写模式锁住时，它是以独

占模式锁住的。 **写独占、读共享**。  

读写锁非常适合于对数据结构读的次数远大于写的情况。

**主要函数**

```c
pthread_rwlock_t  rwlock;
pthread_rwlock_init(&rwlock, NULL);
pthread_rwlock_rdlock(&rwlock);		    // try
pthread_rwlock_wrlock(&rwlock);		    // try
pthread_rwlock_unlock(&rwlock);
pthread_rwlock_destroy(&rwlock);
```

以上函数都是成功返回0，失败返回错误号。

**pthread_rwlock_init 函数**

初始化一把读写锁  

```c
int pthread_rwlock_init(pthread_rwlock_t *restrict rwlock, 
												const pthread_rwlockattr_t *restrict attr);
	// 参 2： attr 表读写锁属性，通常使用默认属性， 传 NULL 即可。
```

**pthread_rwlock_destroy 函数**

销毁一把读写锁

```c
int pthread_rwlock_destroy(pthread_rwlock_t *rwlock);
```

**pthread_rwlock_rdlock 函数**

以读方式请求读写锁。（常简称为：请求读锁）

```c
int pthread_rwlock_rdlock(pthread_rwlock_t *rwlock);
```

**pthread_rwlock_wrlock 函数**

以写方式请求读写锁。（常简称为：请求写锁）

```c
int pthread_rwlock_wrlock(pthread_rwlock_t *rwlock);
```

**pthread_rwlock_unlock 函数**

解锁

```c
int pthread_rwlock_unlock(pthread_rwlock_t *rwlock);
```

**pthread_rwlock_tryrdlock 函数**

非阻塞以读方式请求读写锁（非阻塞请求读锁）

```c
int pthread_rwlock_tryrdlock(pthread_rwlock_t *rwlock);
```

**pthread_rwlock_trywrlock函数**

非阻塞以写方式请求读写锁（非阻塞请求写锁）

```c
int pthread_rwlock_trywrlock(pthread_rwlock_t *rwlock);
```

#### 5.3 两种死锁

是使用锁不恰当导致的现象：

**1. 对一个锁反复lock。**   

**2. 两个线程，各自持有一把锁，请求另一把。**

![image-20211203102505609](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203102505609-1640860197116.png)

#### 5.4 读写锁案例

```c
/*************************************************************************
 > File Name: rwlock.c
 > Author: Winter
 > Created Time: 2021年11月22日 星期一 20时58分33秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 全局共享
int counter;
pthread_rwlock_t rwlock;          // 全局读写锁

// 3个线程不同时写同一资源，5个线程不定时读同一资源

// 写锁
void* th_write(void* arg) {
        int t;
        int i = (int)arg;
        while (1) {
                pthread_rwlock_wrlock(&rwlock);                   // 加锁  写锁，独占
                t = counter;
                usleep(1000);
                printf("===========write: %d, %lu, counter = %d, ++counter = %d\n", i, pthread_self(), t, ++counter);   // 写
                pthread_rwlock_unlock(&rwlock);
                usleep(1000);
        }
        return NULL;
}


// 读
void* th_read(void* arg) {
        int i = (int)arg;
        while (1) {
                pthread_rwlock_rdlock(&rwlock);                   // 加锁 读锁共享
                printf("=================================read: %d, %lu, counter = %d\n", i, pthread_self(), counter);
                pthread_rwlock_unlock(&rwlock);                   // 解锁
                usleep(2000);
        }
        return NULL;


}


int main(int argc, char* argv[])
{
        int i;
        pthread_t tid[8];

        pthread_rwlock_init(&rwlock, NULL);                         // 初始化读写锁

        for (i = 0; i < 3; i++) {
                pthread_create(&tid[i], NULL, th_write, (void*)i);  // 写锁
        }
        for (i = 0; i < 5; i++) {
                pthread_create(&tid[i], NULL, th_read, (void*)i);    // 读锁

        }
        for (i = 0; i < 8; i++) {
                pthread_join(tid[i], NULL);                          // 回收子线程
        }
        pthread_rwlock_destroy(&rwlock);                             // 销毁读写锁
        return 0;
}
```

执行

![image-20211203102932016](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203102932016-1640860212402.png)

程序输出飞快，随便截个图，如上图。

**由于读共享，写独占。写锁优先级高。前面5个read一定先于后面的write到达的，不然write会抢到锁先进行写**

**操作。**

### 6 条件变量

#### 6.1 基本概念

条件变量本身不是锁！但它也可以造成线程阻塞。通常与互斥锁配合使用。给多线程提供一个会合的场所。

#### 6.2 为什么使用条件变量

线程抢占互斥锁时，线程A抢到了互斥锁，但是条件不满足，线程A就会让出互斥锁让给其他线程，然后等待其他

线程唤醒他；一旦条件满足，线程就可以被唤醒，并且拿互斥锁去访问共享区。经过这中设计能让进程运行更稳

定。

#### 6.3 函数使用

##### 6.3.1 pthread_cond_init 函数

**作用**：初始化一个条件变量

```c
int pthread_cond_init(pthread_cond_t *restrict cond, 
											const pthread_condattr_t *restrict attr);
		// 参 2：attr 表条件变量属性，通常为默认值，传 NULL 即可
```

**静态初始化和动态初始化**

```c
// 1 静态初始化
		pthread_cond_t cond=PTHREAD_COND_INITIALIZER
// 2 动态初始化
int pthread_cond_init(pthread_cond_t *restrict cond, 
											const pthread_condattr_t *restrict attr);
```

##### 6.3.2 pthread_cond_destroy 函数

**作用**：销毁一个条件变量

```c
int pthread_cond_destroy(pthread_cond_t *cond);
```

##### 6.3.3 pthread_cond_wait 函数（重）

作用：（非常重要 三点）

> 1 阻塞等待条件变量 cond（参 1）满足
>
> 2 释放已掌握的互斥锁（解锁互斥量）相当于 pthread_mutex_unlock(&mutex);
>
> 3 当被唤醒，pthread_cond_wait 函数返回时，解除阻塞并重新申请获取互斥锁 
>
> pthread_mutex_lock(&mutex);

```c
int pthread_cond_wait(pthread_cond_t *restrict cond, 
											pthread_mutex_t *restrict mutex);
```

> 1.2.两步为一个原子操作。

##### 6.3.4 pthread_cond_timedwait 函数

**作用**：限时等待一个条件变量

```c
int pthread_cond_timedwait(pthread_cond_t *restrict cond, 
													 pthread_mutex_t *restrict mutex, 
													 const struct timespec *restrict abstime);
```

##### 6.3.5 pthread_cond_signal 函数

**作用**：唤醒至少一个阻塞在条件变量上的线程

```c
int pthread_cond_signal(pthread_cond_t *cond);
```

##### 6.3.6 pthread_cond_broadcast 函数

**作用**：唤醒全部阻塞在条件变量上的线程

```c
int pthread_cond_broadcast(pthread_cond_t *cond);
```

![image-20211203104340402](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203104340402-1640860225833.png)

#### 6.4 生产者消费者模型

![image-20211203104452269](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203104452269-1640860233287.png)

```c
/*************************************************************************
 > File Name: conditionVar_product_consumer.c
 > Author: Winter
 > Created Time: 2021年11月23日 星期二 20时02分15秒
 ************************************************************************/

/* 借助条件变量模拟 生产者-消费者 问题 */

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

/* 链表作为共享数据，需要被互斥量保护 */
struct msg {
        struct msg* next;
        int num;
};

struct msg* head;         // 头节点

/* 静态初始化 一个条件变量 和 一个互斥量 */
pthread_cond_t has_product = PTHREAD_COND_INITIALIZER;
pthread_mutex_t lock = PTHREAD_MUTEX_INITIALIZER;

// 消费者
void* consumer(void* p) {
        struct msg* mp;

        for (;;) {
                pthread_mutex_lock(&lock);            // 加锁
                while (head == NULL) {                // 头节点为空，说明没有节点
                        pthread_cond_wait(&has_product, &lock);   // 消费者在阻塞
                }
                mp = head;
                head = head->next;                    // 模拟消费掉一个产品
                pthread_mutex_unlock(&lock);

                printf("--------Consume: %lu----: %d\n", pthread_self(), mp->num);
                free(mp);
                sleep(rand() % 5);
        }
}

// 生产者
void* producer(void* p) {
        struct msg* mp;

        for (; ; ) {
                mp = malloc(sizeof(struct msg));
                mp->num = rand() % 1000 + 1;        // 模拟生产一个产品
                printf("--Produce -------------------: %d\n", mp->num);

                pthread_mutex_lock(&lock);          // 加锁
                mp->next = head;                    // 头插法生产
                head = mp;
                pthread_mutex_unlock(&lock);        // 解锁

                pthread_cond_signal(&has_product);  // 将等待在该条件变量上的一个线程唤醒
                sleep(rand() % 5);
        }

}

int main(int argc, char* argv[])
{
        pthread_t pid, cid;
        srand(time(NULL));

        pthread_create(&pid, NULL, producer, NULL);     // 创建生产者线程
        pthread_create(&cid, NULL, consumer, NULL);     // 创建消费者线程

        pthread_join(pid, NULL);                        // 回收
        pthread_join(cid, NULL);                        // 回收
        return 0;
}
```

执行

![image-20211203104901028](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203104901028-1640860246690.png)

可以看到，消费者按序消费，毕竟是链表。

版本二

```c
/*************************************************************************
 > File Name: cond_produce_consumer.c
 > Author: Winter
 > Created Time: 2021年11月23日 星期二 20时44分17秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 打印输出错误信息
void err_thread(int res, char* str) {
        if (res != 0) {
                fprintf(stderr, "%s , %s\n", str, strerror(res));
                pthread_exit(NULL);
        }
}

// 链表
struct msg {
        int num;
        struct msg* next;
};


pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;     // 定义并且初始化互斥量
pthread_cond_t has_data = PTHREAD_COND_INITIALIZER;    // 定义并且初始化条件变量



struct msg* head;     // 头节点

// 生产者
void* produser(void* arg) {
        while (1) {
                struct msg* mp = malloc(sizeof(struct msg));
                mp->num = rand() % 1000 + 1;                // 模拟生产一个数据

                printf("----Producer : %d\n", mp->num);

                pthread_mutex_lock(&mutex);                  // 加锁
                mp->next = head;
                head = mp;                                   // 头插法
                pthread_mutex_unlock(&mutex);                  // 解锁

                pthread_cond_signal(&has_data);              // 唤醒阻塞在条件变量 has_data上的线程

                sleep(rand() % 3);
        }
        return NULL;
}


// 消费者
void* consumer(void* arg) {
        while (1) {
                struct msg* mp;
                pthread_mutex_lock(&mutex);                  // 加锁

                while (head == NULL) {
                        pthread_cond_wait(&has_data, &mutex);   // 阻塞等待条件变量
                }                                               // 返回时重新加锁

                mp = head;
                head = mp->next;

                pthread_mutex_unlock(&mutex);                  // 解锁

                printf("----Consumer : %d\n", mp->num);

                free(mp);
                sleep(rand() % 3);
        }
                return NULL;
}



int main(int argc, char* argv[])
{
        pthread_t tid, cid;
        int res;
        srand(time(NULL));                                     // 随机数种子

        res = pthread_create(&tid, NULL, produser, NULL);      // 创建生产者线程
        if (res != 0) {
                err_thread(res, "pthread_create producer error");
        }

        res = pthread_create(&cid, NULL, consumer, NULL);      // 创建消费者线程
        if (res != 0) {
                err_thread(res, "pthread_create consumer error");
        }

        pthread_join(tid, NULL);                               // 回收
        pthread_join(cid, NULL);                               // 回收

        return 0;
}
```

执行

![image-20211203105352520](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203105352520-1640860262500.png)

#### 6.5 多个消费者

用循环实现

这是最终版

```c
/*************************************************************************
 > File Name: cond_produce_consumer.c
 > Author: Winter
 > Created Time: 2021年11月23日 星期二 20时44分17秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>

// 打印输出错误信息
void err_thread(int res, char* str) {
        if (res != 0) {
                fprintf(stderr, "%s , %s\n", str, strerror(res));
                pthread_exit(NULL);
        }
}

// 链表
struct msg {
        int num;
        struct msg* next;
};


pthread_mutex_t mutex = PTHREAD_MUTEX_INITIALIZER;     // 定义并且初始化互斥量
pthread_cond_t has_data = PTHREAD_COND_INITIALIZER;    // 定义并且初始化条件变量



struct msg* head;     // 头节点

// 生产者
void* produser(void* arg) {
        while (1) {
                struct msg* mp = malloc(sizeof(struct msg));
                mp->num = rand() % 1000 + 1;                // 模拟生产一个数据

                printf("----Producer : %d\n", mp->num);

                pthread_mutex_lock(&mutex);                  // 加锁
                mp->next = head;
                head = mp;                                   // 头插法
                pthread_mutex_unlock(&mutex);                  // 解锁

                pthread_cond_signal(&has_data);              // 唤醒阻塞在条件变量 has_data上的线程

                sleep(rand() % 3);
        }
        return NULL;
}


// 消费者
void* consumer(void* arg) {
        while (1) {
                struct msg* mp;
                pthread_mutex_lock(&mutex);                  // 加锁

                while (head == NULL) {
                        pthread_cond_wait(&has_data, &mutex);   // 阻塞等待条件变量
                }                                               // 返回时重新加锁

                mp = head;
                head = mp->next;

                pthread_mutex_unlock(&mutex);                  // 解锁

                printf("----Consumer id = %lu -- %d\n", pthread_self(),  mp->num);

                free(mp);
                sleep(rand() % 3);
        }
                return NULL;
}



int main(int argc, char* argv[])
{
        pthread_t tid, cid;
        int res;
        srand(time(NULL));                                     // 随机数种子

        res = pthread_create(&tid, NULL, produser, NULL);      // 创建生产者线程
        if (res != 0) {
                err_thread(res, "pthread_create producer error");
        }
        // -------------------------------------3个消费者--------------------------------------
        res = pthread_create(&cid, NULL, consumer, NULL);      // 创建消费者线程
        if (res != 0) {
                err_thread(res, "pthread_create consumer error");
        }


        res = pthread_create(&cid, NULL, consumer, NULL);      // 创建消费者线程
        if (res != 0) {
                err_thread(res, "pthread_create consumer error");
        }

        res = pthread_create(&cid, NULL, consumer, NULL);      // 创建消费者线程
        if (res != 0) {
                err_thread(res, "pthread_create consumer error");
        }

        pthread_join(tid, NULL);                               // 回收
        pthread_join(cid, NULL);                               // 回收

        return 0;
}
```

**消费者阻塞在条件变量那里应该使用while循环。这样A消费完数据后，B做的第一件事不是去拿锁，而是判定**

**条件变量。**

```c
while (head == NULL) {
  pthread_cond_wait(&has_data, &mutex);   			// 阻塞等待条件变量
}                                               // 返回时重新加锁
```

#### 6.6 条件变量的优点

相较于 mutex 而言，条件变量可以减少竞争。  

如直接使用 mutex，除了生产者、消费者之间要竞争互斥量以外，消费者之间也需要竞争互斥量，但如果汇聚

（链表）中没有数据，消费者之间竞争互斥锁是无意义的。有了条件变量机制以后，只有生产者完成生产，才会引

起消费者之间的竞争。提高了程序效率。  

### 7 信号量

#### 7.1 基本概念

- 简单的说就是进化版的互斥锁（1~N）计数器

- 记录当前可利用的资源数，当资源数量<=0时会阻塞，当资源数量>时才开始进行操作，另外信号量的操作均

  为原子操作

由于互斥锁的粒度比较大，如果我们希望在多个线程间对某一对象的部分数据进行共享，使用互斥锁是没有办法实

现的，只能将整个数据对象锁住。这样虽然达到了多线程操作共享数据时保证数据正确性的目的，却无形中导致线

程的并发性下降。线程从并行执行，变成了串行执行。与直接使用单进程无异。

信号量，是相对折中的一种处理方式，既能保证同步，数据不混乱，又能提高线程并发。  

#### 7.2 函数使用

##### 7.2.1 sem_init 函数

**作用**：初始化一个信号量

```c
int sem_init(sem_t *sem, int pshared, unsigned int value);
				// 参 1：sem 信号量
				// 参 2：pshared 取 0 用于线程间；取非 0（一般为 1）用于进程间
				// 参 3：value 指定信号量初值
```

> 信号量的初值，决定了占用信号量的线程（进程）的个数。

##### 7.2.2 sem_destroy 函数

**作用**：销毁一个信号量

```c
int sem_destroy (sem_t *sem);
```

##### 7.2.3 sem_wait 函数

**作用**：给信号量加锁--

```c
int sem_wait(sem_t *sem);
```

##### 7.2.4 sem_post 函数

**作用**：给信号量解锁 ++

```c
int sem_post(sem_t *sem);
```

##### 7.2.5 sem_trywait 函数

**作用**：尝试对信号量加锁 – (与 sem_wait 的区别类比 lock 和 trylock)

```c
int sem_trywait(sem_t *sem);
```

##### 7.2.6 sem_timedwait 函数

**作用**：限时尝试对信号量加锁 --

```c
int sem_timedwait(sem_t *sem, const struct timespec *abs_timeout);
		// 参 2：abs_timeout 采用的是绝对时间。
```

#### 7.3 信号量实现的生产者消费者

![image-20211203111606219](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203111606219-1640860287387.png)

根据注释，代码还是比较好理解的。两个信号量来控制，一个初始为0，表示一开始生产的数量为0.另外一个初始

的数量是可以存放商品的空格数，初始化就是格子数N。

互斥和条件变量的方式是我消费 你不能生产。 而两个信号量是你生产 我可以消费。毕竟环形的 不影响，最多你

生产了，我不知道。就怕多个生产者 ，你格子放入了还没移到下一个格子我又重复放入了。

```c
/*************************************************************************
 > File Name: sem_producer_consumer.c
 > Author: Winter
 > Created Time: 2021年11月24日 星期三 20时28分11秒
 ************************************************************************/

#include<stdio.h>
#include<stdlib.h>
#include<string.h>
#include<unistd.h>
#include<pthread.h>
#include<semaphore.h>


#define NUM 5

int queue[NUM];                        // 全局数组实现环形队列
sem_t blank_number, product_number;    // 空格子信号量，产品子信号量


// 生产者
void* producer(void* arg) {
        int i = 0;

        while (1) {
                sem_wait(&blank_number);       // 生产者将空格数--，为0则阻塞等待
                queue[i] = rand() % 1000 + 1;  // 模拟生产一个产品
                printf("-------------Produce ----: %d\n", queue[i]);
                sem_post(&product_number);     // 将产品数++

                i = (i + 1) % NUM;
                sleep(rand() % 1);

        }
}



// 消费者
void* consumer(void* arg) {
        int i = 0;
        while (1) {
                sem_wait(&product_number);                 // 消费者将产品数--，为0则阻塞等待
                printf("Consumer : -----%d\n", queue[i]);  // 输出
                queue[i] = 0;                              // 消费一个产品
                sem_post(&blank_number);                   // 消费后，将空格数++

                i = (i + 1) % NUM;                         // 模拟环形队列
                sleep(rand() % 3);
        }
}

int main(int argc, char* argv[])
{
        pthread_t pid, cid;                // 生产者线程和消费者线程

        sem_init(&blank_number, 0, NUM);   // 初始化空格子信号量  信号量为5，线程间共享---0
        sem_init(&product_number, 0, 0);   // 初始化产品子信号量  产品数是0

        pthread_create(&pid, NULL, producer, NULL);    // 创建生产者线程
        pthread_create(&cid, NULL, consumer, NULL);    // 创建消费者线程

        pthread_join(pid, NULL);                       // 回收生产者线程
        pthread_join(cid, NULL);                       // 回收消费者线程

        sem_destroy(&blank_number);
        sem_destroy(&product_number);
        return 0;
}
```

执行

![image-20211203111726639](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211203111726639-1640860301063.png)

### 8 异步

异步处理就是,你现在问我问题,我可以不回答你,等我用时间了再处理你这个问题.同步不就反之了，

同步信息被立即处理 – 直到信息处理完成才返回消息句柄；异步信息收到后将在后台处理一段时间 – 而早在信息

处理结束前就返回消息句柄。

九、参考

[1] https://blog.csdn.net/weixin_44972997/article/details/115869213

[2] [黑马程序员-Linux网络编程_哔哩哔哩_bilibili](https://www.bilibili.com/video/BV1iJ411S7UA?spm_id_from=333.999.0.0)

评论区，好自理哥的笔记

![image-20211204095745827](linux%E7%B3%BB%E7%BB%9F%E7%BC%96%E7%A8%8B.assets/image-20211204095745827-1640860308796.png)

