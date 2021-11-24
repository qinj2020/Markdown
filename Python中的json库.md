# Python中的json库

## 编码Python对象为json格式字符串

### 使用`json.dumps()`
1. 函数原型为：
    ```python
    json.dumps(obj, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, \
                indent=None, separators=None, default=None, sort_keys=False, **kw)
    ```
2. `skipkeys`指定是否跳过关键字。如果Python字典类型的key不是基础类型时，跳过关键字则不会引发TypeError。
3. `ensure_ascii`默认所有传入的非ASCII都被转义，如果设置为False，则按原样输出。
4. `check_circular`指定是否进行循环引用检查。循环引用会导致OverflowError。
5. `allow_nan`如果指定为False，则使用超出范围的浮点值(nan, inf, -inf)将会ValueError。默认为True，即使用其在JavaScript中的等效项(NaN, Infinity, -Infinity)。
6. `cls`用于指定自定义的JSONEncoder子类(例如重写`default()`以序列化其他子类)。
```python
import json
class ComplexEncoder(json.JSONEncoder):
    def default(self, obj):
        if isinstance(obj, complex):
            return [obj.real, obj.img]
        # 让父类default方法引发TypeError
        return json.JSONEncoder.default(self, obj)
json.dumps(2 + 1j, cls=ComplexEncoder)  # '[2.0, 1.0]'
ComplexEncoder().encode(2 + 1j) # '[2.0, 1.0]'
list(ComplexEncoder().iterencode(2 + 1j))   # ['[2.0', ', 1.0', ']']
```
7. `indent`指定缩进可以为int或者string(例如`\t`)。用于更好看的打印出来。
8. `separator`应该指定为`(item_separator, key_separator)`元组。为了获得紧凑输出，需要指定为`(',', ':')`以消除多余的空格。
9. `default`无更多介绍。
10. `sort_key`指定对于字典类型的输出是否按key排序。

### 使用`json.dump()`
1. 函数原型为：
    ```python
    json.dumps(obj, fp, *, skipkeys=False, ensure_ascii=True, check_circular=True, allow_nan=True, cls=None, \
                indent=None, separators=None, default=None, sort_keys=False, **kw)
    ```
2. 多一个文件流参数fp，其他参数与上一个相似。文件流按字符串文本写入，而不是字节流写入。

## 将json格式字符串解码为Python对象

### 使用`json.loads()`
1. 函数原型为：
    ```python
    json.loads(s, *, cls=None, object_hook=None, parse_float=None, parse_int=None, \
                parse_constant=None, object_pairs_hook=None, **kw)
    ```
2. `s`将其反序列化为Python对象。
3. `object_hook`指定一个函数用于实现自定义解码器。
4. `parse_float`可以用于为JSON浮点数指定另一种数据类型或解析器。默认等价于`float(num_str)`。
5. `parse_int`可以用于为JSON整数指定另一种数据类型或解析器。默认等价于`int(num_str)`。
6. `parse_constant`如果指定解码`Infinity`，`-Infinity`，`NaN`这些字符串时将被调用，可以用于遇到JSON无效数字时，引发异常。
7. `object_pairs_hook`对于有序对列表解码时调用，可以用于指定自定义解码器，如果同时指定了`object_hook`，则该参数优先调用。

### 使用`json.load()`
1. 函数原型为：
    ```python
    json.loads(fp, *, cls=None, object_hook=None, parse_float=None, parse_int=None, \
                parse_constant=None, object_pairs_hook=None, **kw)
    ```
2. 参数基本与上一个一样。

## 基类解码器Decoders
```python
class json.JSONDecoder(*, object_hook=None, parse_float=None, parse_int=None, parse_constant=None, \
                        strict=True, object_pairs_hook=None)
```
1. 默认解码对应于下表。

    | JSON        | Python |
    | ----------  | ------ |
    | object      | dict   |
    | array       | list   |
    | string      | str    |
    | number(int) | int    |
    |number(real) | float  |
    | true        | True   |
    | false       | False  |
    | null        | None   |

2. 可以理解`NaN`, `Infinity`, `-Infinity`为他们对应的浮点值。
3. `strit`指定是否允许在字符串中使用控制字符。默认为不允许。
4. `decode(s)`方法，返回s表示的Python对象。
5. `raw_decode(s)`方法，解码Python文档。
   
## 基类编码器Encoders
    ```python
    class JSONEncoder(*, skipkeys=False, ensure_acsii=Teue, check_circular=True, allow_nan=True, \
                        sort_keys=False, indent=None, separators=None, default=None)
    ```
1. 默认支持下表类型：
    | python                                 | json   |
    |----------------------------------------|--------|
    | dict                                   | object |
    | list, tuple                            | array  |
    | str                                    | string |
    | int, float, int- & float-derived Enums | number |
    | True                                   | true   |
    | False                                  | false  |
    | None                                   | null   |

2. `default(o)`方法，子类实现该方法，返回一个o的序列化的对象，否则调用父类的实现引发异常。例如：
    ```python
    def default(self, o):
        try:
            iterable = iter(o)
        except TypeError:
            pass
        else:
            return list(iterable)
        return json.JSONEncoder.default(self, o)
    ```
3. `encode(o)`方法，返回JSON字符串表示的Python对象。
4. `iterencode(o)`方法，对o进行编码，并产生每个可用的字符串表示。
