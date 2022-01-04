# Qt库

- 元对象编译器(Meta-Object Compiler, MOC)
    - 一个预处理器，在源程序被编译前将Qt特性程序代码转为标准C++兼容的形式，然后再由标准C++编译器进行编译

- 元对象系统(Meta-Object System)
    - 提供了对象之间通信的信号与槽机制，运行时类型信息和动态属性系统

- 信号与槽的关联方式(枚举类型：`Qt::ConnwctionType`)
    - `Qt::AutoConnection`缺省值，在信号发射时自动确定关联方式。如果信号的接收者与发射者在同一个线程，就使用`Qt::DirectConnection`方式，否则使用`Qt::QueuedConnection`方式
    - `Qt::DirectConnection`信号被发射时槽函数立即执行
    - `Qt::QueuedConnection`在事件循环回到接收者线程后执行槽函数
    - `Qt::BlockingQueuedConnection`信号发射者线程会阻塞直到槽函数执行结束

- 获得信号发送者的指针`sender()`
    - 函数原型：`QObject* QObject::sender() const`
    - 示例：`QPushButton* pcur = qobject_cast<QPushButton*>(sender());`

- 自定义信号
    - 信号就是类定义里的一个函数声明，该函数必须是无返回值的，可以有输入参数。但是这个函数不需要实现，只需要发射(emit)
    ```C++
        signals:
            void signal_name(int a);    // 自定义信号

        ...

        emit signal_name(x);    // 发射信号
    ``` 

- 定时器(`QTimer`)
- 计时器(`QTime`)

- `QPlainTextEdit`右键菜单
    - `QMenu* menu=ui->plainTextEdit->createStandardContextMenu(); //创建标准右键菜单`

- 视图(View)是显示和编辑数据的界面组件
- 模型(Model)是视图与原始数据之间的接口

- 项数据的角色
    - `Qt::ItemDataRole`枚举类型，有多种取值
    - `Qt::DisplayRole`视图组件中显示的字符串
    - `Qt::ToolTipRole`鼠标提示消息
    - `Qt::UserRole`自定义数据

- `QFileSystemModel`提供一个可用于访问本机文件系统的数据模型
    - 用于获取磁盘文件目录的数据模型还有一个`QDirModel`，功能相似，但是`QFileSystemModel`使用单独的线程获取目录文件结构，而`QDirModel`没有使用单独的线程，使用单独的线程才不会阻碍主线程

- `QStyledItemDelegate`和`QItemDelegate`的差别在于其可以使用当前的样式表设置来绘制组件

- 不管是从`QStyledItemDelegate`还是`QItemDelegate`继承设计自定代理组件，都必须实现如下4个函数
    - `createEditor()`创建用于编辑模型数据的widget组件，例如一个`QComboBox`组件
    - `setEditorData()`从数据模型获取数据，供widget组件进行编辑
    - `setModelData()`将widget上的数据更新到数据模型
    - `updateEditorGeometry()`用于给widget组件设置一个合适的大小

- `setAttribute()`用于对窗体的一些属性进行设置，当设置为`Qt::WA_DeleteOnClose`时，窗口关闭时会自动删除，以释放内存。对话框在关闭时缺省是隐藏自己，除非显式的delete

