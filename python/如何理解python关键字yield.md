#[What does the yield keyword do in Python?](http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-pytho])  
#[如何理解python的yield关键字](http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-pytho])
<br/>
<br/>
To understand what yield does, you must understand what generators are. And before generators come iterables.    
<br/>
想要理解python里yield，你必须要理解什么是生成器(generators)。理解生成器之前首先要知道什么是可迭代对象(iterables)。
<br/>
<br/>

##Iterables
When you create a list, you can read its items one by one, and it's called iteration:  

```python
>>> mylist = [1, 2, 3]
>>> for i in mylist:
...    print(i)
1
2
3
```

Mylist is an iterable. When you use a list comprehension, you create a list, and so an iterable:

```python
>>> mylist = [x*x for x in range(3)]
>>> for i in mylist:
...    print(i)
0
1
4
```  
Everything you can use "for... in..." on is an iterable: lists, strings, files... These iterables are handy because you can read them as much as you wish, but you store all the values in memory and it's not always what you want when you have a lot of values.
<br/>
<br/>
##可迭代对象
当你创建一个列表(list)时，你可以逐个读取每一项，这就叫做迭代(iteration)。

```python
>>> mylist = [1, 2, 3]
>>> for i in mylist:
...    print(i)
1
2
3
```
Mylist 就是一个可迭代的对象，当你使用列表推导式(list comprehension)生成一个列表的时候，你创建的也是个可迭代对象。    
任何你可以使用 "for...in..." 的都是可迭代对象，比如 列表(lists)，字符串(string)，文件(files)... 这些可迭代对象(iterables)使用非常方便，因为你可以按你所想无限次读取，但是你却是把(列表里的)所有数值都存在了内存里。当你有大量的数值时，你可不想这么做。
<br/>
<br/>


##Generators

Generators are iterators, but you can only iterate over them once. It's because they do not store all the values in memory, they generate the values on the fly:

```python
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator:
...    print(i)
0
1
4
```
It is just the same except you used () instead of []. BUT, you can not perform for i in mygenerator a second time since generators can only be used once: they calculate 0, then forget about it and calculate 1, and end calculating 4, one by one.

##生成器
生成器也是迭代器，但是你只能**迭代一次**。因为他们并非把他们存在内存中，而是用到的时候才生成:

```python
>>> mygenerator = (x*x for x in range(3))
>>> for i in mygenerator:
...    print(i)
0
1
4
```
你用()替换[]结果还是一样的。但是你不能第二次执行```for i in mygenerator```，因为生成器只能被执行一次: 首先计算出了 0，然后计算出1，一个接一个，直到最后计算出4。


##Yield

Yield is a keyword that is used like return, except the function will return a generator.

```python
>>> def createGenerator():
...    mylist = range(3)
...    for i in mylist:
...        yield i*i
...
>>> mygenerator = createGenerator() # create a generator
>>> print(mygenerator) # mygenerator is an object!
<generator object createGenerator at 0xb7555c34>
>>> for i in mygenerator:
...     print(i)
0
1
4
```
Here it's a useless example, but it's handy when you know your function will return a huge set of values that you will only need to read once.

To master yield, you must understand that when you call the function, the code you have written in the function body does not run. The function only returns the generator object, this is a bit tricky :-)

Then, your code will be run each time the for uses the generator.

Now the hard part:

The first time the for calls the generator object created from your function, it will run the code in your function from the beginning until it hits yield, then it'll return the first value of the loop. Then, each other call will run the loop you have written in the function one more time, and return the next value, until there is no value to return.

The generator is considered empty once the function runs but does not hit yield anymore. It can be because the loop had come to an end, or because you do not satisfy a "if/else" anymore.
<br/>
#Yield
这个例子没什么实际用途，但是可以有助你理解，你的函数将返回大量的值并且你只能读取一次。  
想要深入理解 yield， 你首先要明确当你调用函数的时候，你函数体内的代码并不会运行，仅仅是返回生成器对象而已，有点奇怪是不是 :-)
当你每次使用生成器的时候，你(函数体内)的代码才会执行。  
下面是关键部分了：  
第一次你调用你函数产生的生成器对象，你的代码将从函数的开头被执行，直到遇到yield为止，然后它会返回循环的第一个值。接着以后每次调用都会循环刚才的代码，返回下一个值，直到没有值可以返回。
如果你的函数在执行的时候，一次yield也没执行到，那么最后产生的生成器变是空得。可能是因为你的循环结束了或者是不符合你的"if/else"条件。
<br/>
<br/>

You can stop here, or read a little bit to see a advanced use of generator:
#Controlling a generator exhaustion
```python
>>> class Bank(): # let's create a bank, building ATMs
...    crisis = False
...    def create_atm(self):
...        while not self.crisis:
...            yield "$100"
>>> hsbc = Bank() # when everything's ok the ATM gives you as much as you want
>>> corner_street_atm = hsbc.create_atm()
>>> print(corner_street_atm.next())
$100
>>> print(corner_street_atm.next())
$100
>>> print([corner_street_atm.next() for cash in range(5)])
['$100', '$100', '$100', '$100', '$100']
>>> hsbc.crisis = True # crisis is coming, no more money!
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> wall_street_atm = hsbc.create_atm() # it's even true for new ATMs
>>> print(wall_street_atm.next())
<type 'exceptions.StopIteration'>
>>> hsbc.crisis = False # trouble is, even post-crisis the ATM remains empty
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> brand_new_atm = hsbc.create_atm() # build a new one to get back in business
>>> for cash in brand_new_atm:
...    print cash
$100
$100
$100
$100
$100
$100
$100
$100
$100
...
```
It can be useful for various things like controlling access to a resource.

你现在就可以不用往下看了，或者在了解点生成器(generator)的一些高级用法
#控制生成器的穷举
```python
>>> class Bank(): # 来，我们创建个银行，造些自动取款机
...    crisis = False
...    def create_atm(self):
...        while not self.crisis:
...            yield "$100"
>>> hsbc = Bank() # 一切就绪后，你想要多少自动取款机就给多少
>>> corner_street_atm = hsbc.create_atm()
>>> print(corner_street_atm.next())
$100
>>> print(corner_street_atm.next())
$100
>>> print([corner_street_atm.next() for cash in range(5)])
['$100', '$100', '$100', '$100', '$100']
>>> hsbc.crisis = True # 金融危机来了，没有更多的钱了
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> wall_street_atm = hsbc.create_atm() # 即使是新的自动取款机也是这样(没钱)
>>> print(wall_street_atm.next())
<type 'exceptions.StopIteration'>
>>> hsbc.crisis = False # 好，问题来了，金融危机过后自动取款机还是没钱
>>> print(corner_street_atm.next())
<type 'exceptions.StopIteration'>
>>> brand_new_atm = hsbc.create_atm() # 新建一个让它恢复正常运转
>>> for cash in brand_new_atm:
...    print cash
$100
$100
$100
$100
$100
$100
$100
$100
$100
...
```
<br/>
<br/>
#Itertools, your best friend

The itertools module contains special functions to manipulate iterables. Ever wish to duplicate a generator? Chain two generators? Group values in a nested list with a one liner? Map / Zip without creating another list?

Then just ```import itertools```.

An example? Let's see the possible orders of arrival for a 4 horse race:

```python
>>> horses = [1, 2, 3, 4]
>>> races = itertools.permutations(horses)
>>> print(races)
<itertools.permutations object at 0xb754f1dc>
>>> print(list(itertools.permutations(horses)))
[(1, 2, 3, 4),
 (1, 2, 4, 3),
 (1, 3, 2, 4),
 (1, 3, 4, 2),
 (1, 4, 2, 3),
 (1, 4, 3, 2),
 (2, 1, 3, 4),
 (2, 1, 4, 3),
 (2, 3, 1, 4),
 (2, 3, 4, 1),
 (2, 4, 1, 3),
 (2, 4, 3, 1),
 (3, 1, 2, 4),
 (3, 1, 4, 2),
 (3, 2, 1, 4),
 (3, 2, 4, 1),
 (3, 4, 1, 2),
 (3, 4, 2, 1),
 (4, 1, 2, 3),
 (4, 1, 3, 2),
 (4, 2, 1, 3),
 (4, 2, 3, 1),
 (4, 3, 1, 2),
 (4, 3, 2, 1)]
```
#Itertools, 你最好的朋友
itertools 模块里包含很多特殊的函数来控制可迭代对象。想过复制一个生成器吗？链接两个生成器？一行代码就把嵌套列表分组？ Map/Zip 不需要额外创建额外的列表？  
只要 ```import itertools```就行了。  
需要个例子？那好我们来看看四匹马排名顺序的所有可能组合。  
```python
>>> horses = [1, 2, 3, 4]
>>> races = itertools.permutations(horses)
>>> print(races)
<itertools.permutations object at 0xb754f1dc>
>>> print(list(itertools.permutations(horses)))
[(1, 2, 3, 4),
 (1, 2, 4, 3),
 (1, 3, 2, 4),
 (1, 3, 4, 2),
 (1, 4, 2, 3),
 (1, 4, 3, 2),
 (2, 1, 3, 4),
 (2, 1, 4, 3),
 (2, 3, 1, 4),
 (2, 3, 4, 1),
 (2, 4, 1, 3),
 (2, 4, 3, 1),
 (3, 1, 2, 4),
 (3, 1, 4, 2),
 (3, 2, 1, 4),
 (3, 2, 4, 1),
 (3, 4, 1, 2),
 (3, 4, 2, 1),
 (4, 1, 2, 3),
 (4, 1, 3, 2),
 (4, 2, 1, 3),
 (4, 2, 3, 1),
 (4, 3, 1, 2),
 (4, 3, 2, 1)]
```
<br/>
<br/>

# Understanding the inner mechanisms of iteration

Iteration is a process implying iterables (implementing the __iter__() method) and iterators (implementing the __next__() method). Iterables are any objects you can get an iterator from. Iterators are objects that let you iterate on iterables.

More about it in this article about [how does the for loop work](http://effbot.org/zone/python-for-statement.htm).

# 理解迭代(iteration)内部机制
迭代是对可迭代对象实现了 \_\_iter\_\_()方法 和迭代器实现 \_\_next\_\_()方法 的一个操作过程。 可迭代对象是任何你可以从中获取迭代器的对象。 迭代器是可以让你对可迭代对象进行迭代的对象。

想要继续了解可以参看这篇文章[how does the for loop work](http://effbot.org/zone/python-for-statement.htm)。




