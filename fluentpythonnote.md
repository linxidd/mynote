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
 ` >>> b, a = a, b `  
 ` _ ` 解包时候用作占位符。 
**使用 * 获取剩余的元素**  
平行赋值(Python3)  
 ` >>> a, b, \*rest = range(5) `  
 ` * ` 可以出现在任何位置  
pep3113 Removal of Tuple Parameter Unpacking  
S
#### 命名元祖  

` collections.namedtuple `  
例1-1中 
` Card = collections.namedtuple('Card', ['rank', 'suit']) `  
**命名元祖** namedtuple类实例占用的空间和tuple相同，因为区域名储存在类中，占用空间比普通对象小，因为属性不会储存在每个实例的\__dict\__中。  

### 切片 
  
#### 切片为什么不包含最后一个元素  
- 可以方便的辨认切片和range的长度 
- 可以方便的计算切片和range的长度 结束-开始  
- 可以很简单的把序列从x处分隔开，而不会重叠:` my_list[:x] `` my_list[x:] `
  
#### 切片赋值  
` >>> l[2:5] = 100 `  
不合法因为再给切片赋值时右边必须为可迭代对象。  

#### 列表中的列表  
```
>>> board = [['_'] * 3 for i in range(3)]
>>> board[1][2] = '0'
>>> board
[['_', '_', '_'], ['_', '_', '0'], ['_', '_', '_']]
```
```
>>> weird_board = [['_'] * 3] * 3
>>> weird_board[1][2] = '0'
>>> weird_board
[['_', '_', '0'], ['_', '_', '0'], ['_', '_', '0']]
```
#### 元祖+=

```
>>> t = （1, 2, [20, 30])
>>> t[2] += [50, 60]
```
t变成(1, 2, [20, 30, 50, 60]) ，而且会抛出TypeError 错误
`dis.dis('s[a] += b')` 可以获取到bytecode。  
- 在元祖中使用列表元素不好  
- 增量赋值不是一个原子操作  
- 检查Python的字节代码并不难而且很有用  

### list.sort和sorted内建函数  
**重要的Python API约定**：如果函数和方法直接改变对象那么应该返回None使调用者明白对象发生了变化，没有生成新的对象。