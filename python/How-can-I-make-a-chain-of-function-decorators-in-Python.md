[How can I make a chain of function decorators in Python?](http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python)

[如何给一个函数加多个装饰器修饰?](http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python)


How can I make two decorators in Python that would do the following?

```python
@makebold
@makeitalic
def say():
   return "Hello"
```
which should return

```<b><i>Hello</i></b>```
I'm not trying to make HTML this way in a real application, just trying to understand how decorators and decorator chaining works.


我如何用像下面那样添加两个装饰器

```python
@makebold
@makeitalic
def say():
   return "Hello"
```
(上面的代码)应该返回如下

```<b><i>Hello</i></b>```  
我并不是想要在一个实际的应用中，按照上面的方式生成HTML，只是为了方便理解装饰器和多个装饰器的运行机制。