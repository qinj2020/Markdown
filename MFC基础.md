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