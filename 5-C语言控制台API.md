### 获取句柄API

#### GetStdHandle

返回标准的输入、输出或错误的设备的句柄，也就是获得输入、输出 /错误的屏幕缓冲区的句柄。



##### 函数原型

```c
HANDLE WINAPI GetStdHandle(
  _In_ DWORD nStdHandle
);
```





##### 参数介绍

| 参数       | 描述                 |
| ---------- | -------------------- |
| DWORD      | unsigned long        |
| nStdHandle | The standard device. |

nStdHandle

| Value             | Meaning                     |
| ----------------- | --------------------------- |
| STD_INPUT_HANDLE  | The standard input device.  |
| STD_OUTPUT_HANDLE | The standard output device. |
| STD_ERROR_HANDLE  | The standard error device.  |



##### 头文件

```
ProcessEnv.h (via Winbase.h, include Windows.h)
```



### 控制台窗口信息结构体

```c
typedef struct _CONSOLE_SCREEN_BUFFER_INFO { 

COORD dwSize; // 缓冲区大小

COORD dwCursorPosition; // 当前光标位置

WORD wAttributes; // 字符属性

SMALL_RECT srWindow; // 当前窗口显示的大小和位置

COORD dwMaximumWindowSize; // 最大的窗口缓冲区大小

} CONSOLE_SCREEN_BUFFER_INFO ;
```



### 控制台窗口操作

#### 控制台窗口操作的API函数

| 函数                       | 描述                 |
| -------------------------- | -------------------- |
| GetConsoleScreenBufferInfo | 获取控制台窗口信息   |
| GetConsoleTitle            | 获取控制台窗口标题   |
| ScrollConsoleScreenBuffer  | 在缓冲区中移动数据块 |
| SetConsoleScreenBufferSize | 更改指定缓冲区大小   |
| SetConsoleTitle            | 设置控制台窗口标题   |
| SetConsoleWindowInfo       | 设置控制台窗口信息   |



#### GetConsoleScreenBufferInfo

检索有关指定控制台屏幕缓冲区的信息



##### 函数原型

```c
BOOL WINAPI GetConsoleScreenBufferInfo(
  _In_  HANDLE                      hConsoleOutput,
  _Out_ PCONSOLE_SCREEN_BUFFER_INFO lpConsoleScreenBufferInfo
);
```



##### 参数介绍

| 参数                      | 描述                                |
| ------------------------- | ----------------------------------- |
| hConsoleOutput            | STD_OUTPUT_HANDLE，标准输出设备句柄 |
| lpConsoleScreenBufferInfo | ConsoleScreenBufferInfo指针         |



ConsoleScreenBufferInfo 是窗口缓冲区信息结构体

结构体原型

```c
typedef struct _CONSOLE_SCREEN_BUFFER_INFO { 

COORD dwSize; 				// 缓冲区大小

COORD dwCursorPosition; 	// 当前光标位置

WORD wAttributes; 			// 字符属性
	
SMALL_RECT srWindow; 		// 当前窗口显示的大小和位置

COORD dwMaximumWindowSize; 	// 最大的窗口缓冲区大小

} CONSOLE_SCREEN_BUFFER_INFO ;
```



##### 返回值

```
If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero.
```



##### 头文件

```
ConsoleApi2.h (via Wincon.h, include Windows.h)
```





#### GetConsoleTitle

获取控制台窗口标题



##### 函数原型

```c
DWORD WINAPI GetConsoleTitle(
  _Out_ LPTSTR lpConsoleTitle,
  _In_  DWORD  nSize
);
```



##### Parameters

*lpConsoleTitle* [out]
A pointer to a buffer that receives a null-terminated string containing the title. If the buffer is too small to store the title, the function stores as many characters of the title as will fit in the buffer, ending with a null terminator.

*nSize* [in]
The size of the buffer pointed to by the *lpConsoleTitle* parameter, in characters.

##### Return value

If the function succeeds, the return value is the length of the console window's title, in characters.

If the function fails, the return value is zero and [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) returns the error code.

##### 头文件

```
ConsoleApi2.h (via Wincon.h, include Windows.h)
```



#### SetConsoleTitle

The string to be displayed in the title bar of the console window. The total size must be less than 64K.



##### 函数原型

```c
BOOL WINAPI SetConsoleTitle(
  _In_ LPCTSTR lpConsoleTitle
);
```



##### 返回值

```
If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero. 
```





#### SetConsoleTextAttribute

##### 函数原型

```c
BOOL WINAPI SetConsoleTextAttribute(
  _In_ HANDLE hConsoleOutput,	// 使用GetStdHandle取得的句柄
  _In_ WORD   wAttributes		// 设置文本样式
);
```



##### 文本样式表

| Attribute                      | Meaning                                       |
| :----------------------------- | :-------------------------------------------- |
| FOREGROUND_BLUE                | Text color contains blue.                     |
| FOREGROUND_GREEN               | Text color contains green.                    |
| FOREGROUND_RED                 | Text color contains red.                      |
| FOREGROUND_INTENSITY           | Text color is intensified.                    |
| **BACKGROUND_BLUE**            | Background color contains blue.               |
| **BACKGROUND_GREEN**           | Background color contains green.              |
| **BACKGROUND_RED**             | Background color contains red.                |
| **BACKGROUND_INTENSITY**       | Background color is intensified.              |
| **COMMON_LVB_LEADING_BYTE**    | Leading byte.                                 |
| **COMMON_LVB_TRAILING_BYTE**   | Trailing byte.                                |
| **COMMON_LVB_GRID_HORIZONTAL** | Top horizontal.                               |
| **COMMON_LVB_GRID_LVERTICAL**  | Left vertical.                                |
| **COMMON_LVB_GRID_RVERTICAL**  | Right vertical.                               |
| **COMMON_LVB_REVERSE_VIDEO**   | Reverse foreground and background attributes. |
| **COMMON_LVB_UNDERSCORE**      | Underscore.                                   |



**官方文档**

```c
https://docs.microsoft.com/en-us/windows/console/console-screen-buffers#_win32_font_attributes
```



##### 返回值

```
If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero.
```











> **SetConsoleCursorInfo设置光标的信息**
>
> BOOL SetConsoleCursorInfo(
>
> HANDLE hConsoleOutput,
>
> // 使用GetStdHandle取得的句柄
>
> CONST CONSOLE_CURSOR_INFO *lpConsoleCursorInfo 
>
> // 光标信息
>
> );

> **SetConsoleCursorPosition设置光标位置**
>
> BOOL SetConsoleCursorPosition(
>
>  HANDLE hConsoleOutput,
>
>  COORD dwCursorPosition
>
> );

> **SetConsoleTitle设置控制台的标题**
>
> BOOL SetConsoleTitle(
>
>  LPCTSTR lpConsoleTitle
>
> );

> **SetConsoleActiveScreenBuffer设置活动屏幕缓冲区**
>
> BOOL SetConsoleActiveScreenBuffer( 
>
>   HANDLE hConsoleOutput 
>
> );





#### 获取属性API

> **GetConsoleScreenBufferInfo取得控制台屏幕缓冲信息**
>
> BOOL GetConsoleScreenBufferInfo(
>
>  HANDLE hConsoleOutput,
>
>  PCONSOLE_SCREEN_BUFFER_INFO lpConsoleScreenBufferInfo
>
> );

#### 读写API



> **ReadConsoleInput来获取输入事件**
>
> BOOL ReadConsoleInput( 
>
> HANDLE hConsoleInput, //输入句柄 
>
> PINPUT_RECORD lpBuffer,//输入事件结构体的指针 
>
> DWORD nLength, //要读取的记录数 
>
> LPDWORD lpNumberOfEventsRead //用来接受成功读取记录数的指针 
>
> ); 

> **FillConsoleOutputCharacter 填充指定数据的字符**
>
> BOOL FillConsoleOutputCharacter(
>
> HANDLE hConsoleOutput, // 句柄
>
> TCHAR cCharacter, // 字符
>
> DWORD nLength, // 字符个数
>
> COORD dwWriteCoord, // 起始位置
>
> LPDWORD lpNumberOfCharsWritten // 已写个数
>
> );

> **FillConsoleOutputAttribute从屏幕缓冲区中指定的坐标位置开始，为指定数量的字符单元设置字符属性**
>
> BOOL FillConsoleOutputAttribute( 
>
> HANDLE hConsoleOutput, 
>
> WORD wAttribute, 
>
> //写到控制台屏幕缓冲区的属性
>
> DWORD nLength,
>
> //将被设置成指定颜色属性的字符单元数目
>
> COORD dwWriteCoord,
>
> LPDWORD lpNumberOfAttrsWritten
>
> //指向变量的指针，变量用来存放被设置属性字符单元的实际数目。
>
> );



------

**控制台屏幕缓冲区**

屏幕缓冲区是一个在控制台窗口输出的二维字符及颜色数组。一个控制台可以包含多个屏幕缓冲区，当前屏幕缓冲区指的是显示在屏幕上的那个缓冲区。

系统在创建新控制台时就会创建一个屏幕缓冲区。调用CreateFile函数指定CONOUT$值便可打开控制台的当前屏幕缓冲区。程序可以CreateConsoleScreenBuffer函数为它的控制台创建额外的屏幕缓冲区。一个新的屏幕缓冲区用自己的句柄调用SetConsoleActiveScreenBuffer函数便可设置为当前缓冲区。然而，不管是否是当前缓冲区，都可以被访问以进行读取及写入操作。

　　每个屏幕缓冲区都有自己的二维字符信息记录数组。每个字符信息都被存储在CHAR_INFO结构中，该结构中指定了Unicode或ANSI字符以及显示字符时的前景及背景颜色。

　　每个屏幕缓冲区的关联属性都可以被单独设置。这也意味着变更控制台的当前屏幕缓冲区的效果会很有意思。屏幕缓冲区的关联属性包括：

屏幕缓冲区大小，按字符行列记。

文本属性（WriteFile或WriteConsole函数用于“显示”文本所用的前景及背景）。

窗口大小及定位（在控制台窗口中显示的屏幕缓冲区的矩形区域）。

光标位置，外观及可见度。

输出模式（ENABLE_PROCESSED_OUTPUT及ENABLE_WRAP_AT_EOL_OUTPUT）。关于控制台输出模式的更多信息，请参见高级控制台模式。

**技术**

双缓冲技术(解决屏幕闪烁问题)

[http://www.cnblogs.com/xdblog/p/4783364.html](https://link.jianshu.com?t=http%3A%2F%2Fwww.cnblogs.com%2Fxdblog%2Fp%2F4783364.html)

1.新建一个屏幕缓冲区(此时不可见)

CreateConsoleScreenBuffer();

2.在新建的屏幕缓冲区中写入想要一次显示的内容

WriteConsole();

3.把该缓冲区设置为当前缓冲区(可见)

SetConsoleActiveScreenBuffer();

------

下面是一个是用控制台api编写的一个画图小程序

源码如下：

> \#include <stdio.h>
>
> \#include <windows.h>
>
> int main(void)
>
> {
>
>   HANDLE handle_out = GetStdHandle(STD_OUTPUT_HANDLE);
>
>   HANDLE handle_in = GetStdHandle(STD_INPUT_HANDLE);
>
>   CONSOLE_SCREEN_BUFFER_INFO bInfo;
>
>   INPUT_RECORD mouseRec;
>
>   DWORD res;
>
>   COORD crPos, crHome = {0, 0};
>
>   CONSOLE_CURSOR_INFO cursorinfo= {1, 0};
>
>   SetConsoleCursorInfo(handle_out, &cursorinfo);
>
>   printf("[Cursor Position] X: %2lu Y: %2lu\n", 0, 0);
>
>   while(1)
>
>   {
>
> ​    ReadConsoleInput(handle_in, &mouseRec, 1, &res);
>
> ​    if(mouseRec.EventType == MOUSE_EVENT)
>
> ​    {
>
> ​      if(mouseRec.Event.MouseEvent.dwButtonState == FROM_LEFT_1ST_BUTTON_PRESSED)
>
> ​      {
>
> ​        if(mouseRec.Event.MouseEvent.dwEventFlags == DOUBLE_CLICK)
>
> ​        {
>
> ​          break;
>
> ​        }
>
> ​      }
>
> ​      crPos = mouseRec.Event.MouseEvent.dwMousePosition;
>
> ​      GetConsoleScreenBufferInfo(handle_out, &bInfo);
>
> ​      SetConsoleCursorPosition(handle_out, crHome);
>
> ​      printf("[Cursor Position] X: %2lu Y: %2lu", crPos.X, crPos.Y);
>
> ​      SetConsoleCursorPosition(handle_out, bInfo.dwCursorPosition);
>
> ​      switch(mouseRec.Event.MouseEvent.dwButtonState)
>
> ​      {
>
> ​      case FROM_LEFT_1ST_BUTTON_PRESSED:
>
> ​        FillConsoleOutputCharacter(handle_out, 'A', 1, crPos, &res);
>
> ​        break;
>
> ​      case RIGHTMOST_BUTTON_PRESSED:
>
> ​        FillConsoleOutputCharacter(handle_out, 'a', 1, crPos, &res);
>
> ​        break;
>
> ​      default:
>
> ​        break;
>
> ​      }
>
> ​    }
>
>   }
>
>   CloseHandle(handle_out);
>
>   CloseHandle(handle_in);
>
>   return 0;
>
> }



作者：zzkdev
链接：https://www.jianshu.com/p/71b8edd0bd48
来源：简书
著作权归作者所有。商业转载请联系作者获得授权，非商业转载请注明出处。





```
printf("		**********************************************************\n");
	printf("		*                                                        *\n");
	printf("		*    1.  录入成绩                                        *\n");
	printf("		*    2.  计算每门课程的总分和平均分                      *\n");
	printf("		*    3.  计算每个学生的总分和平均分                      *\n");
	printf("		*    4.  按照每个学生的总分由高到低排出名次表            *\n");
	printf("		*    5.  按照每个学生的总分由低到高排出名次表            *\n");
	printf("		*    6.  按照学号由小到大排出名次表                      *\n");
	printf("		*    7.  按照姓名的字典顺序排出名次表                    *\n");
	printf("		*    8.  按照学号查询学生排名及考试成绩                  *\n");
	printf("		*    9.  按照姓名查询学生排名及考试成绩                  *\n");
	printf("		*    10. 按照优秀、良好、中等、及格、不及格统计百分比    *\n");
	printf("		*    11. 输出所有                                        *\n");
	printf("		*    12. 写入文件                                        *\n");
	printf("		*    13. 读取文件                                        *\n");
	printf("		*                                                        *\n");
	printf("		**********************************************************\n");
```



