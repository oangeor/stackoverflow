[How can I make a chain of function decorators in Python?][1]  
[如何给一个函数加多个装饰器修饰?][1]  
#Question:
How can I make two decorators in Python that would do the following?

```
@makebold
@makeitalic
def say():
   return "Hello"
```
which should return

```<b><i>Hello</i></b>```
I'm not trying to make HTML this way in a real application, just trying to understand how decorators and decorator chaining works.


#我如何用像下面那样添加两个装饰器

```
@makebold
@makeitalic
def say():
   return "Hello"
```
(上面的代码)应该返回如下

```<b><i>Hello</i></b>```  
我并不是想要在一个实际的应用中，按照上面的方式生成HTML，只是为了方便理解装饰器和多个装饰器的运行机制。

#Answer:
Check out [the documentation][2] to see how decorators work. Here is what you asked for:

```
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
```
#回答：
看看[这篇文档][2]来了解多个装饰器(decorators)是如何工作的。以下是你问题的答案：

```
def makebold(fn):
    def wrapped():
        return "<b>" + fn() + "</b>"
    return wrapped

def makeitalic(fn):
    def wrapped():
        return "<i>" + fn() + "</i>"
    return wrapped

@makebold
@makeitalic
def hello():
    return "hello world"

print hello() ## returns <b><i>hello world</i></b>
```

[1]: http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python
[2]: https://docs.python.org/2/reference/compound_stmts.html#function





