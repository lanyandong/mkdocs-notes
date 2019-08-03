## Python3基本数据类型

### Number（数字）

Python 3支持int、float、bool、complex（复数）。他们是不可改变的数据类型，这意味着改变数字数据类型会分配一个新的对象。

- Python可以同时为多个变量赋值，如a, b = 1, 2。
- 一个变量可以通过赋值指向不同类型的对象。
- 数值的除法包含两个运算符：/ 返回一个浮点数，// 返回一个整数。
- 在混合计算时，Python会把整型转换成为浮点数。

### String（字符串）

Python中的字符串用单引号 ' 或双引号 " 括起来，同时使用反斜杠 \ 转义特殊字符。

- 反斜杠可以用来转义，使用r可以让反斜杠不发生转义。
- 字符串可以用+运算符连接在一起，用*运算符重复。
- Python中的字符串有两种索引方式，从左往右以0开始，从右往左以-1开始。
- Python中的字符串不能改变。

### List（列表）

列表中元素的类型可以不相同，它支持数字，字符串甚至可以包含列表（所谓嵌套）。列表是写在方括号 [] 之间、用逗号分隔开的元素列表。和字符串一样，列表同样可以被索引和截取，列表被截取后返回一个包含所需元素的新列表。
Python中的列表是由对其它对象的引用组成的连续数组。指向这个数组的指针及其长度被保存在一个列表头结构中。这意味着，每次添加或删除一个元素时，由引用组成的数组需要该标大小（重新分配）。幸运的是，Python在创建这些数组时采用了指数分配，所以并不是每次操作都需要改变数组的大小。但是，也因为这个原因添加或取出元素的平摊复杂度较低。

- List写在方括号之间，元素用逗号隔开。
- 和字符串一样，list可以被索引和切片。
- List可以使用+操作符进行拼接。
- List中的元素是可以改变的。

### Tuple（元组）

元组（tuple）与列表类似，不同之处在于元组的元素不能修改。元组写在小括号 () 里，元素之间用逗号隔开。

- 与字符串一样，元组的元素不能修改。
- 元组也可以被索引和切片，方法一样。
- 注意构造包含 0 或 1 个元素的元组的特殊语法规则。
- 元组也可以使用+操作符进行拼接。

### Set（集合）

集合（set）是一个无序不重复元素的集。基本功能是进行成员关系测试和消除重复元素。可以使用大括号 或者 set()函数创建set集合，注意：创建一个空集合必须用 set() 而不是 { }，因为{ }是用来创建一个空字典。集合被实现为带有空值的字典。

- set集合中的元素不重复，重复了它会自动去掉。
- set集合可以用大括号或者set()函数创建，但空集合必须使用set()函数创建。
- set集合可以用来进行成员测试、消除重复元素。

### Dictionary（字典）

字典是一种映射类型（mapping type），它是一个无序的键 : 值对集合。关键字必须使用不可变类型，也就是说list和包含可变类型的tuple不能做关键字。在同一个字典中，关键字还必须互不相同。使用伪随机探测(pseudo-random probing)的散列表(hash table)作为字典的底层数据结构

- 字典是一种映射类型，它的元素是键值对。

- 字典的关键字必须为不可变类型，且不能重复。

- 创建空字典使用{ }。

  

## Python三种控制结构

1. **if语句（elif）**：严格以缩进区别语块，缩进反映代码逻辑性，缩进可由任意的空格和制表符组成，同一个语句块的缩进必须保持一致。
2. **for语句**：for循环通常用来遍历有序的序列对象内的元素。For循环语句——range函数的应用（Python的range函数通常用来生产整数列表）。
3. **while语句**：（判断bool，continue语句，break语句）



## Python 常用库

**标准库**：os 操作系统，time 时间，random 随机，pymysql 连接数据库，threading 线程，multiprocessing进程，queue 队列。

**第三方库**：django 和 flask 也是第三方库，requests，virtualenv，selenium，scrapy，xadmin，celery，re，hashlib，md5。

**科学计算库**：Numpy，Scipy，Pandas等。

**常用网址：**

* PyQt官方文档：http://pyqt.sourceforge.net/Docs/PyQt5/introduction.html
* Django官方文档：https://docs.djangoproject.com/en/2.0/
* flask官方文档：http://flask.pocoo.org/docs/1.0/
* numpy官方文档：https://docs.scipy.org/doc/numpy/reference/
* Scipy官方文档：https://docs.scipy.org/doc/scipy/reference/
* pandas官方文档：http://pandas.pydata.org/pandas-docs/stable/
* matplotlib官方文档：https://matplotlib.org/contents.html#
* requests中文文档：http://docs.python-requests.org/zh_CN/latest/
* BeautifulSoup中文文档：https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/
* scikit-image官方文档：http://scikit-image.org/docs/stable/
* pillow官方文档：https://pillow.readthedocs.io/en/5.1.x/

