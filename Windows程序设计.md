# Windows程序设计基础

- 头文件
    - `windows.h`重要的头文件，其中还包含了若干的其他Windows头文件
    - `windef.h`基本数据类型定义
    - `winnt.h`支持Unicode的类型定义
    - `winbase.h`内核的相关函数
    - `winuser.h`用户界面函数
    - `wingdi.h`图形设备接口函数

- Windows程序的入口函数是`WinMain`
    ```C++
    int WINAPI WinMain( 
        HINSTANCE hInstance,        // 应用程序的实例句柄 
        HINSTANCE hPrevInstance,    // 该应用程序的前一个实例句柄，不再使用通常为NULL
        LPSTR pszCmdLine,           // 命令行参数
        int  nShowCmd               // 程序窗口的最初显示方式
        );
    ```

- 显示消息窗口函数
    ```C++
    int MessageBox(
        HWND hWnd,          // 父窗口句柄
        LPCTSTR lpText,     // 要在消息框显示的文本
        LPCTSTR lpCaption,  // 标题栏文本
        UINT uType          // 按钮、图标等类型
        );
    ```
    - 示例：
        - `MessageBox(NULL, _T("Hello, Windows 98!"), _T("HelloMsg"), 0);`

- `va_list`, `va_start`, `va_end`处理堆栈指针
    - `int sprintf(char* _Buffer, const char* _Format, ...)`
        - `_Buffer`保存结果的缓冲区
        - `_Format`格式字符串
        - 第三个参数是指向待格式化的参数数组的指针，该指针指向堆栈中供函数调用的变量
    - va_*就是用来处理这个指针的
    - 示例： 
        ```C++
        int MessageBoxPrintf (TCHAR * szCaption, TCHAR * szFormat, ...)
        {
            TCHAR szBuffer[1024] ; 
            va_list pArgList ;

            // The va_start macro (defined in STDARG.H) is usually equivalent to:
            // pArgList = (char*)&szFormat + sizeof(szFormat) ;
            va_start (pArgList, szFormat) ;

            // The last argument to wvsprintf points to the arguments
            _vsntprintf (szBuffer, sizeof (szBuffer) / sizeof (TCHAR), szFormat, pArgList) ;

            // va_end宏将pArgList清零
            va_end (pArgList) ;

            return MessageBox (NULL, szBuffer, szCaption, 0) ;
        }
        ``` 

- 获得屏幕宽度和高度的像素信息
    - `cxScreen = GetSystemMetrics(SM_CXSCREEN);`
    - `cyScreen = GetSystemMetrics(SM_CYSCREEN);`
     
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
    ```C++
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
    };
    ``` 

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
    ```C++
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
	);
    ``` 
    - 示例：
        ```C++
        //返回一个窗口的句柄，返回值保存在HWND类型的变量中
	    hwnd = CreateWindow(szAppName,  // 窗口类名称
            _T("The Hello Program"),  // window 标题
            WS_OVERLAPPEDWINDOW,        // 窗口风格，这里是普通层叠窗口
            CW_USEDEFAULT,              // initial x position
            CW_USEDEFAULT,              // initial y position
            CW_USEDEFAULT,              // initial x size
            CW_USEDEFAULT,              // initial y size
            NULL,                       // 父窗口 handle
            NULL,                       // window menu handle
            hInstance,                  // program instance handle
            NULL);                      // 创建参数
        ```

- 显示窗口
    ```C++
    BOOL ShowWindow(
  		HWND hWnd,     //显示的窗口句柄
  		int nCmdShow   //显示的方式
	); 
    ```
    - `SW_SHOWNORMAL`窗口正常显示
    - `SW_SHOWMAXIMIZED`窗口以最大化显示
    - `SW_SHOWMINNOACTIVE`窗口只在任务栏显示
    - 如果该函数的最后一个参数是`SW_SHOWNORMAL`，则该窗口的客户区将被窗口类中所指定的背景画刷擦除。下面就需要调用`UpdateWindow(hwnd);   // 这个函数向窗口过程函数(WndProc)发送一个WM_PAINT消息`将窗口客户区重绘。
     
- 字符串转换
    ```C++
    int WideCharToMultiByte(
			  UINT CodePage,            // 字符串代码页
			  DWORD dwFlags,            // 转换方式
			  LPCWSTR lpWideCharStr,    // 需要被转换WCHAR地址
			  int cchWideChar,          // 需要被转换WCHAR的长度
			  LPSTR lpMultiByteStr,     // 用于存放转换后的结果BUFF
			  int cchMultiByte,         // BUFF的长度
			  LPCSTR lpDefaultChar,     // 使用的缺省字符串的地址
			  LPBOOL lpUsedDefaultChar  // 缺省字符串被使用的标识
			);
    ``` 
    
    ```C++
    int MultiByteToWideChar(
        UINT CodePage,              // 字符串代码页
        DWORD dwFlags,              // 转换方式
        LPCSTR lpMultiByteStr,      // 需要被转换CHAR地址
        int cchMultiByte,           // 需要被转换CHAR的长度
        LPWSTR lpWideCharStr,       // 用于存放转换后的结果BUFF
        int cchWideChar             // BUFF的长度
        );
    ```
    - 转换方法：
        1. 将要转换的字符串传给函数，从返回值中获取转换后字符串的长度 
        2. 再次调用，向缓冲区写入该长度的转换后的值
    - 示例：
        ```C++
        iLength = MultiByteToWideChar(CP_ACP, 0, szStr, strlen(szStr) + 1, NULL, 0);
        MultiByteToWideChar(CP_ACP, 0, szStr, strlen(szStr) + 1, szTchar, iLength);
        ```

- 消息循环
    ```C++
	// Windows为每个程序维护了一个消息队列，当事件发生后，会自动把事件转化为消息
	// 并放置在程序的消息队列中，程序通过下面的消息循环来从消息队列中获取消息
	while(GetMessage(&msg, NULL, 0, 0))	    // GetMessage()从消息队列中检索消息
	{										// 后三个参数为NULL/0表示得到该程序创建的所有窗口的消息
											// 若得到的消息为WM_QUIT，则GetMessage()返回0，否则返回非0

		TranslateMessage(&msg); // 返回给Windows以进行某些键盘消息的转换

		DispatchMessage(&msg);  // 再次将消息返回给Windows
	}
    ```

- 窗口过程函数，或者交窗口消息处理函数
    ```C++
    //程序真正有意义的在窗口过程中：(窗口过程函数名可任意)
    //一个程序可以包含多个窗口过程，单一个窗口过程总与一个调用RegisterClass的窗口类关联
    //窗口过程函数只用来处理窗口消息！（但是这个消息包括了CREATE和PAINT消息）
    LRESULT CALLBACK WndProc(HWND hwnd,   //窗口句柄
        UINT message,  // 消息标志
        WPARAM wParam, // 后两个参数用于提供消息更丰富的信息
        LPARAM lParam)
    ```

- `WM_CREATE`窗口创建消息
  
- `WM_SIZE`窗口尺寸改变消息

- `WM_PAINT`绘图消息
    - 当窗口的客户区的部分或全部"无效"且必须更新时，应用程序将得到此通知，意味着窗口必须重绘
    - 窗口首次创建时，整个客户区都是无效的
    - 调整窗口的尺寸时，客户区会变为无效
    - 最大化，最小化，窗口无效
    - 窗口重叠时，被覆盖的区域在后来不再被遮挡时，窗口无效

- `WM_DESTORY`窗口销毁消息
    - 户用单击【关闭】按钮时发出该消息
    - 通过`PostQuitMessage(0);`响应，插入`WM_QUIT`消息到程序的消息队列，退出消息循环

- 按键消息
    - `WM_KEYDOWN`
    - `WM_KEYUP`

- 按键产生的字符消息
    - `WM_CHAR`

- 鼠标移动消息
    - `WM_MOUSEMOVE`

- 定时器消息
    - `WM_TIMER`

- 注销窗口
    - `UnregisterClass()`

- 获取窗口类信息
    - `GetClassInfo()`

- 获取窗口的窗口类名称
    - `GetClassName()`

- 窗口类的附加数据的设置和获取
    - `GetClassLong()`
    - `SetClassLong()`

- 窗口的附加数据的设置和获取
    - `GetWindowLong()`
    - `SetWindowLong()`

- 设置和获取指定窗口的父窗口
    - `SetParent()`
    - `GetParent()`

- 移动窗口
    - `MoveWindow()`

- 异步播放音频文件，开始播放立即返回
    - `PlaySound(_T("hellowin.wav"), NULL, SND_FILENAME | SND_ASYNC);`

- 相应`WM_PAINT`消息进行绘图
    - `hdc = BeginPaint(hwnd, &ps); // 获得设备环境句柄，使整个客户区有效`
    - 绘制图形
    - `EndPaint(hwnd, &ps); // 释放设备环境句柄`
    - `BeginPaint()`返回的设备环境句柄，无法在客户区外绘制

- 获得窗口客户区的尺寸大小(legt, top, right, bottom)信息
    - `GetClientRect(hwnd, &rect);`

- 绘制文本
    ```C++
    DrawText(hdc,		// 设备环境句柄
        _T("Hello, Windows 98!"),   // 文本信息
        -1,     // 表示该文本字符串以0结尾
        &rect,
        DT_SINGLELINE | DT_CENTER | DT_VCENTER);    // 显示样式：单行，居中，水平
    ``` 

- 默认窗口过程函数
    - `return DefWindowProc(hwnd, message, wParam, lParam);`

- 占位符
    - C标准
        - `%s` -> `const char*`
        - `%ls` -> `const wchar_t*`
    - 微软
        - `%s` -> `const wchar_t*`
        - `%S`或`%hs` -> `const char*`
    - `%hs`可以保证微软与其他C库都都解释成`const char*`

