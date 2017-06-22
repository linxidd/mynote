# FluentPython笔记  

## 第二章 序列数组  

### 内建序列  

内建序列包括容器序列，顺序序列。其中容器序列包含的是对包含对象的*引用*，顺序序列在内存中存储相应对象。  
容器对象包括 **list tuple collections.deque** 可包含各种类型的对象。  
顺序对象包括 **str bytes bytearray memoryview array.array** 只能包含一种类型的对象。  
  
可变序列 **list bytearray array.array collections.deuqe memoryview**   
不可变序列 **tuple str bytes**  

#### 列表解析  

不可滥用列表解析，如果列表解析超过两行需要重构或者使用普通的for 循环。  
**注意** 在[],{},()中可以不适用\换行。  
**极其注意** Python2.x中列表解析中使用的变量具备周围作用域    
``` 
>>> x = 'my precious'
>>> dummy = [x for x in 'ABC']
>>> x
'C'
```
注意x的值变化了，Python3中不会有这种问题。  
现在列表解析，生成器语句和他们的兄弟set和dict解析都有自己的作用域，周围作用域中的变量仍然可以引用。局部变量不影响周围作用域的变量。  

### 元祖不仅仅是不可变的列表  

#### 元祖作为记录  
元祖可以作为一个记录使用包括它的值和位置  
```
>>> traveler_ids = [('USA', '31195855'), ('BRA', 'CE342567'), ('ESP', 'XDA205856')]
>>> for passport in sorted(traveler_ids):
...     print('%s/%s' % passport)
...
BRA/CE342567
ESP/XDA205856
USA/31195855
```  
#### 元祖的解包  
 ``` >>> b, a = a, b ```  
 ``` _ ``` 解包时候用作占位符。 
**使用 \* 获取剩余的元素**  
平行赋值(Python3)  
 ``` >>> a, b, \*rest = range(5) ```  
 ``` \* ``` 可以出现在任何位置  
pep3113 Removal of Tuple Parameter Unpacking  

#### 命名元祖  

``` collections.namedtuple ```  
例1-1中 
``` Card = collections.namedtuple('Card', ['rank', 'suit']) ```  
**命名元祖** namedtuple类实例占用的空间和tuple相同，因为区域名储存在类中，占用空间比普通对象小，因为属性不会储存在每个实例的\__dict\__中。  
