# Windows程序设计基础

- 头文件
    - `windows.h`重要的头文件，其中还包含了若干的其他Windows头文件
    - `windef.h`基本数据类型定义
    - `winnt.h`支持Unicode的类型定义
    - `winbase.h`内核的相关函数
    - `winuser.h`用户界面函数
    - `wingdi.h`图形设备接口函数

- Windows程序的入口函数是`WinMain`
    ```C
    int WINAPI WinMain( 
        HINSTANCE hInstance,        // 应用程序的实例句柄 
        HINSTANCE hPrevInstance,    // 该应用程序的前一个实例句柄，不再使用通常为NULL
        LPSTR pszCmdLine,           // 命令行参数
        int  nShowCmd               // 程序窗口的最初显示方式
        )
    ```

- 显示消息窗口函数
    ```C
    int MessageBox(
        HWND hWnd,          // 父窗口句柄
        LPCTSTR lpText,     // 要在消息框显示的文本
        LPCTSTR lpCaption,  // 标题栏文本
        UINT uType          // 按钮、图标等类型
        )
    ```
    - 可选按钮类型：
        - `MB_OK`
        - `MB_OKCANCEL`
        - `MB_ABORTRETRYIGNORE`
        - `MB_YESNOCANCEL`
        - `MB_YESNO`
        - `MB_RETRYCANCEL`
    - 可以用`|`连接表示哪个按钮为默认按钮
        - `MB_DEFBUTTON1`
        - `MB_DEFBUTTON2`
        - `MB_DEFBUTTON3`
    - 可以用`|`连接指定显示的图标
        - `MB_ICONHAND`   别名：`MB_ICONERROR`，`MB_ICONSTOP`
        - `MB_ICONQUESTION`
        - `MB_ICONEXCLAMATION`    别名：`MB_ICONWARNING`
        - `MB_ICONASTERISK`   别名：`MB_ICONINFORMATION`

- Windows的字符串函数
    - `iLength = lstrlen(pString);`计算字符串长度
    - `pString = lstrcpy(pString1, pString2);`复制字符串
    - `pString = lstrcpy(pString1, pString2, iCount);`复制字符串
    - `pString = lstrcat(pString1, pString2);`连接字符串
    - `iComp = lstrcmp(pString1, pString2);`比较字符串
    - `iComp = lstrcmpi(pString1, pString2);`比较字符串，忽略大小写

- 常用Windows函数
    - `LoadIcon()`加载图标以供程序使用
    - `LoadCursor()`加载鼠标光标以供程序使用
    - `GetStockObject()`获取一个图形对象
    - `RegisterClass()`为应用程序的窗口注册一个窗口类
    - `CreateWindow()`基于窗口类创建一个窗口
    - `ShowWindow()`在屏幕中显示窗口
    - `UpdateWindow()`指定窗口对其自身进行重绘
    - `GetMessage()`从消息队列获取消息
    - `TranslateMessage()`翻译一些键盘消息
    - `DispatchMessage()`将消息发送给窗口过程
    - `PlaySound()`播放声音文件
    - `BeginPaint()`表明窗口绘制开始
    - `EndPaint()`结束窗口绘制
    - `GetClientRect()`获取窗口客户区的尺寸
    - `DrawText()`显示一个文本字符串
    - `PostQuitMessage()`将退出消息插入消息队列
    - `DefWindowProc()`执行默认的消息处理

- Windows的匈牙利标记法
    - 前缀`c`，表示`char`或`WCHAR`或`TCHAR`
    - 前缀`by`，表示`BYTE`(无符号字符)
    - 前缀`n`，表示`short`(短整型)
    - 前缀`i`，表示`int`(整型)
    - 前缀`x`和`y`，表示`int`，代表x坐标和y坐标
    - 前缀`cx`和`cy`，表示`int`，代表x或y的长度，`c`表示`count`(计数)
    - 前缀`B`或`f`，表示BOOL(int)，`f`表示flag
    - 前缀`w`，表示WORD(无符号短整型)
    - 前缀`l`，表示LONG(长整型)
    - 前缀`dw`，表示DWORD(无符号长整型)
    - 前缀`fn`，表示函数
    - 前缀`s`，表示字符串
    - 前缀`sz`，表示以0结束的字符串
    - 前缀`h`，表示句柄
    - 前缀`p`，表示指针
    - 前缀`cb`，表示字节数(count of byte)

- 窗口类结构体
    ```C
    struct WNDCLASS { 
        UINT    style;          // 窗口类的风格
        WNDPROC lpfnWndProc;    // 窗口消息处理函数
        int     cbClsExtra;     // 窗口类附加数据大小
        int     cbWndExtra;     // 窗口附加数据大小
        HANDLE  hInstance;      // 当前应用程序的实例句柄
        HICON   hIcon;          // 窗口图标
        HCURSOR hCursor;        // 窗口的鼠标
        HBRUSH  hbrBackground;  // 窗口的背景画刷
        LPCTSTR lpszMenuName;   // 菜单
        LPCTSTR lpszClassName;  // 窗口类名称
    }
    ``` 
    - 窗口类的风格
        - `CS_HREDRAW`指定无论何时窗口的水平尺寸被改变，所有基于该窗口类的窗口都将被重新绘制
        - `CS_VREDRAW`指定无论何时窗口的垂直尺寸被改变，所有基于该窗口类的窗口都将被重新绘制

- `GetLastError()`用于获取当函数调用失败时的扩展错误信息
- 检查每个函数调用的返回值以判断是否有错误发生，这种举措是很有意义的

- 加载图标以供程序使用
    - `wndclass.hIcon = LoadIcon(NULL, IDI_APPLICATION);`

- 加载鼠标光标以供程序使用
    - `wndclass.hCursor = LoadCursor(NULL, IDC_ARROW);`	

- 获取白色画刷图形对象
    - `wndclass.hbrBackground = (HBRUSH)GetStockObject(WHITE_BRUSH);`

- 为应用程序的窗口注册一个窗口类
    - `RegisterClass(&wndclass);`

- 创建窗口
    ```C
     HWND CreateWindow(
  		LPCTSTR lpClassName,    // 窗口类型名称
  		LPCTSTR lpWindowName,   // 窗口标题
  		DWORD dwStyle,          // 窗口风格
  		int x,                  // 窗口的左上角X坐标，相对于屏幕左上角
  		int y,                  // 窗口的左上角Y坐标，相对于屏幕左上角
  		int nWidth,             // 窗口的宽度
  		int nHeight,            // 窗口的高度
  		HWND hWndParent,        // 父窗口句柄
  		HMENU hMenu,            // 窗口菜单句柄
  	    HANDLE hInstance,       // 应用程序的实例句柄
  	    LPVOID lpParam          // 创建的参数，一般为NULL
	)
    ``` 