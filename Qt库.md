#Qt库

1. 获得信号发送者的指针`sender()`
   1. 函数原型：`QObject* QObject::sender() const`
   2. 示例：`QPushButton* pcur = qobject_cast<QPushButton*>(sender());`