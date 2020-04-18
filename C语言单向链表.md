本文将介绍C语言中单向链表的创建

[TOC]



## 1. 单向链表---定义

### 1. 链表是什么

链表是线性表的存储结构。什么意思呢？搞这么多专业术语跟念经一样，其实它也不难，大家都见过自行车的链条，它是由一节一节连起来的，最后形成一个圈。像这种一个圈的链表，就是所谓的循环链表。不过我们这节讲的是单向链表，通俗一点就是将自行车链条剪断，这时候就是一条链子了，也就是单向链表。每一节就是所谓的节点，第一个节点连着第二个节点，第二个节点连着第三个节点，以此类推，最后一个节点没有东西可以连了，也就不连了。



### 2. 节点是什么

节点是一个结构体的形式，我们知道结构体可以同时具有多个不同类型的变量，那么我在结构体中定义一个指向下一节点的指针，也就是指向下一个结构体的指针，以此类推，不就把所有的节点连接在一起了吗，如下

```c
typedef struct hero {
    int attack;
    int HP;
    int MP;
    char name[20];			//数据域：存储英雄的数据
    struct hero* p_next;	//指针域：存储下一个节点的地址
}HERO;
```



### 3. 头节点和尾节点

头节点顾名思义就是指向第一个节点的指针，那么有的小伙伴可能想，那我直接用第一个节点的地址作为头节点不就行了吗？理论上可以的，但是呢，之后你就会发现没有头指针，链表操作起来就比较繁琐，所以，头指针还是有必要的。

尾节点就是最后一个节点，在它后面没有下一个节点了，所以它的p_next = NULL;

```c
HERO hero1 = {68, 550, 0, };
HERO hero2;
HERO *p_head = hero1;	//头节点
hero1.p_next = &hero2;
hero2 = NULL:			//尾节点
```



### 4.建立一个单向链表

我们已经知道了单向链表的原理，现在就可以建立一个单向链表了

```c
//创建节点
HERO hero1 = { 68, 550, 0 ,"盖伦" };
HERO hero2 = { 78, 550, 0 ,"亚索" };
HERO hero3 = { 78, 450, 200 ,"艾希" };

//链接起来
hero1.p_next = &hero2;
hero2.p_next = &hero3;

//头节点
HERO* p_head = &hero1;

//尾节点
hero3.p_next = NULL;

printf("盖伦的攻击力为：%d\n", p_head->attack);	//68
```



### 5. 数据丢失

上面我们成功创建了一个链表，如果将这么多代码全都放在main函数里面就会显得很复杂，所以我们把它封装到函数里面，如下

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

typedef struct hero {
	int attack;
	int HP;
	int MP;
	char name[20];			//数据域：存储英雄的数据
	struct hero* p_next;	//指针域：存储下一个节点的地址
}HERO;

HERO* buide() {
	//创建节点
	HERO hero1 = { 68, 550, 0 ,"盖伦" };
	HERO hero2 = { 78, 550, 0 ,"亚索" };
	HERO hero3 = { 78, 450, 200 ,"艾希" };

	//链接起来
	hero1.p_next = &hero2;
	hero2.p_next = &hero3;

	//头节点
	HERO* p_head = &hero1;

	//尾节点
	hero3.p_next = NULL;

	return p_head;
}

int main() {
	HERO* p_head = buide();
	printf("hero1的姓名为：%s\n", p_head->name);

    //输出
	//hero1姓名为：烫烫(?烫烫烫烫烫烫烫烫??
    
	return 0;
}
```

哎？为什么姓名会输出乱码呢？其实我们进入了一个误区，函数里面声明的变量是存储在栈中的，也就是局部变量，什么意思呢？局部变量的生存期是伴随函数的，通俗的讲，就是buide函数一结束，你声明的"盖伦"，"亚索"，它们都没了。

所以我们用局部变量创建链表是不行的，那怎么办呢？我们学过动态内存分配函数malloc()，它申请的内存空间在堆中，由程序猿执掌生死。这样不就能治住它了吗

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

typedef struct hero {
	int attack;
	int HP;
	int MP;
	char name[20];			//数据域：存储英雄的数据
	struct hero* p_next;	//指针域：存储下一个节点的地址
}HERO;

HERO* buide() {
	//申请空间
	HERO* hero1 = (HERO*)malloc(sizeof(HERO));
	HERO* hero2 = (HERO*)malloc(sizeof(HERO));
	HERO* hero3 = (HERO*)malloc(sizeof(HERO));
    
    //需要判断hero1内存空间有没有分配成功
	if (hero1 != NULL) {
		hero1->attack = 68;
		hero1->HP = 550;
		hero1->MP = 0;
		strcpy(hero1->name, "盖伦");
	}
	
    ····
	
	HERO* p_head = hero1;
	hero1->p_next = hero2;
	hero2->p_next = hero3;
	hero3->p_next = NULL;

	return p_head;
}

int main() {
	HERO* p_head;
	p_head= buide();
	printf("hero1的姓名为：%s\n", p_head->name);
	printf("hero1的生命值为：%d\n", p_head->HP);
	return 0;
}

//输出
//hero1的姓名为：盖伦
//hero1的生命值为：550
```



### 6. 建立一个100个节点的链表

建立一个有100个节点的链表，特殊的地方为链表的头节点，所以我们要单独的创健一个头节点，之后再循环创建99个节点

```c
#include<stdio.h>
#include<string.h>
#include<stdlib.h>

typedef struct hero {
	int attack;
	int HP;
	int MP;
	char name[20];			//数据域：存储英雄的数据
	struct hero* p_next;	//指针域：存储下一个节点的地址
}HERO;

HERO* buide();

int main() {
	HERO* p_head;
	p_head = buide();
	return 0;
}

HERO* buide() {
	HERO* p_head;	//头节点
	HERO* p_new;	//新节点
	HERO* p_last;	//尾节点

	//创建第一个节点
	p_new = (HERO*)malloc(sizeof(HERO));
	if (p_new != NULL) {
		printf("name = ");
		scanf("%s", p_new->name);
		printf("HP = ");
		scanf("%d", &p_new->HP);
		printf("MP = ");
		scanf("%d", &p_new->MP);
		printf("attack = ");
		scanf("%d", &p_new->attack);

		//头节点和尾节点均指向第一个节点
		p_head = p_new;
		p_last = p_new;
	}
	else {
		printf("创建链表失败！\n");
		exit(0);
	}

	//循环创建99个节点
	for (int i = 1; i < 100; i++) {
		p_new = (HERO*)malloc(sizeof(HERO));
		if (p_new != NULL) {
			printf("name = ");
			scanf("%s", p_new->name);
			printf("HP = ");
			scanf("%d", &p_new->HP);
			printf("MP = ");
			scanf("%d", &p_new->MP);
			printf("attack = ");
			scanf("%d", &p_new->attack);

			p_last->p_next = p_new;	//尾节点的指针域指向新节点
			p_last = p_new;			//尾节点更新为新节点
		}
		else {
			printf("创建链表失败！\n");
			exit(0);
		}
	}
	p_last->p_next = NULL;	//尾节点的指针域赋值为NULL;
	return p_head;
}
```



### 7. 查找尾节点

有时候我们要向一个链表中再插入元素，这时候就需要找到该链表的尾节点。通俗的说就是又出新英雄了，我们需要找到最后一个英雄，把新英雄接上去。













