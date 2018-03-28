class: center, middle

# 간결한 코드를 위한
# 파이썬 미세 팁

이승훈

https://earlbread.github.io/peoplefund-startup-tech-challenge-2018-03/

---

## Python Philosophy

```python
>>> import this
```

--

```python
The Zen of Python, by Tim Peters

Beautiful is better than ugly.
Explicit is better than implicit.
Simple is better than complex.
Complex is better than complicated.
Flat is better than nested.
Sparse is better than dense.
Readability counts.
Special cases aren't special enough to break the rules.
Although practicality beats purity.
Errors should never pass silently.
Unless explicitly silenced.
In the face of ambiguity, refuse the temptation to guess.
There should be one-- and preferably only one --obvious way to do it.
Although that way may not be obvious at first unless you're Dutch.
Now is better than never.
Although never is often better than *right* now.
If the implementation is hard to explain, it's a bad idea.
If the implementation is easy to explain, it may be a good idea.
Namespaces are one honking great idea -- let's do more of those!
```

---

## Packing & Unpacking

```python
person = ('Seunghun', 20, 'Seoul')
name = person[0]
age = person[1]
city = person[2]
```

--

* Better

```python
person = ('Seunghun', 20, 'Seoul')
name, age, city = person
```

--

```python
x = 1
y = 2
x, y = y, x  ## x = 2, y = 1
```

--

```python
numbers = [1,2,3,4]

first, *others = numbers  ## 1, [2, 3, 4]
*others, last = numbers  ## [1, 2, 3], 4
first, *others, last = numbers ## 1, [2, 3], 4
```

---

## *args

```python
def multiply(x, y):
    print (x * y)

coords = (10, 20)

## TypeError: multiply() missing 1 required positional argument: 'y'
multiply(coords)
```

--

```python
def multiply(x, y):
    print (x * y)

coords = (10, 20)

## 200
multiply(*coords)
```

--

``` python
def func(*args):
    for arg in args:
        print(arg, )

func(1, 2, 3) ## 1 2 3
```

---

## **kwargs

```python
def print_values(**kwargs):
    for key, value in kwargs.items():
        print("The value of {} is {}".format(key, value))

print_values(my_name="Sammy", your_name="Casey")

## The value of your_name is Casey
## The value of my_name is Sammy
```

--

```python
def some_kwargs(kwarg_1, kwarg_2, kwarg_3):
    print("kwarg_1:", kwarg_1)
    print("kwarg_2:", kwarg_2)
    print("kwarg_3:", kwarg_3)

kwargs = {"kwarg_1": "Val", "kwarg_2": "Harper", "kwarg_3": "Remy"}
some_kwargs(**kwargs)
```

---

## Dictionary update

```python
xs = {'a': 1, 'b': 2} 
ys = {'b': 3, 'c': 4}
zs = {'d': 5, 'e': 6}

for key, value in ys.items():
    xs[key] = value

for key, value in zs.items():
    xs[key] = value
```

--

* Better

```python
xs.update(ys)
xs.update(zs)
```

--

* Python 3.5 Above

```python
xs = {**xs, **ys, **zs}
```

---

## Loop Basic

```python
colors = ['red', 'blue', 'green', 'yellow']
for i in range(len(colors)):
    print(colors[i])
```

--

* Better

```python
for color in colors:
    print(color)
```

---

## Enumerate

```python
colors = ['red', 'blue', 'green', 'yellow']
for i in range(len(colors)):
    print(i, colors[i])
```

--

* Better

```python
for index, color in enumerate(colors):
    print(index, color)
```

---

## Zip

```python
colors = ['red', 'blue', 'green', 'yellow']
fruits = ['apple', 'grape', 'melon', 'banana']
for i in min(len(fruit), len(color))
    print(fruits[i], colors[i])
```

--

* Better

```python
for fruit, color in zip(fruits, colors):
    print(fruit, color)
```

--

## Enumerate with Zip

```python
for index, (fruit, color) in enumerate(zip(fruits, colors)):
    print(index, fruit, color)
```

---

## List Comprehension

```python
numbers = [1, 2, 3, 4, 5]
squares = []

for number in numbers:
    squares.append(number * number)
```

--

* Better

```python
squares = [n * n for n in numbers]
```

--

```python
squares = [n * n for n in numbers if n % 2 == 0]
```

--

```python
matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
squared = [[n * n for n in row] for row in matrix] 
```

---

## List Comprehension

* Complicated 

```python
my_lists = [
    [[1, 2, 3], [4, 5, 6]],
    [[7, 8, 9], [10, 11, 12]],
]
flat = [x for sublist1 in my_lists
        for sublist2 in sublist1
        for x in sublist2]
```

--

* Better

```python
flat = []
for sublist1 in my_lists:
    for sublist2 in sublist1:
        flat.extend(sublist2)
```

---

## Generator Expression

```python
numbers = [1, 2, 3, 4, 5]
sum_of_squares = sum([n * n for n in numbers if n % 2 == 0])
```

--

```python
sum_of_squares = sum((n * n for n in numbers if n % 2 == 0))
```

--

* Better

```python
sum_of_squares = sum(n * n for n in numbers if n % 2 == 0)
```

---

## Dict Comprehension

```python
numbers = [1, 2, 3, 4, 5]
square_dict = {n: n * n for n in numbers}
```

--

## Set Comprehension

```python
numbers = [1,2,3,1,2,3]
numbers_set = {i for i in numbers}  # {1, 2, 3}
```

---

## Read Line Until Line Is Empty

```python
lines = []

with open('test.txt') as f:
    while True:
        line = f.readline()
        if not line:
            break
        lines.append(line.strip())
```

--

```python
lines = []

with open('test.txt') as f:
    for line in iter(f.readline, ''):
        lines.append(line.strip())
```

--

```python
with open('test.txt') as f:
    lines = [line.strip() for line in iter(f.readline, '')]
```

---

## Decorator

```python
def greeting(func):
    def wrapper(*args, **kwargs):
        print('hi')
        ret = func(*args, **kwargs)
        print('bye')
        return ret
    return wrapper

def dummy():
    print("I'm dummy")

>>> dummy = greeting(dummy)

>>> dummy()
hi
I'm dummy
bye
```

--

```python
@greeting
def dummy():
    print("I'm dummy")
```

---

## Decorator
```python
def fibonacci(n):
    if n < 2:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)
```

아주 간단한 피보나치 함수
이 함수의 문제점은 반복되는 코드가 많아서 너무 느림.

--

```python
def fibonacci_memo(n, saved={}):
    if n < 2:
        return 1

    if n in saved:
        return saved[n]

    result = fibonacci_memo(n-1, saved) + fibonacci_memo(n-2, saved)
    saved[n] = result
    return result
```

---

## Decorator

```python
def cache(func):
    saved = {}
    def wrapper(*args):
        if args in saved:
            return saved[args]
        result = func(*args)
        saved[args] = result
        return result
    return wrapper
```

--

```python
@cache
def fibonacci(n):
    if n < 2:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)
```

--

```python
from functools import lru_cache
@lru_cache
def fibonacci(n):
    if n < 2:
        return 1
    return fibonacci(n-1) + fibonacci(n-2)
```

---

## Context Manager

```python
f = open('my_text.txt')

try:
    f.write('hello')
finally:
    f.close()
```

--

```python
with open('my_text.txt') as f:
    f.write('hello')

print(f)  # Closed
```

---

## Context Manager

```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, trace_back):
        self.file_obj.close()

with File('my_text.txt', 'wb') as f:
    f.wirte('hello')
```

--

```python
from contextlib import contextmanager

@contextmanager
def open_file(name):
    f = open(name, 'wb')
    yield f
    f.close()
```

---

## contextlib

```python
try:
    os.remove('somefile.tmp')
except FileNotFoundError:
    pass
```

--

```python
@contextmanager
def ignored(*exceptions):
    try:
        yield
    except exceptions:
        pass

with ignored(FileNotFoundError):
    os.remove('somefile.tmp')
```

--

```python
from contextlib import suppress

with suprress(FileNotFoundError):
    os.remove('somefile.tmp')
```

---

## contextlib

```python
from urllib.request import urlopen

page = urlopen('http://www.python.org')
try:
    for line in page:
        print(line)
finally:
    page.close()
```

--

```python
from contextlib import closing
from urllib.request import urlopen

with closing(urlopen('http://www.python.org')) as page:
    for line in page:
        print(line)
```

---

## contextlib

```python
with open('help.txt', 'w') as f:
    oldstdout = sys.stdout
    sys.stdout = f
    try:
        help(pow)
    finally:
        sys.stdout = oldstdout
```

--

```python
from contextlib import redirect_stdout

with open('my_output.txt', 'w') as f:
    with redirect_stdout(f):
        print('This is output')
```

요런게 있음. contextlib 를 보세요.
설명하기 복잡해서 안하지만 ExitStack 이라는 애도 유용함.
굵직한 내용은 여기까지.

---

## if - else (Switch simulation)

```python
if cond == 'cond_a':
    handle_a()
elif cond == 'cond_b':
    handle_b()
else:
    handle_default()
```

--

```python
func_dict = {
    'cond_a': handle_a,
    'cond_b': handle_b,
}
cond = 'cond_a'
func_dict.get(cond, handle_default)()
```

---

## if - else (Switch simulation)

```python
def dispatch_if(operator, x, y):
    if operator == 'add':
        return x + y
    elif operator == 'sub':
        return x - y
    elif operator == 'mul':
        return x * y
    elif operator == 'div':
        return x / y

>>> dispatch_if('mul', 2, 8)
16
>>> dispatch_if('unknown', 2, 8) None
```

--

```python
def dispatch_dict(operator, x, y):
    return {
        'add': lambda: x + y,
        'sub': lambda: x - y,
        'mul': lambda: x * y,
        'div': lambda: x / y,
    }.get(operator, lambda: None)()
```

---

## Line Continuation
```python
my_very_big_string = "For a long time I used to go to bed early. Sometimes, "
my_very_big_string += "when I had put out my candle, my eyes would close so quickly "
my_very_big_string += "that I had not even time to say “I’m going to sleep."
```

```python
my_very_big_string = """For a long time I used to go to bed early.
    Sometimes, when I had put out my candle, my eyes would close so quickly \
    that I had not even time to say “I’m going to sleep."""
```

--

```python
my_very_big_string = (
    "For a long time I used to go to bed early. Sometimes, "
    "when I had put out my candle, my eyes would close so quickly "
    "that I had not even time to say “I’m going to sleep."
)

파이썬에서는 괄호안에서 개행이 무시되므로 아래같이 쓸 수 있다.
```

---

## Super

* Python 2

```python
class SomeClass(SomeSuper):
    def __init__():
        super(SomeClass, self).__init__()
```

--

* Python 3

```python
class SomeClass(SomeSuper):
    def __init__():
        super().__init__()
```

---

## Pathlib

* Python 2

```python
import os

BASE_DIR = os.path.dirname(os.path.dirname(__file__))
STATIC_ROOT= os.path.join(PROJECT_DIR,'static/')
```

--

* Python 3

```python
from pathlib import Path

BASE_DIR = Path(__file__).resolve().parents[2]
STATIC_ROOT = BASE_DIR / 'dist' / 'static'
```

---

## Format string (Python 3.6 Above)

* Old

```python
'%s %d' % (name, age)
```

--


* New

```python
'{name} {age}'.format(name=name, age=age)
```

요즘엔 이런식. 좀더 explicit 해지고 타입 지정을 할필요 없다는게 좋지만 너무 김.

--

* Python 3.6 Above

```python
f'{name} {age}'
```

--

* [future-fstring](https://github.com/asottile/future-fstrings)

```python
## -*- coding: future_fstrings -*-
f'{name} {age}'
```

---

## Conclusion

```
Beautiful code is short and concise so if you were to give that code to
another programmer, they would say, oh, that’s well written code.
It’s much like if you’re writing a poem.

- Santiago Gonzalez
```

---

## References

[Transforming Code Into Beautiful, Idiomatic Python | Raymond Hettinger][1]

[Raymond Hettinger - Beyond PEP 8 -- Best practices for beautiful intelligible code][2]

[Intermediate Python][3]

[Effective PYTHON][4]

[Fluent Python][5]

[Python Tricks][6]

[The Python Language Reference][7]

[1]: https://www.youtube.com/watch?v=anrOzOapJ2E
[2]: https://www.youtube.com/watch?v=wf-BqAjZb8M
[3]: http://book.pythontips.com/en/latest/#
[4]: https://effectivepython.com/
[5]: http://shop.oreilly.com/product/0636920032519.do
[6]: https://dbader.org/products/python-tricks-book/
[7]: https://docs.python.org/3/reference/index.html
