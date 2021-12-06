# Qt库


- 获得信号发送者的指针`sender()`
    - 函数原型：`QObject* QObject::sender() const`
    - 示例：`QPushButton* pcur = qobject_cast<QPushButton*>(sender());`

