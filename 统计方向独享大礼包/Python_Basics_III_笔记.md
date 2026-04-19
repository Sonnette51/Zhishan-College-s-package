
## 一、JSON 文件

### 1.1 什么是 JSON？

JSON（JavaScript Object Notation）是最通用的**结构化数据交换格式**。

常见场景：调用 Web API · 读取数据文件 · 存储配置 · 保存机器学习结果

### 1.2 JSON ↔ Python 类型对照

| JSON | Python |
|------|--------|
| object | `dict` |
| array | `list` |
| string | `str` |
| number | `int` / `float` |
| true / false | `True` / `False` |
| null | `None` |

> 💡 **核心思想**：JSON 本质上就是"用文本写成的 Python 字典 + 列表"。

### 1.3 JSON 语法规则

- 键**必须**是字符串
- 字符串使用**双引号**（不能用单引号）
- **不能**有尾随逗号
- 支持嵌套

```json
{
  "name": "俞声",
  "phd_year": 2012,
  "is_teacher": true,
  "courses": ["数据科学编程", "深度学习"]
}
```

---

### 1.4 ⭐ 四个核心函数

| 函数                  | 方向                 | 说明                 |
| ------------------- | ------------------ | ------------------ |
| `json.loads(s)`     | JSON字符串 → Python对象 | load string        |
| `json.load(f)`      | JSON文件 → Python对象  | load file          |
| `json.dumps(obj)`   | Python对象 → JSON字符串 | dump to **s**tring |
| `json.dump(obj, f)` | Python对象 → JSON文件  | dump to file       |

**记忆技巧**：有 `s` 的处理字符串，没 `s` 的处理文件。

```python
import json

# ── 读 ──────────────────────────────────
# 从字符串读
data = json.loads('{"name": "俞声", "age": 30}')

# 从文件读
with open('data.json', mode='r', encoding='utf-8') as f:
    data = json.load(f)

# ── 写 ──────────────────────────────────
# 转为字符串（ensure_ascii=False 保留中文，indent=2 格式化缩进）
text = json.dumps(data, ensure_ascii=False, indent=2)

# 写入文件
with open('data.json', mode='w', encoding='utf-8') as f:
    json.dump(data, f, ensure_ascii=False, indent=2)
```

---

### 1.5 JSONL（JSON Lines）格式

每一行都是一个独立的 JSON 对象，行与行之间相互独立。

| 格式    | 结构         | 适用场景             |
| ----- | ---------- | ---------------- |
| JSON  | 单个对象或数组    | 配置文件、API响应、小型数据集 |
| JSONL | 每行一个JSON对象 | 日志、大型数据集、流式数据    |

```python
# ── 读 JSONL ────────────────────────────
data = []
with open('input.jsonl', mode='r', encoding='utf-8') as f:
    for line in f:
        obj = json.loads(line)   # 逐行解析
        data.append(obj)

# ── 写 JSONL ────────────────────────────
with open('output.jsonl', mode='w', encoding='utf-8') as f:
    for row in data:
        f.write(json.dumps(row, ensure_ascii=False) + "\n")
```

> 💡 JSONL 优势：无需一次性加载全部数据，适合处理**超大文件**和机器学习数据集。

---

## 二、函数

### 2.1 基本定义

```python
def my_func(x, y):
    return x + y

my_func(3, 5)   # → 8
```

### 2.2 参数类型

```python
# 默认参数
def greet(name="同学"):
    return "Hello, " + name

greet()          # → "Hello, 同学"
greet("梅林")   # → "Hello, 梅林"

# 混用位置参数和关键字参数（位置参数必须在前）
def describe(animal, name, age):
    print(f"I have a {age} year old {animal} named {name}")

describe("cat", age=16, name="猫日")
```

---

### 2.3 ⭐ *args 与 **kwargs

#### `*args` — 接收任意数量的**位置参数**，函数内为 `tuple`

```python
def my_func(*args):
    print(type(args))    # <class 'tuple'>
    print(args[0])       # 第一个参数

my_func("唐伯虎", "孔仲尼", "秦叔宝")

# 与普通参数混用（普通参数必须在前）
def greet_all(greeting, *names):
    for name in names:
        print(greeting, name)

greet_all("早上好", "唐伯虎", "孔仲尼", "秦叔宝")
```

#### `**kwargs` — 接收任意数量的**关键字参数**，函数内为 `dict`

```python
def my_func(**kwargs):
    print(type(kwargs))       # <class 'dict'>
    print(kwargs["name"])
    print(kwargs["age"])

my_func(species="cat", age=16, name="猫日")
```

> 💡 `*` 和 `**` 才是关键，名字叫 `args`/`kwargs` 只是惯例。

---

### 2.4 返回多个值

Python 函数可以返回多个值（本质是返回一个 `tuple`），并支持**解包**：

```python
def get_info():
    return "猫日", 15, "cat"   # 返回 tuple

name, age, species = get_info()   # 解包
print(name, age, species)         # 猫日 15 cat
print(get_info())                 # ('猫日', 15, 'cat')
```

---

### 2.5 函数是对象

Python 中函数可以像变量一样传递：

```python
clean_ops = [str.strip, str.title]   # 函数存入列表

def clean_strings(strings, *ops):
    result = []
    for value in strings:
        for func in ops:
            value = func(value)     # 调用传入的函数
        result.append(value)
    return result

states = [" Alabama ", "georgia", "FlOrIda"]
clean_strings(states, str.strip, str.title)
# → ['Alabama', 'Georgia', 'Florida']
```

---

### 2.6 ⭐ Lambda（匿名函数）

语法：`lambda 参数 : 表达式`（只能有**一个**表达式）

```python
x = lambda a, b: a * b
x(5, 6)   # → 30
```

**常与 `map()` / `filter()` / `sorted()` 配合使用：**

```python
numbers = [1, 2, 3, 4, 5]

# map：对每个元素应用函数
doubled = list(map(lambda x: x * 2, numbers))
# → [2, 4, 6, 8, 10]

# filter：过滤满足条件的元素
odds = list(filter(lambda x: x % 2 != 0, numbers))
# → [1, 3, 5]

# sorted：自定义排序键
names = ["唐伯虎", "孔仲尼", "秦叔宝", "四季豆"]
sorted_names = sorted(names, key=lambda x: x.encode('gb18030'))  # 按拼音排序
```

> ⚠️ `.sort()` 是**原地排序**，返回 `None`；`sorted()` 返回**新列表**。

---

## 三、面向对象编程（OOP）

### 3.1 为什么需要 OOP？

项目变大后代码混乱：变量太多、函数太多、难以追踪状态。

**OOP 核心思想**：将**数据 + 行为**封装进单一对象，便于管理。

### 3.2 核心概念

| 概念 | 含义 |
|------|------|
| `class` | 蓝图（模板） |
| object | 类的实例 |
| attribute | 对象内的变量 |
| method | 类内的函数 |

---

### 3.3 ⭐ 创建类与对象

```python
import random

class Monster:
    total_monsters = 0   # ← 类属性：所有实例共享

    def __init__(self, name, dmg_min, dmg_max):   # ← 构造方法
        self.name = name           # 实例属性
        self.dmg_min = dmg_min
        self.dmg_max = dmg_max
        Monster.total_monsters += 1   # 通过类名访问类属性

    def attack(self):              # ← 实例方法（第一个参数必须是 self）
        dmg = random.randint(self.dmg_min, self.dmg_max)
        print(f"{self.name} made {dmg} damage.")
        return dmg

# 创建对象（实例化）—— 不需要传 self
skeleton     = Monster("Skeleton", 1, 3)
black_dragon = Monster("Black Dragon", 40, 50)

# 访问类属性
print(Monster.total_monsters)   # 2

# 调用方法
skeleton.attack()
black_dragon.attack()
```

### 3.4 类属性 vs 实例属性

| | 类属性 | 实例属性 |
|--|--------|----------|
| 定义位置 | 类体内，方法外 | `__init__` 内用 `self.` 定义 |
| 共享方式 | 所有实例共享 | 每个实例独立 |
| 访问方式 | `类名.属性` 或 `self.属性` | `self.属性` |
| 典型用途 | 计数器、全局配置 | 每个对象自己的数据 |

---

## 四、速查卡

```
JSON 四函数
  loads / load   →  JSON → Python
  dumps / dump   →  Python → JSON
  有 s = 字符串，无 s = 文件

函数参数顺序
  普通参数 → *args → **kwargs

Lambda 三件套
  map()     对每个元素变换
  filter()  筛选满足条件的元素
  sorted()  自定义排序（key=lambda）

OOP 三要素
  __init__   初始化实例属性
  self       指向当前对象本身
  类属性     类名.属性 访问，所有实例共享
```
