# MFC基础


- Microsoft基础类(MFC)库提供完整源代码
- 头文件在 \atlmfc\include 目录中
- 实现文件位于 \atlmfc\src\mfc 目录中


## 单文档界面(SDI)与多文档界面(MDI)
1. SDI应用程序一次只允许一个打开的文档框架窗口。
2. MDI应用程序同一个应用程序实例中打开多个文档框架窗口。


- 任何对象都可以通过调用全局函数`AfxGetApp`来获取指向应用程序对象的指针。


- 资源编辑器中的动态布局
    - 移动类型，确定控件在更改对话框大小时如何移动
    - 调整大小类型，确定控件在更改对话框大小时如何调整大小
    - 例如希望按钮等控件保持固定大小并位于右下角
        - 将"调整大小类型"设置为"无"
        - 将"移动类型"设置为"两者 "
        - 对于 "移动类型"下的"移动 X"和"移动 Y" 值，设置 100%，使控件保持与右下角的固定距离。


- 以编程方式设置动态布局属性
    - 调用`GetDynamicLayout()`获得指向`CMFCDynamicLayout`对象的指针
        - 示例：`CMFCDynamicLayout* dynamicLayout = pDialog->GetDynamicLayout();`
    - 用动态布局类上的静态方法创建用于编码控件调整方式的`MoveSettings`结构
        - 对移动的水平和/或垂直特性传入一个百分比
        -  这些静态方法都将返回新创建的`MoveSettings`对象，你可以使用该对象来指定控件的移动行为
        -  100 表示完全根据该对话框更改的大小进行移动，这会导致控件的边缘与新边框保持固定的距离。
              -  示例：`MoveSettings moveSettings = CMFCDynamicLayout::MoveHorizontal(100);`
    - 对大小行为执行相同的操作，该行为使用`SizeSettings`类型
        - 示例：`SizeSettings sizeSettings = CMFCDynamicLayout::SizeNone();`
    - 使用`CMFCDynamicLayout::AddItem()`方法将 控件添加到动态布局管理器
        - 示例：`dynamicLayout->AddItem(hWndControl, moveSettings, sizeSettings);`
    - 要启用或者禁用对话框动态布局，调用`CWnd：：EnableDynamicLayout()` 方法
        - 示例：`pDialog->EnableDynamicLayout(TRUE);`

- 3个WindowsAPI可以运行可执行文件
    1. `WinExec()`主要用于运行exe文件
       - `UINT WinExec(LPCSTR lpCmdLine, UINT uCmdShow)`
       -  `lpCmdLine`要执行的应用程序的命令行字符串
       -  `uCmdShow`定义程序的窗口如何显示
       -  返回值>31，表示函数调用成功。失败返回以下值：
          1. 0：系统资源不足
          2. `ERROR_BAD_FORMAT`：exe文件无效
          3. `ERROR_FILE_NOT_FOUND`：未找到指定文件
          4. `ERROR_PATH_NOT_FOUND`：未找到指定路径
    2. `ShellExecute()`不仅可以运行exe文件，非exe文件自动通过关联的程序打开
        ```c++
            HINSTANCE ShellExecute(HWND hWnd, LPCTSTR lpOperation, LPCTSTR lpFile, LPCTSTR lpParameters, LPCTSTR lpDirectory, INT nShowCmd);
        ```
        - `hWnd`指定父窗口句柄
        - `lpOperation`指定要进行的操作，"open"表示执行指定的程序或打开指定的文件或文件夹，"print"表示打印指定文件, "explore"表示浏览指定文件夹，NULL默认open
        - `lpFile`指定文件
        - `lpParameters`指定命令行参数，若非exe文件，则应为NULL
        - `lpDirectory`指定默认目录
        - `nShowCmd`指定程序窗口的初始显示方式，若非exe文件，则应为0，可以使用以下值：
            - `SW_HIDE`, `SW_SHOW`, `SW_SHOWNORMAL`, `SW_SHOWMAXIMIZED`, `SW_SHOWMINIMIZED`
        - 返回值>32，表示函数调用成功
        - 示例：
            - `ShellExecute(handle, "open ", path_to_folder, NULL, NULL, SW_SHOWNORMAL);`
            - `ShellExecute(handle, "explore ", path_to_folder, NULL, NULL, SW_SHOWNORMAL);`
            - `ShellExecute(this->m_hWnd, "open", "notepad.exe", "c:\\MyLog.log", "", SW_SHOW);`打开一个应用程序，并没有给完整路径
            - `ShellExecute(this->m_hWnd, "open", "c:\\abc.txt", "", "", SW_SHOW);`打开与某程序关联的文件
            - `ShellExecute(this->m_hWnd, "open", "http://www.google.com", "", "", SW_SHOW);`打开一个网页

