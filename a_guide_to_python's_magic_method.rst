Python魔法方法指南
====================

**简介**

什么是魔法方法？他们是Python面向对象编程的一切。他们是你在类中定义的‘魔法’一样的特殊方法。魔法方法总是围绕这双下划线（比如：__init__或者__lt__）。文档中对魔法方法的说明并不充分。所有的魔法方法全部在Python文档中的同一部分说明，但是他们分布散乱结构混乱。很难在文档中找到相应的示例代码。

为了修复Python文档的缺陷，我为魔法方法编写了这个通俗易懂、富含各种实例的文档。我着手每周撰写博客文章，现在已经完成，是时候将所有内容整合到这个指南中了。

希望你们可以喜欢。把这份指南当做一个教程、一次进修、或者是参考；希望这份指南可以获得大家的喜爱。

**1、构造和实例化（__new__,__init__,__del__)**

``__init__`` ，大家都知道这个最基础的魔法方法。通过它我们可以定义对象的实例化操作。然而，当我们调用 ``x=SomeClass()``  ，``__init__`` 并不是第一被调用的魔法方法。实际上是 ``__new__`` 方法创建的类实例，然后给初始化器传入创建实例时的所有参数。
 
在对象的生命末期有 ``del`` 方法。让我们更深入的探讨这三个魔法方法。

``__new__(cls, [...)`` 
__new__是对象实例化调用的第一个方法。这个类是它的第一个方法，其他参数全部传输给__init__方法。__new__很少被使用，但是它有它的用途，尤其是继承一个不可变类型比如：tuple或string。我并不想太深入的解释new方法因为它并不实用，想了解更深入的内容可以阅读Python docs。 ``python metaclass and __new__`` what's metaclass?  

``__init__(self,[...)``  

类的初始化器。它会接受所有传入构造器的参数，（比如如果你调用 ``x=SomeClass(10,'foo')`` ,init会得到10和'foo'参数。 ``__inti__`` 在Python类定义中应用非常广泛。
``__del(self)__`` 
如果 ``__new__`` 和 ``__init__`` 是对象的构造器结构， ``__del__`` 是解构器。它并没有实现del x语句（所有del x并不会解释为x.__del__()）。实际上，它是为垃圾回收设计的。在进行额外清理的时候是非常有用的对象，比如套接字或者文件对象。注意，如果解释器存在的情况下对象任然在活动那么del并不会执行，所以__del__并不能替代其他手段。（你在执行它之前要关闭所有的连接）。实际上，因为它的不稳定性所以基本不适用它。

把他们放在一起，这里有个使用__inti__和__del__的例子：
::
    from os.path import join

        class FileObject:
            '''Wrapper for file objects to make sure the file gets closed on deletion'''
            def __init__(self, filepath='~', filename='sample.txt'):
            # open a file filename in filepath in read and write mode
                self.file = open(join(filepath,filename),'r+')  
            def __del__(self):
                self.file.close()
                del self.file

