[toc]





### 1. vs2019创建Win32

启动窗口

c++

windows桌面应用程序

应用程序

空项目



以目前学习的知识，源文件的后缀名.c和.cpp无所谓，是通用的





---



### 2. 基础介绍

#### 头文件

```c
#include <windows.h>
```

windows.h是主要头文件，如果不知道一个函数的头文件位置，可以去msdn官网查询

```c
https://social.msdn.microsoft.com/search/en-US
```



#### 主函数

```c
int WINAPI WinMain(HINSTANCE hInstance, HINSTANCE hPrevInstance, 
    LPSTR lpCmdLine, int nChowCmd)
{
    MessageBox(NULL, "Goodbye, cruel world!", "Note", MB_OK);
    return 0;
}
```



##### WinMain

WinMain本质就是main函数调用的，main函数被封装了



##### 返回值

返回值为int类型



##### 参数列表

instance 翻译为：实例，HINSTANCE就相当于一个句柄id

参数名可以随便写，不过通用这样写

| 参数          | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| hInstance     | 应用程序当前实例的句柄(內存中的.exe文件)                     |
| hPrevInstance | 前一个实例句柄，目前已经舍弃，始终为NULL                     |
| lpCmdLine     | 命令行参数组成的一个单字符串，不包括程序名字                 |
| nChowCmd      | 一个将要传递给ShowWindow()的整数指定窗口如何显示，也可以使用nCmdShow，无所谓 |



#### Win32数据类型

　　你会发现很多普通的关键字或类型在windows中有特定的定义．UINT是unsigned　int，LPSTR是char \*你怎么用完全取決于你自己.你要是喜欢 char\* 超过了 LPSTR，那就用就是了．当然在你替換一个数据类型前你要确定你知道它是什么．

　一个C接在LP后面表示是常量指针．LPCSTR表示一个指向不会也不能被修改的常量字符串的指针．LPSTR指向的就不是常量的，可以被修改．

| 数据类型 | 描述             |
| -------- | ---------------- |
| UINT     | unsigned　int    |
| LPSTR    | char *           |
| LP/P开头 | 指针类型         |
| HBRUSH   | 背景画笔类的句柄 |
| HICON    | 图标类的句柄     |
| HCURSOR  | 类光标的句柄     |
| HWND     | 窗口句柄         |
|          |                  |



#### sal技术

使用 vs2019 你的 `WinMain函数` 可能会有黄色的下划线警告，但是我的 `vs2019 16.5.3` 版本并没有恶心的下划线，如果小伙伴们有，可以这样解决，当然不用也是完全可以的

```c
int WINAPI WinMain(	_In_ HINSTANCE hInstance,
					_In_opt_ HINSTANCE hPreInstance,
					_In_ LPSTR lpCmdLine,
					_In_ int nCmdShow) {

	return 0;
}
```

这是sal技术，Microsoft源代码注释语言，是微软的东西可能跨平台不好使，本质是辅助我们避免野指针



| 前缀            | 描述                                                 |
| --------------- | ---------------------------------------------------- |
| \_In\_          | 用于传入指针，表示参数不能为空指针，在非指针上无影响 |
| \_In\_opt\_     | 用于传入指针，表示参数可以为空指针                   |
| \_Out\_         | 用于指针赋值，表示参数不能为空指针                   |
| \_Out\_opt\_    | 用于指针赋值，表示参数可以为空指针                   |
| \_Inout\_       | 双向传递，不可为NULL                                 |
| \_Inout\_opt\_  | 双向传递，可为NULL                                   |
| \_Outptr\_      | 用于二级指针，修改外部指针的指向，不可为NULL         |
| \_Outptr\_opt\_ | 用于二级指针，修改外部指针的指向，可为NULL           |





---



### 3. 窗口类

`WNDCLASS/WNDCLASSEX` 用于设置一个窗口的基本属性，实质就是一个结构体

我们使用 `WNDCLASSEX` 是升级版



#### 结构体原型

```c
typedef WNDCLASSEXW WNDCLASSEX;
typedef struct tagWNDCLASSEXW {
  UINT      cbSize;
  UINT      style;
  WNDPROC   lpfnWndProc;
  int       cbClsExtra;
  int       cbWndExtra;
  HINSTANCE hInstance;
  HICON     hIcon;
  HCURSOR   hCursor;
  HBRUSH    hbrBackground;
  LPCWSTR   lpszMenuName;
  LPCWSTR   lpszClassName;
  HICON     hIconSm;
} WNDCLASSEXW, *PWNDCLASSEXW, *NPWNDCLASSEXW, *LPWNDCLASSEXW;
```



#### 参数列表

| 属性                     | 描述                                                 |
| ------------------------ | ---------------------------------------------------- |
| int   cbWndExtra;        | 窗口的额外内存空间，一般为0                          |
| int   cbClsExtra;        | 窗口类的额外内存空间，一般为0                        |
| UINT   cbSize;           | 当前结构体的大小                                     |
| HBRUSH    hbrBackground; | 窗口颜色                                             |
| HCURSOR   hCursor;       | 鼠标样式                                             |
| HICON     hIcon;         | 上角图标如果此成员为NULL，则系统提供默认图标。       |
| HICON     hIconSm;       | 任务栏图标                                           |
| HINSTANCE hInstance;     | 包含类的窗口过程的实例的句柄                         |
| WNDPROC   lpfnWndProc;   | 回调函数                                             |
| LPCSTR    lpszMenuName;  | 窗口类名字（唯一性）                                 |
| LPCSTR    lpszMenuName;  | 菜单名字                                             |
| UINT      style;         | 显示类型，垂直刷新，水平刷新CS_HREDRAW \| CS_VREDRAW |




#### 窗口颜色

官方文档

```
https://docs.microsoft.com/en-us/windows/win32/api/winuser/ns-winuser-wndclassexw
```

必须是以下的标准颜色

| 颜色               | 翻译 |
| ------------------ | ---- |
| COLOR_ACTIVEBORDER |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |
|                    |      |



- COLOR_ACTIVEBORDER
- COLOR_ACTIVECAPTION
- COLOR_APPWORKSPACE
- COLOR_BACKGROUND
- COLOR_BTNFACE
- COLOR_BTNSHADOW
- COLOR_BTNTEXT
- COLOR_CAPTIONTEXT
- COLOR_GRAYTEXT
- COLOR_HIGHLIGHT
- COLOR_HIGHLIGHTTEXT
- COLOR_INACTIVEBORDER
- COLOR_INACTIVECAPTION
- COLOR_MENU
- COLOR_MENUTEXT
- COLOR_SCROLLBAR
- COLOR_WINDOW
- COLOR_WINDOWFRAME
- COLOR_WINDOWTEXT



#### 示例

```c
#include <windows.h>

int WINAPI WinMain(	_In_ HINSTANCE hInstance,
					_In_opt_ HINSTANCE hPreInstance,
					_In_ LPSTR lpCmdLine,
					_In_ int nCmdShow) {

	WNDCLASSEX wnd;

	wnd.cbClsExtra = 0;
	wnd.cbWndExtra = 0;
	wnd.cbSize = sizeof(WNDCLASSEX);
	wnd.hbrBackground = (HBRUSH)COLOR_BTNFACE;
	wnd.hCursor = LoadCursor;
	wnd.hIcon = NULL;
	wnd.hIconSm = NULL;
	wnd.hInstance = hInstance;
	wnd.lpfnWndProc = NULL;
	wnd.lpszClassName = TEXT("windows_name");
	wnd.lpszMenuName = NULL;
	wnd.style = CS_HREDRAW | CS_VREDRAW;

	return 0;
}

```



#### 注意事项

```c
wnd.lpszClassName = TEXT("windows_name");
```

字符串用一个TEXT代表自动适应字符串的编码，在多字符编码中，一个字符占一个字节，在unicode中，一个字符占2个字节



```c
wnd.lpszClassName = L"windows_name";
```

字符串前面加一个L代表unicode



```c
#include<TCHAR.h>

wnd.lpszClassName = _T("windows_name");
```

与TEXT功能一样，只不过形式不一样





---



### 4. 注册窗口类

```c
ATOM WINAPI RegisterClassExW(_In_ CONST WNDCLASSEXW *);
```



```c
RegisterClassEx(&wnd);
```





---



### 5. 创建窗口

使用 `CreateWindowEx` 函数创建窗口



#### 函数原型

```c
#define CreateWindowEx  CreateWindowExA
HWND CreateWindowExA(
  DWORD     dwExStyle,		//窗口属性
  LPCSTR    lpClassName,	//类名称，wnd.lpszClassName
  LPCSTR    lpWindowName,	//窗口名称，自定义
  DWORD     dwStyle,		//窗口风格
  int       X,				//x坐标
  int       Y,				//y坐标
  int       nWidth,			//窗口宽度
  int       nHeight,		//窗口高度
  HWND      hWndParent,		//父窗口关系，没有父窗口就NULL
  HMENU     hMenu,			//菜单，没有就NULL
  HINSTANCE hInstance		//示例句柄
  LPVOID    lpParam			//自定义参数，没有就NULL
);
```



#### 返回值

返回值为窗口句柄

```
If the function succeeds, the return value is a handle to the new window.
If the function fails, the return value is NULL.
```



#### 示例

```c
HWND hwnd = CreateWindowEx(WS_EX_TOPMOST | WS_EX_TOOLWINDOW, 
		wnd.lpszClassName, TEXT("新窗口"), WS_TILEDWINDOW,
		200, 100, 400, 300, NULL, NULL, hInstance, NULL);
```



#### 官方文档

##### 窗口额外风格

```
https://docs.microsoft.com/zh-cn/windows/win32/winmsg/extended-window-styles
```



##### 窗口风格

```
https://docs.microsoft.com/zh-cn/windows/win32/winmsg/window-styles
```





### 6. 显示窗口

#### 函数原型

```c
BOOL ShowWindow(
  HWND hWnd,
  int  nCmdShow
);
```



#### 返回值

```
If the window was previously visible, the return value is nonzero.

If the window was previously hidden, the return value is zero.
```



#### 参数列表

| 参数     | 描述     |
| -------- | -------- |
| hWnd     | 窗口句柄 |
| nCmdShow | 显示风格 |



##### 显示风格

|      |      |
| ---- | ---- |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |
|      |      |





#### 官方文档

显示显示风格

```
https://docs.microsoft.com/en-us/windows/win32/api/winuser/nf-winuser-showwindow
```



### 7. 更新窗口



#### 函数原型

```c
BOOL UpdateWindow(
  HWND hWnd
);
```



#### 参数

Handle to the window to be updated.



#### 返回值

```
If the function succeeds, the return value is nonzero.

If the function fails, the return value is zero.
```





### 8. 消息机制

windows处理用户行为的代码逻辑，即算法，这个算法就叫消息机制



















