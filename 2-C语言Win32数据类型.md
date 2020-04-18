### 数据类型



| 数据类型 | 描述             |
| -------- | ---------------- |
| UINT     | unsigned　int    |
| LPSTR    | char *           |
| LP/P开头 | 指针类型         |
| HBRUSH   | 背景画笔类的句柄 |
| HICON    | 图标类的句柄     |
| HCURSOR  | 类光标的句柄     |
| HWND     | 窗口句柄         |
| DWORD    | unsigned long    |
| WORD     | unsigned short   |
| SHORT    | short            |



### 结构体原型

#### CONSOLE_SCREEN_BUFFER_INFO

```c
typedef struct _CONSOLE_SCREEN_BUFFER_INFO { 

COORD dwSize; 				// 缓冲区大小

COORD dwCursorPosition; 	// 当前光标位置

WORD wAttributes; 			// 字符属性
	
SMALL_RECT srWindow; 		// 当前窗口显示的大小和位置

COORD dwMaximumWindowSize; 	// 最大的窗口缓冲区大小

} CONSOLE_SCREEN_BUFFER_INFO ;
```



#### COORD

```c
typedef struct _COORD {
    SHORT X;
    SHORT Y;
} COORD, *PCOORD;
```



#### SMALL_RECT

```c
typedef struct _SMALL_RECT {
    SHORT Left;		//矩形左上角的x坐标
    SHORT Top;		//矩形左上角的y坐标
    SHORT Right;	//矩形右下角的x坐标
    SHORT Bottom;	//矩形右下角的y坐标
} SMALL_RECT, *PSMALL_RECT;
```





#### CONSOLE_CURSOR_INFO

```c
typedef struct _CONSOLE_CURSOR_INFO {
  DWORD dwSize;
  BOOL  bVisible;
} CONSOLE_CURSOR_INFO, *PCONSOLE_CURSOR_INFO;
```





**dwSize**
光标填充的字符单元格的百分比。此值在1到100之间。光标外观有所不同，范围从完全填充单元格到在单元格底部显示为水平线。

**注意**  虽然**dwSize**值通常在1到100之间，但是在某些情况下，可能会返回该范围之外的值。例如，如果注册表中的**CursorSize**设置为0，则返回的**dwSize**值将为0。

 

**bVisible**
光标的可见性。如果光标可见，则此成员为**TRUE**。



































