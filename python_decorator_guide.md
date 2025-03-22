# Python 装饰器入门：从函数到底层理解

> 一篇专为初学者准备的、结构清晰的装饰器入门教程

---

## 🧠 为什么要学装饰器？

装饰器是 Python 的语法糖，但它背后连接的是函数式编程的核心理念：

- 函数是对象，可以被传递、返回、组合
- 名字只是绑定，函数名其实就是指向函数对象的变量
- `@装饰器` 是 Python 提供的快捷语法，帮助你在函数定义时做包装

---

## 1️⃣ 函数也是对象

```python
def greet():
    print("Hi")

hello = greet
hello()  # 输出 Hi
```

函数可以：
- 被赋值给变量
- 被作为参数传入其他函数
- 被其他函数返回

这就是装饰器的基础。

---

## 2️⃣ 什么是装饰器？

装饰器的目的，是在不修改原函数代码的前提下，为函数增加额外的行为。本质上，它是一个接收函数、返回新函数的“函数包函数”结构。

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        print("调用前")
        result = func(*args, **kwargs)
        print("调用后")
        return result
    return wrapper
```

使用方式：

```python
@decorator
def say_hi():
    print("Hi")

say_hi()
```

等价于：
```python
def say_hi():
    print("Hi")

say_hi = decorator(say_hi)
```

---

## 3️⃣ 什么是 wrapper？为什么需要它？

wrapper 是你包裹原始函数的一层函数：
- 它接受原函数的参数
- 可以在执行前后插入逻辑
- 返回值通常是 `func(*args, **kwargs)`

如果你在装饰器里直接写：
```python
def decorator(func):
    return func()
```
你就**提前执行了函数**，而不是返回一个函数。

✅ 正确做法是：返回一个可以延迟执行的函数，即 `wrapper`。

---

## 4️⃣ 多层装饰器的顺序？

```python
def A(func):
    def wrap(*a, **k):
        print("A")
        return func(*a, **k)
    return wrap

def B(func):
    def wrap(*a, **k):
        print("B")
        return func(*a, **k)
    return wrap

@A
@B
def hello():
    print("Hello")

hello()
```
输出：
```
A
B
Hello
```
执行顺序是：`hello = A(B(hello))`

---

## 5️⃣ 最小可用装饰器模板

```python
def decorator(func):
    def wrapper(*args, **kwargs):
        return func(*args, **kwargs)
    return wrapper
```

你可以在 wrapper 里：
- 打印日志
- 捕获异常
- 添加功能

---

## 6️⃣ 注册器实战（指令表）

```python
# 注册器：用来保存动作名 -> 函数 的映射关系
registry = {}

# 装饰器：接收名字，返回装饰器函数
def register(name):
    def deco(func):
        registry[name] = func  # 将函数注册到字典中
        return func            # 返回原函数不做改动
    return deco

# 使用装饰器注册一个函数，名字为 "say"
@register("say")
def say_hi():
    print("Hi")

# 根据字符串名字调用函数（类似命令解析）
registry["say"]()  # 输出 Hi
```

---

## ✅ 总结一句话

> @装饰器，其实就是把函数名重新绑定为一个“包裹后”的函数。

它让你在函数定义时，就能加入统一逻辑。


