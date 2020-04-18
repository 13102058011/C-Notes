

#### 函数细节


| 函数             | 描述                  |
| ---------------- | --------------------- |
| void swap();     | 系统默认有两个int参数 |
| void swap(void); | 没有任何参数          |



#### 命令行参数

在GUI界面出现之前，计算机的操作界面都是字符式的命令行界面，如下

```c
gcc hello.c		//编译c文件
hello.exe		//运行文件
```

命令行参数可以决定程序干什么，怎么干，命令行参数由main函数接受

int main(int argc, char*argv[])

| 参数                 | 描述                             |
| -------------------- | :------------------------------- |
| argc                 | 命令行参数的数量(包括程序名本身) |
| argv                 | 指向命令行参数的指针数组         |
| argv[0]              | 为指向程序名的字符指针           |
| argv[1]~argv[argc-1] | 为指向余下的命令行参数的字符指针 |

示例

```c
prg.exe hello world <回车>
prg hello world <回车>
```

这里注意回车并不算一个命令行参数，此时 argc = 3，此外 prg.exe 和 prg 是一样的，都是程序名，都会被算作命令行参数

#### 存储单位

|   单位   | 英文字符 | 进制 |
| :------: | :------: | :--: |
|   比特   |    b     |  1   |
|   字节   |    B     |  8   |
|  千字节  |    KB    | 1024 |
|  兆字节  |    MB    | 1024 |
| 千兆字节 |    GB    | 1024 |
|  太字节  |    TB    | 1024 |
|  拍字节  |    PB    | 1024 |
|  艾字节  |    EB    | 1024 |
|  泽字节  |    ZB    | 1024 |
|  尧字节  |    YB    | 1024 |



#### printf和scanf返回值的作用

printf() 和 scanf() 函数都有返回值，我们平时都是忽略返回值的，那返回值有什么意义呢

printf函数的返回值类型是int，返回值是输出的字符个数

scanf函数的返回值类型也是int，代表正确输入的个数

有的同学开发工具用的是visual studio，所以可能使用的是scanf_s()，不用担心，scanf_s()的返回值等同于scanf()的返回值

```c
int integer1 = 100;
int integer2 = 1000;
double decimal1 = 1.0;
double decimal2 = 12.0;
int a, b;

int printfBack0 = printf("\n");					//输出 换行
int printfBack1 = printf("%d\n", integer1);		//输出 100+换行
int printfBack2 = printf("%d\n", integer2);		//输出 1000+换行
int printfBack3 = printf("%lf\n", decimal1);	//输出 1.000000+换行
int printfBack4 = printf("%lf\n", decimal2);	//输出 12.000000+换行
int printfBack5 = printf("汉字\n");			   //输出 汉字+换行

printf("printfBack0 = %d\n", printfBack0);	//输出1，\n只是一个字符
printf("printfBack1 = %d\n", printfBack1);	//输出4
printf("printfBack2 = %d\n", printfBack2);	//输出5
printf("printfBack3 = %d\n", printfBack3);	//输出9，1.000000+换行一共是9个字符
printf("printfBack4 = %d\n", printfBack4);	//输出10
printf("printfBack5 = %d\n", printfBack5);	//输出5，一个汉字占两个字符

int scanfBack0 = scanf("%d", &a);			//若正确输入则返回1
int scanfBack1 = scanf("%d%d", &a, &b);	    //若都正确输入则返回2

printf("scanfBack0 = %d\n", scanfBack0);	//如果进行了错误输入，例如输入一个字符a，则scanfBack0的返回值是0
printf("scanfBack1 = %d\n", scanfBack1);	//如果进行了一个错误输入，例如输入：10 a，则scanfBack1的返回值是1

scanf("%d\n", &a);
//这里插入一个scanf容易犯的错误
//此时你打一个数字（例：10）后，无论按多少个回车都没有用
//不是你错了，而是scanf中要求格式控制要与输入格式保持一致
//你需要在打一个\n上去，所以scanf里面\n可不是回车的意思
```







**	以下结论均在vs，c11+下执行
*/

int main()
{

	float a;
	double b;
	long double c;
	
	scanf_s("%f", &a);		//首先是float，输入只能使用%f, %lf会出现堆栈损坏的问题
							//scanf_s("%lf", &a); 禁止！
	
	scanf_s("%lf", &b);		//然后是double，输入需要使用%lf, %f不会报错，但输出会有问题(随机数)
							//scanf_s("%f", &a); 禁止！
	
	scanf_s("%lf", &c);		//最后是long double，有的同学说，既然float是%f，double是%lf,那么long double是%llf
							//绝对不可以，vs下编译通过，但运行时程序会崩溃，老老实实使用%lf就可以了
							//scanf_s("%llf", &c); 禁止！
	
	//输出就比较宽松了，亲测对于float，double，long double三种类型，%f,%lf,%llf,都可以使用
	//那有的同学会想，%lllf可以吗，也不可以，这在vs下一样是编译通过，运行时程序会崩溃
	printf("a = %llf\n", a);
	printf("b = %llf\n", b);
	printf("c = %llf\n", c);	
	
	//printf("c = %lllf\n", c); 禁止！
	
	return 0;
}

/*
附加笔记

if(!a--)	先执行!a, 后a--

if(!--a)	先执行--a, 后!a

if(a, b)	语法正确，判断最右边的b

while(1);	死循环

*/**





1. 头文件 
	#include<stdlib.h>
	#include<time.h>

2. 为rand函数设置随机种子
	srand((unsigned)time(NULL));
	
3. 获取随机值
	rand();



