# Flask中的Python

这里我们先集中一节学习一下Flask中用到的一些Python较为生僻的特性.
当然如果不太复杂的话, 我们尽量直接看代码来学习.
而且我们只限于理解Flask中的用法, 而不扩大范围.
以下代码的部分来自cpython项目, 我在其中删除了一些源注释, 添加了中文注释.

importlib
* spec

一个命名空间，其中包含用于加载模块的相关导入信息。是 importlib.machinery.ModuleSpec 的实例.
包括模块全限定名, 模块的加载器, 模块的原始地址, 子模块地址列表, 模块所在的包的全限名等

* from asgiref.sync import async_to_sync


* functool.update_wrapper

更新一个 wrapper 函数以使其类似于 wrapped 函数.

```
def update_wrapper(wrapper, #装饰函数
                   wrapped, #被装饰函数
                   assigned = WRAPPER_ASSIGNMENTS,
                   updated = WRAPPER_UPDATES):
    # 对assgined参数中指明的属性进行装饰函数属性设置
    for attr in assigned:
        try:
            value = getattr(wrapped, attr)
        except AttributeError:
            pass
        else:
            setattr(wrapper, attr, value)

    # 对updated参数中指明的属性进行装饰函数属性值更新
    for attr in updated:
        getattr(wrapper, attr).update(getattr(wrapped, attr, {}))

    # 给装饰函数添加__wrapped__属性, 指向被装饰函数
    wrapper.__wrapped__ = wrapped
    return wrapper
```

* typing.cast

cast方法其实什么都没做. 它的源代码是这样:

```
def cast(typ, val):
    return val
```

它只是示意val是指定的类型, 不会做任何检查和转换.
Python是动态类型语言, 所以这里也不需求做任何转换.


* typing.TypeVar

TypeVar的用法如下:
```
T = TypeVar('T')  # T可以是任意类型
A = TypeVar('A', str, bytes)  # A是str或者bytes类型
```

类型变量主要是用于静态类型检查.
我们可以用它定义泛型类型以及泛型函数.

* typing.Optional 可选类型

有些类型检查工具会对a:int = None这样的声明报错.
使用Optional可以告诉类型检查工具这个参数除了给定的默认值外还可以是None, 所以可以改成使用a:Optional[int] = None

Optional[X] 等价于 Union[X, None]
3.10后可以用联合类型表达式直接写成 X | None