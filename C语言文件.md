### 文件

[toc]



### 1.C语言中的标准流

| 文件指针 | 标准流       | 含义           |
| -------- | ------------ | -------------- |
| stdin    | 标准输入     | 键盘           |
| stdout   | 标准输出     | 终端显示器屏幕 |
| stderr   | 标准错误输出 | 终端显示器屏幕 |

`scanf()，getchar()，gets()` 等通过 `stdin` 获得输入

`printf()，putchar()，puts()` 等用 `stdout` 进行输出





---



### 2.按数据分类

#### 文本文件

C程序源代码

用字节表示字符的字符序列，存储每个字符的ASII码

按行划分，会有行结束符和文件结束符EOF



#### 二进制文件

可执行的C程序

按照数据在内存中的存储形式(二进制)存储到文件

二进制文件读写更快

必须按照数据存入的类型和格式读出才能恢复本来的面貌





---



### 3.文件与流的关系

C语言中流的打开和关闭是通过文件指针实现的



#### 文件指针

```c
FILE *fp;	//定义一个文件指针
```



#### fopen

使用 `fopen` 打开文件，也就是建立文件流



##### 函数原型和示例

```c
FILE *fopen(const char *filename, const char *mode);

FILE *fopen("D:\\001.txt", "r")    
```



##### 参数介绍

| 参数     | 描述                           |
| -------- | ------------------------------ |
| filename | 文件路径（相对路径和绝对路径） |
| mode     | 文件打开方式                   |



##### mode类型

带b的为二进制文件的打开方式

| 文件打开方式 | 含义 | 描述                                 |
| ------------ | ---- | ------------------------------------ |
| r / rb       | 只读 | 必须是已存在的文件                   |
| w / wb       | 只写 | 无论是否存在，都会新建一个文件       |
| a / ab       | 追加 | 文件必须存在，向文件末尾追加数据     |
| r+ / rb+     | 读写 | 文件必须存在，进行读写               |
| w+ / wb+     | 读写 | 新建文件，可读可写                   |
| a+ / ab+     | 读写 | 文件必须存在，向尾部追加数据，也可读 |



##### 返回值

返回指向此文件的指针 *fp ，如果打开失败，则返回NULL

文件打开后一定要检测是否打卡成功

```c
if (fp == NULL) {
    printf("文件打开失败\n");
    exit(0);
}
```



#### fclose

使用 `fclose` 关闭文件，也就是关闭文件流

把遗留在缓冲区中的数据写入文件，实施系统级的关闭操作

文件用完一定要关闭



##### 函数原型和示例

```c
int fclose(FILE *fp);

fclose(fp);
```



##### 返回值

成功执行关闭操作返回0，若关闭有错误则返回非零值



### 3.按格式读写文件

#### fscanf

fscanf为按格式读文件

##### 

##### 函数原型及示例

```c
int fsacnf(FILE *stream, consnt char* format, [argument...]);

fscanf(fp, "%d", &num);
```



##### 参数介绍

stream 为标准流，如果使用 `stdin` ，则和 `scanf` 相同

```c
fscanf(stdin, "%d", &num);
```



##### vs消除警告

```c
#define _CRT_SECURE_NO_WARNINGS	//用于消除vs中的警告
```





#### fprintf

fprintf我按格式写文件



##### 函数原型及示例

```c
int fprintf(FILE *stream, consnt char* format, [argument...]);

fprintf(fp, "%d", num);
```



##### 参数介绍

stream 为标准流，如果使用 `stdout` ，则和 `printf` 相同

```c
fprintf(stdout, "%d", num);
```



#### fseek

fseek函数可以重定位流(数据流/文件)上的**文件内部位置指针**，而不是定位文件指针，文件指针指向文件/流，文件指针如果不重新赋值将不会改变指向别的文件

位置指针为指向文件内部的字节位置，随着文件的读取会移动



##### 函数原型及示例

```c
int fseek(FILE *stream, long offset, int fromwhere);

fseek(fp, 0L, SEEK_SET);
```



##### 参数介绍

| 参数      | 描述         |
| --------- | ------------ |
| stream    | 文件指针     |
| offset    | 偏移量，字节 |
| fromwhere | 从何处偏移   |



`fromwhere` 的一些常量

| 常量     | 值   | 含义                                        |
| -------- | ---- | ------------------------------------------- |
| SEEK_SET | 0    | 参数offset 即为新的读写位置                 |
| SEEK_CUR | 1    | 以目前的读写位置往后增加offset 个位移量.    |
| SEEK_END | 2    | 将读写位置指向文件尾后再增加offset 个位移量 |



#### 小示例

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc, char* argv[]) {
	FILE* fp  = fopen("001.txt","w+");
	int num;
	if (fp == NULL) {
		printf("文件打开失败\n");
		exit(0);
	}
	fprintf(fp, "%d", 20);
	fseek(fp, 0L, SEEK_SET);
	fscanf(fp, "%d", &num);	
	printf("num = %d\n", num);
	return 0;
}
```





---



### 4.按字符读写文件

#### getc 和 putc

getc 和 putc 函数是从任意流读写字符



##### 函数原型

```c
int getc(FILE *fp);
int putc(int c,FILE *fp);
```

putchar 和 getchar 其实是宏定义

```c
#define putchar(c) putc(c,stdouut)
#define getchar(c) getc(stdin)
```



#### fgetc 和 fputc

fgetc 和 fputc 其实和 getc 和 putc 作用是一样的，但常用于读写文件



##### 函数原型

```c
int fgetc(FILE *fp);	
int fputc(int c,FILE *fp);
```



##### 作用及返回值

| fgetc                                      | fputc               |
| ------------------------------------------ | ------------------- |
| 从fp读出一个字符，将位置指针指向下一个字符 | 向fp写入字符        |
| 若读成功，则返回该字符                     | 写成功，返回c       |
| 读到文件尾或读取错误，返回EOF，值为-1      | 写入错误，则返回EOF |



#### feof

feof 函数用于检测文件是否到达文件尾

检测的文件尾会返回一个非零值，不是文件尾返回零值



##### 函数原型及示例

```c
int feof(FILE* stream);

feof(fp);
```



#### ftell

ftell 函数用于返回当前文件指针的位置

这个位置是当前文件指针相对于文件开头的位移量

偏移量从0计数，单位为字节，调用读入函数后，文件指针自动移到读入的最后一个字符的下一个位置



##### 函数原型及示例

```c
long ftell(FILE *fp);

ftell(fp);
```



#### ferror

ferror 用于判断文件是否发生读写错误，出错返回非零值（真值），否则返回0



##### 函数原型及示例

```c
int ferror(FILE *fp);

ferror(fp);
```



#### 小示例

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc, char* argv[]) {
	FILE* fp  = fopen("001.txt","w+");
	int ch;
	if (fp == NULL) {
		printf("文件打开失败\n");
		exit(0);
	}
	ch = getchar();
	while(ch != '\n') {
		fputc(ch, fp);
		ch = getchar();
	}
	printf("当前文件指针的位置为：%d\n", ftell(fp));
	fseek(fp, 0L, SEEK_SET);
	ch = fgetc(fp);
	while (!feof(fp)) {
		putchar(ch);
		ch = fgetc(fp);
	}
	return 0;
}
```





---



### 5.按行读写文件

#### fputs

fputs 函数将字符串s写入fp所指的文件中

不会自动添加换行符，除非字符串本身含有换行符



##### 函数原型及示例

```c
int fputs(const char *s,FILE *fp);

fputs("hello world", fp);
```



##### 返回值

若写入错误，则返回EOF，否则返回一个非负数



#### fgets

从fp所指向的文件中读取字符串，最多读取n-1个字符

当读到回车换行符、文件末尾，或读满n-1个字符时，在字符串末尾添加'\0'，返回改字符串的首地址

fgets保留字符串中的'\n'



##### 函数原型及示例

```c
char *fgets(char *s, int n, FILE *fp);

```



##### 参数介绍

| 参数 | 描述                    |
| ---- | ----------------------- |
| s    | 读取的数据赋值给字符串s |
| n    | 读取的字节数            |
| fp   | 输入流                  |



##### 返回值

读取成功返回返回改字符串的首地址，否则返回NULL



#### 小示例

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc, char* argv[]) {
	FILE* fp  = fopen("001.txt","w+");
	int ch;
	char chs[20];
	if (fp == NULL) {
		printf("文件打开失败\n");
		exit(0);
	}
	fputs("hello world!", fp);
	printf("当前文件指针的位置为：%d\n", ftell(fp));
	fseek(fp, 0L, SEEK_SET);
	fgets(chs, 20, fp);
	printf("\nchs = %s\n", chs);	
	fclose(fp);
	return 0;
}
```





---



### 6.按数据块读取文件

#### fread

fread 函数用于按数据块读文件

从 fp 所指向的文件中读取数据块存储到 buffer 指向的内存中



##### 函数原型及示例

```c
typedef unsigned int     size_t;
site_t fread(void *buffer, size_t site, site_t count, FILE *stream);

int num =  fread(array, sizeof(int), 10, fp);
```



##### 参数介绍

| 参数   | 描述                                 |
| ------ | ------------------------------------ |
| buffer | 存储数据块的内存首地址，如内存首地址 |
| site   | 每个数据块的大小，一般用 sizeof 表示 |
| count  | 要读入的数据块数量                   |
| steram | 所指文件                             |



##### 返回值

返回值为实例读入的数据块个数，应等于count，除非读到了文件尾部，或者读取错误



#### fwrite

fwrite 为按数据块写文件

将 buffer 指向的内存中的数据写入 fp 所指的文件中



##### 函数原型及示例

```c
typedef unsigned int     size_t;
site_t fwrite(void *buffer, size_t site, site_t count, FILE *stream);


```



##### 参数介绍

| 参数   | 描述                                 |
| ------ | ------------------------------------ |
| buffer | 存储数据块的内存首地址，如内存首地址 |
| site   | 每个数据块的大小，一般用 sizeof 表示 |
| count  | 要读入的数据块数量                   |
| steram | 所指文件                             |



##### 返回值

返回值为实例写入的数据块个数，应等于count，除非出现写入错误



#### 小示例

```c
#include<stdio.h>
#include<stdlib.h>

int main(int argc, char* argv[]) {
	FILE* fp  = fopen("001.txt","w+");
	if (fp == NULL) {
		printf("文件打开失败\n");
		exit(0);
	}
	int writeNum = fwrite("hello world", sizeof(char), 12, fp);
	printf("writeNum = %d\n", writeNum);
	fclose(fp);
	fp = fopen("001.txt", "r+");
	if (fp == NULL) {
		printf("文件打开失败\n");
		exit(0);
	}
	char array[12];
	int readNum = fread(array, sizeof(char), 12, fp);
	printf("readNum = %d\n", readNum);
	printf("array = %s\n", array);
	fclose(fp);
	return 0;
}
```





---



### 7.文件的随机读写

#### 文件定位

##### rewind

rewind 函数使文件位置指针重新指向文件的开始位置

```c
void rewind(FILE *fp);
```



##### ftell

返回当前文件位置的指针（相对于文件起始位置的字节偏移量）

```c
long ftell(FILE *fp);
```



##### fseek

改变文件位置指针，实现随机读写

```c
int fseek(FILE *fp, long offset, int fromwhere);
```



#### 文件缓冲

##### 输出缓冲

- 写入文件的数据，实际是存储在内存的缓冲区内
- 当缓冲区满或者关闭文件时，缓冲区自动 “清洗”（写入输出设备）
- 主动清洗缓冲区，用 fflush(fp);

##### 输入缓冲

- 来自输入设备的数据存在输入缓冲区内
- 从缓冲区读数据代替了从设备本身读数据



