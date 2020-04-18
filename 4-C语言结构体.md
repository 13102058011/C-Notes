结构体
	1.结构体模板
		1.示例
			struct student {
				long student_id;
				char student_name[10];
				char student_sex;
			};

		2.说明
			结构体模板没有定义结构体变量，只是声明了一个模板
	
	2.定义结构体变量
		1.先定义模板在定义变量名
			1.示例
				struct student {
					long student_id;
					char student_name[10];
					char student_sex;
				};	
				struct student stu1;
	
			2.说明
				struct 和 student 都不可省略


		2.定义模板的同时定义变量
			1.示例
				struct student {
					long student_id;
					char student_name[10];
					char student_sex;
				}stu;
	
			2.说明
				必须有分号
	
		3.直接定义结构体变量
			1.示例
				struct {
					long student_id;
					char student_name[10];
					char student_sex;
				}stu;
		
			2.说明
				相当于一次性
	
	3.使用typedef起别名
		1.单独起别名
			typedef struct student STUDENT;
	
		2.直接起别名
			typedef struct student {
				long student_id;
				char student_name[10];
				char student_sex;
			}STUDENT;
	
		3.可以省略结构体标签
			typedef struct {
				long student_id;
				char student_name[10];
				char student_sex;
			}STUDENT;
	
	4.结构体的初始化
		1.示例
		    STUDENT stu2 = { 123,"TonyDon",'b' };
	
		2.说明
			1. 定义时初始化
			2. 赋值顺序需要一致
	
	5.结构体的嵌套
		1. 结构体之间可以嵌套
			1.示例
				typedef struct {
					int year;
					int month;
					int day;
				}DATE;
				typedef struct student {
					long student_id;
					char student_name[10];
					char student_sex;
					DATE birthday;
				}STUDENT;
	
			2.嵌套结构体的初始化
				STUDENT stu2 = { 123,"TonyDon",'b',{2020,3,13} };
	
		2.结构体与数组可以嵌套
			1.示例
				STUDENT stu[30] = {
					{123,"TonyDon",'b',{2020,3,13}},
					{123,"TonyDon",'g',{2020,3,13}}
				};
	
	6.结构体大小
		1.示例
			typedef struct {
				char a;
				char b;
				int c;
			}SAMPLE;
			printf("length = %d\n", sizeof(a));//8
	
		2.说明
			1. 在计算机中有内存对齐，4个字节为一组
			2. 不同编译器和系统内存对齐方式可能不同
	
	7.访问结构体成员
		1.示例
			STUDENT stu = { 1911,"TonyDon",'b',{2001,3,5} };
			printf("id = %d\n",stu.student_id);		
	
		2.说明
			1. 成员选择运算符(圆点运算符)
			2. 结构体变量名.成员名
	
	8.结构体指针
		1.说明
			结构体指针就是指向结构体的指针
	
		2.示例
			STUDENT stu;
			STUDENT* pt = &stu;
	
		3.通过结构体指针访问结构体成员
			1.解引用
				(*pt).student_name
				//括号不可省略
	
			2.通过指向运算符
				pt -> student_name
	
			3.访问结构体嵌套成员
				pt->birthday.day
				(*pt).birthday.day
	
	9.指向结构体数组的指针
		STUDENT stu[30];
		STUDENT* pt = stu;
	
	10.向函数传递结构体
		1.向函数传递结构体的单个成员
	
		2.向函数传递结构体的完整结构
			1.说明
				复制结构体的所有成员，占用大量空间，不推荐使用
				在函数内对结构体内容修改不会影响实参
	
			2.示例
				void pri(STUDENT s) {
					printf("id = %d\n", s.student_id);
				}
	
		3.向函数传递结构体的首地址
			1.说明
				只复制一个指针变量
				在函数内部会修改结构体的实际值
			
			2.示例
				STUDENT s = { 001,"hello",'b',2001,3,5 };
				STUDENT* pt = &s;
				pri(pt);
				void pri(STUDENT* ps) {
					printf("id = %d\n", ps->student_id);
				}

---------------------------------------------------------------------------------------------------
枚举类型
	1.声明
		enum weeks {SUN=7,MON=7,TUE};
		enum weeks today;

		typedef enum weeks {SUN=7,MON=7,TUE} WEEKS;
		WEEKS today;
	2.说明
		1. 可以在声明同时赋值
		2. 前面最后一个赋值的变量 后面的变量依次自增
		3. weeks为枚举标签
		4. 枚举类型是一种基本数据类型
		5. 枚举常量为整型数据
	
	3.枚举类型的用途
		1. 增强程序的可读性
		2. 定义布尔类型

---------------------------------------------------------------------------------------------------
共用体
	1.示例
		union sample {
			int a;
			char b;
		};

	2.共用体所占字节数
		1.示例
			printf("length = %d\n", sizeof(sam)); //4
	
		2.说明
			共用体所占字节数取决于占空间最多的那个成员变量
	
	3.使用共用体
		1.示例
				union sample sam = {2};
				printf("a = %d\n", sam.a);
				sam.b = 'd';
				printf("b = %c\n", sam.b);
	
		2.说明 
			1. 与结构体一样，sam.a;
			2. 每一瞬时只能保存一个成员，起作用的是最后一次赋值的成员
			3. 共用体只能给第一个成员初始化，{}不可省略
	
	4.共用体的应用
		构造混合的数组
			1.示例
				typedef union {
					int i;
					float f;
				}NUMBER;
				NUMBER array[10];

---------------------------------------------------------------------------------------------------
动态内存分配函数
	1.堆和栈
		1.栈用来存储函数，内存地址从高到低
		2.堆的内存使用由程序员决定，内存地址由下向上

	2.动态内存分配函数
		1.malloc
			1.函数原型
				void *malloc(unsigned int size);
	
			2.说明
				1. 向系统申请大小为 size 的内存块，返回首地址
				2. 若申请不成功则返回NULL
	
		2.calloc
			1.函数原型
				void *calloc(unsigned int num, unsigned int size);
	
			2.说明
				1. 向系统申请 num 个大小为 size 的内存块，返回首地址
				2. 若申请不成功则返回NULL
			
		3.realloc
			1.函数原型
				void* realloc(void* p, unsigned int size);
	
			2.说明
				1. 将指针p所指向的存储空间的大小改为 size 个字节
				2. 返回新分配的存储空间的首地址
				3. 与原来的首地址不一定相同
	
		4.free
			1.函数原型
				void free(void* p);
	
			2.说明
				1.释放申请的内存块
			
		3.说明
			1. void * 是一种空类型的指针，可以指向任意类型的变量，使用时需要强制类型转换
	
	3.动态数组