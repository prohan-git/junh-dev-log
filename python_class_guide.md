# Python 中的类、self 与继承


---

## 🧠 类的本质：一个定制对象的工厂

在 Python 中，一个类（`class`）本身是一个对象，它也是创建一个“定制对象”的**工厂**。

```python
class Person:
    def __init__(self, name):
        self.name = name
```

这里的 `__init__` 方法其实就是你定制**生成对象时做什么**的入口。

当你写：
```python
p = Person("Alice")
```
Python 实际做了三件事：
1. 创建一个空对象（内存分配）
2. 把这个对象作为 `self`，传给 `__init__`
3. 执行 `self.name = name`

可以理解为：
```python
obj = object.__new__(Person)   # 创建实例（底层）
Person.__init__(obj, "Alice")  # 初始化
```

---

## 🔍 self 是什么？为什么要写在方法里？

你写的所有实例方法（非 `@staticmethod`），第一个参数默认都是 `self`。

**self 不是关键字，只是习惯名称**，它代表的是“调用这个方法的那个对象”。

```python
class Dog:
    def bark(self):
        print(f"🐶 {self} 在叫！")
```

当你执行：
```python
d = Dog()
d.bark()
```
Python 实际执行的是：
```python
Dog.bark(d)
```

✅ 所以你写的方法 `def bark(self):`，其实就是你定义一个接收“对象本身”参数的函数。

---

## 🧪 举例：我们造一个“自我介绍工厂”

```python
class Human:
    def __init__(self, name, age):
        self.name = name
        self.age = age

    def introduce(self):
        print(f"我是 {self.name}，今年 {self.age} 岁。")

h = Human("小明", 30)
h.introduce()
```

输出：
```
我是 小明，今年 30 岁。
```

你写的 `__init__` 就是构造过程；你写的 `introduce` 就是给对象加的功能。

---


## 🧰 继承：我们能造“工厂的工厂”

```python
class Animal:
    def move(self):
        print("移动中...")

class Dog(Animal):
    def bark(self):
        print("汪汪！")

x = Dog()
x.move()  # ✅ 来自父类
x.bark()  # ✅ 自己的方法
```

继承让我们：
- 重复利用已有的工厂逻辑（父类）
- 扩展或覆盖其中的功能（子类）

---

## 💡 self 是绑定对象，cls 是绑定类

```python
class Tool:
    def instance_method(self):
        print(f"来自实例 {self}")

    @classmethod
    def class_method(cls):
        print(f"来自类 {cls}")
```

区别：
- `instance_method` 自动接收实例对象（self）
- `class_method` 自动接收类本身（cls）

---

## 🧬 继承内置类型 list 举例

```python
class MyList(list):
    def sum(self):
        return sum(self)

ml = MyList([1, 2, 3])
print(ml.sum())  # 输出 6
```

你可以添加方法，甚至重写 list 的方法：
```python
    def append(self, item):
        print(f"📌 正在追加: {item}")
        super().append(item)
```

掌握了 self，你就理解了“对象”；理解了继承，你就能做出更复杂、可复用的“工厂系统”。

