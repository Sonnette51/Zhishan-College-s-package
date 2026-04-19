## 一、内置数据类型

### 1.1 类型总览

| 分类 | 类型 |
|------|------|
| 文本 | `str` |
| 数值 | `int`, `float`, `complex` |
| 布尔 | `bool`（`True` / `False`） |
| 序列 | `list`, `tuple`, `range` |
| 映射 | `dict` |
| 集合 | `set`, `frozenset` |
| 空值 | `NoneType`（`None`） |

**类型转换（Casting）：**
```python
x = str(3)    # → '3'
y = int(3.9)  # → 3
z = float(3)  # → 3.0
```

---

### 1.2 运算符

**数值运算符：**

| 运算符 | 含义 |
|--------|------|
| `+` `-` `*` `/` | 加减乘除 |
| `//` | 整除（取商） |
| `%` | 取余 |
| `**` | 幂运算 |

**逻辑运算符：**

| 运算符 | 用途 | 注意 |
|--------|------|------|
| `==` `!=` `>` `<` `>=` `<=` | 比较值 | — |
| `and` `or` `not` | 逻辑运算 | ✅ 用这个 |
| `&` `\|` `~` | 位运算 | ❌ 别用于布尔 |
| `is` / `is not` | 判断是否同一对象 | 常用于 `x is None` |
| `in` / `not in` | 判断是否在集合中 | — |

> ⚠️ **`==` 在 Python 中的含义：**
> - 基本标量类型（int/float/str）→ 比较**值**是否相等
> - 基本集合类型（list/dict等）→ 也比较**值**，且支持递归比较
> - 自定义对象 → 默认比较**内存地址**，可通过 `__eq__()` 重载
> - 比较两个浮点数是否相等，用 `math.isclose(a, b)`

---

## 二、集合数据类型对比

| 类型 | 有序 | 可变 | 允许重复 | 创建方式 |
|------|------|------|----------|----------|
| `list` | ✅ | ✅ | ✅ | `[1, 2, 3]` |
| `tuple` | ✅ | ❌ 不可变 | ✅ | `(1, 2, 3)` |
| `set` | ❌ | ✅ | ❌ | `{1, 2, 3}` |
| `dict` | ✅(3.7+) | ✅ | ❌(键不重复) | `{"k": "v"}` |

> 💡 `set` 和 `dict` 底层都用**哈希表**实现，增删查的平均时间复杂度为 **O(1)**。

---

## 三、⭐ List（列表）

### 3.1 基本操作

```python
thislist = ["apple", "banana", "cherry"]

# 访问
thislist[1]    # → "banana"（正向索引从0开始）
thislist[-1]   # → "cherry"（负数索引从末尾算）
thislist[2:5]  # 切片：含头不含尾
```

### 3.2 增删改

```python
# 改
thislist[1] = "blackcurrant"
thislist[1:2] = ["blackcurrant", "watermelon"]  # 范围替换

# 增
thislist.append("orange")       # 末尾追加
thislist.insert(1, "orange")    # 指定位置插入

# 删
thislist.remove("banana")  # 删除第一个匹配项
thislist.pop(2)            # 按索引删，返回被删元素
del thislist[2]            # 按索引删，无返回值
```

> ⚠️ **关于 `del`：** `del` 只删除**引用**，不删除对象本身。对象由垃圾回收器自动销毁。
> ```python
> a = [1, 2, 3]
> b = a        # b 和 a 指向同一个对象
> del a        # 只删除 a 这个引用，对象还在
> print(b)     # → [1, 2, 3]  b 仍然安全
> ```

### 3.3 ⭐ 列表推导式（List Comprehension）

```python
# 基本语法
newlist = [expr for item in collection]

# 带过滤（后置条件 = 过滤）
newlist = [x.upper() for x in fruits if "a" in x]

# 带三元表达式（前置条件 = 转换规则）
newlist = [x.upper() if "a" in x else x for x in fruits]
```

### 3.4 复制列表

> ⚠️ `list2 = list1` 只是**浅拷贝**（引用），修改一个会影响另一个！

```python
# 真正复制的三种方式（等价）
newlist = thislist.copy()
newlist = list(thislist)
newlist = thislist[:]
```

### 3.5 合并列表

```python
list3 = list1 + list2        # 生成新列表
list1.extend(list2)          # 原地扩展 list1
```

---

## 四、Tuple（元组）

```python
thistuple = ("apple", "banana", "cherry")
print(thistuple[1])   # 访问方式与 list 相同
```

元组**不可修改**（immutable）。常用于：函数返回多个值、作为字典的键、保护数据不被修改。

---

## 五、⭐ Set（集合）

```python
thisset = {"apple", "banana", "orange"}

# 遍历（无法按索引访问）
for x in thisset:
    print(x)

# 成员检测
print("banana" in thisset)

# 增删
thisset.add("cherry")
thisset.discard("banana")  # 安全删除（不存在也不报错）
thisset.remove("banana")   # 删除（不存在会报错）
```

### 集合运算

| 操作 | 返回新集合 | 原地修改 | 运算符 |
|------|-----------|---------|--------|
| 并集 | `union()` | `update()` | `\|` |
| 交集 | `intersection()` | `intersection_update()` | `&` |
| 差集 | `difference()` | `difference_update()` | `-` |
| 对称差 | `symmetric_difference()` | `symmetric_difference_update()` | `^` |

---

## 六、⭐ Dict（字典）

```python
# 创建
thisdict = {"name": "俞声", "department": "统计与数据科学系"}

# 从两个列表创建（⭐常用技巧）
thisdict = dict(zip(keys, values))

# 访问
thisdict["name"]

# 增/改单个
thisdict["address"] = "新地址"

# 增/改多个
thisdict.update({"phone": "12345", "address": "新地址"})
```

---

## 七、⭐ 推导式三兄弟

```python
# 列表推导式
listcomp = [expr for value in collection]

# 集合推导式（自动去重）
set_comp  = {expr for value in collection}

# 字典推导式
dict_comp = {key_expr: value_expr for value in collection}
```

**嵌套推导式示例**（按字符串长度分组）：
```python
thislist = ["apple", "banana", "cherry", "orange", "kiwi", "melon"]
len_groups = {
    length: [w for w in thislist if len(w) == length]
    for length in {len(w) for w in thislist}   # 内层：集合推导式获取所有长度
}
```

---

## 八、⭐ 常用序列函数

```python
# enumerate：同时获取索引和值
for index, value in enumerate(collection):
    ...

# zip：将多个序列逐元素配对
dict(zip(keys, values))

# sorted：返回新的已排序列表（不修改原序列）
sorted([7, 1, 2, 6, 0, 3, 2])              # 升序
sorted([7, 1, 2, 6, 0, 3, 2], reverse=True) # 降序
sorted("data science")                       # 字符串也可排序

# reversed：返回反转迭代器（需用 list() 转换才能看到内容）
list(reversed(range(10)))
```

---

## 九、文件读写

### 9.1 读文本文件

```python
# 推荐方式（with 自动关闭文件）
with open("data.txt", encoding='utf-8') as f:
    thislist = [line.rstrip() for line in f.readlines()]
    # .rstrip() 去掉每行末尾的换行符 \n
```

### 9.2 写文本文件

```python
with open('output.txt', mode='w', encoding='utf-8') as f:
    f.write('\n'.join(thislist) + '\n')   # ⭐ 最快：一次性写入
```

### 9.3 字符编码

| 概念 | 说明 |
|------|------|
| 字符集 | 字符 ↔ 编号的映射（如 Unicode） |
| 编码 | 编号 ↔ 二进制的算法（如 UTF-8、GB18030） |

常见编码：UTF-8（国际通用）、GB18030（中国国家强制标准）

> ⚠️ 编码指定错误会导致乱码，读写文件时**始终显式指定 `encoding`**！

---

## 十、⭐ 异常处理

```python
for number in numbers:
    try:
        print(float(number))
    except ValueError:
        print(f"无法转换: {number}")
    except Exception as e:    # Exception 是所有错误的父类
        print(e)
```

常见异常类型：`TypeError`、`ValueError`、`FileNotFoundError`、`KeyError`、`IndexError`

> 💡 严肃的错误日志记录，使用 `logging` 模块而非 `print`。

---

## 十一、Range 与迭代器

```python
# range：惰性序列（数据按需计算，不占内存）
range(start, end, step)   # end 不包含

for i in range(5):        # 0 1 2 3 4
    print(i)

r = range(10, 100, 5)
r[3]          # 可按索引访问
30 in r       # 可用 in 检测
list(r)       # 转为列表
```

**迭代器（Iterator）：** 单向遍历，每次只返回一个元素，节省内存，适合处理超大数据流。`list / tuple / set / dict / str` 都是可迭代对象（Iterable）。

> 💡 进阶：`itertools` 模块提供 `combinations()`、`permutations()`、`groupby()` 等实用工具。

---

## 十二、速查卡

```
四种集合类型
  list    有序可变，允许重复       → 最常用
  tuple   有序不可变，允许重复     → 保护数据 / 多返回值
  set     无序，自动去重           → 成员检测 / 集合运算
  dict    键值对，键不重复         → 快速查找

列表推导式两种条件位置
  后置 if → 过滤（不满足的直接排除）
  前置 if/else → 转换（每个元素都保留，只是值不同）

复制列表
  list2 = list1       ← 浅拷贝（坑！）
  list2 = list1[:]    ← 真正复制（推荐）

del 只删引用，不删对象
  垃圾回收器负责释放内存

文件操作习惯
  始终用 with open(..., encoding='utf-8') as f:
  始终指定编码，避免乱码

set vs dict
  都是哈希表，查找 O(1)
  set  = 只有键，省内存，适合成员检测
  dict = 键值对，适合按键取值
```
