typing官方文档：https://docs.python.org/zh-cn/3.12/library/typing.html

mypy官方文档：https://mypy.readthedocs.io/en/stable/cheat_sheet_py3.html

这里仅对typing的基本使用做简单描述，更加详细的内容可以查看官方文档、源码或者大佬写的文档。

**大佬文档**：https://zhuanlan.zhihu.com/p/464979921

## 常用类型

下面是一些简单的常用类型，都是可以通过一句话进行简单描述的类型，所以整理为一个表格。一些比较复杂的类型会在文档后面进行赘述。

| 类型名                                                       | 作用                                                         | 使用方式                                                     | 支持的python版本                                             | 建议                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ | ------------------------------------------------------------ |
| int, str, bool, bytes, float, setint, str, bool, bytes, float, set | 表明参数为基本数据类型                                       | def test(x: int) -> int:    pass                             | 3.6+                                                         |                                                              |
| Type Alias(类型别名)                                         | 将类型赋值为指定的别名，可互换的同义词                       | second = floatsecond: TypeAlias = floattype second = float   | second = float(3.6+)second: TypeAlias = float(3.10-3.11)type second = float(3.12+) |                                                              |
| List/list                                                    | 列表                                                         | typing.List[int]list[int]                                    | list (python 3.9+)typing.List (python3.6 +)                  |                                                              |
| Dict/dict                                                    | 字典                                                         | typing.Dict[Ktype, Vtype]dict[Ktype, Vtype]                  | dict (3.9+)typing.Dict(3.6+)                                 |                                                              |
| Type Casting(强制类型转换)                                   | 检测器无法正确推断类型，代码正确，强制转换值的类型（并非在运行时强制转换值的类型，而是给检测器一个提示） | return typing.cast(x)                                        | Python 3.6+                                                  |                                                              |
| NewType                                                      | 创建名义上的新类型                                           | UserId = NewType("UserId", int)#正确get_user(UserId(1))#错误，int类型而不是UserIdget_user(1) | Python 3.6+                                                  |                                                              |
| Any                                                          | 参数类型可以为任意                                           | typing.Any                                                   | Python 3.6+                                                  | 一般不建议使用，作用不大。如果必须使用也可用object替代。     |
| Union（联合类型）                                            | 方法的参数或返回值可能是不同的类型                           | typing.Union[A, B, C, D, E]                                  | Python 3.10 -                                                | Python 3.10+可使用：int \| str                               |
| Optional                                                     | 参数类型可以为None或指定类型                                 | typing.Optional[A]                                           | Python 3.10 -                                                | Python 3.10+可使用：None \| str                              |
| Tuple/tuple                                                  | 元组。不可变的元组，固定长度                                 | Tuple[int] #元组仅一个元素Tuple[int, ...] # 可变元素         | typing.Tuple (3.6+)tuple(3.9+)                               | 可使用NamedTuple，使代码更加清晰。                           |
| Never/NoReturn(底类型)                                       | 表示该函数永远不会有返回值。如sys.exit()                     | typing.NoRetruntyping.Never                                  | Never(3.12 +)NoRetrun(3.11 -)                                |                                                              |
| Literal(字面量类型）                                         | Literal 支持整数字面量、byte、Unicode 字符串、布尔值、枚举 (Enum) 以及 None. | typing.Literal["apple", "banana"]                            | python 3.8+                                                  | 一定程度上Literal可以替代Enum, 但Literal并不提供运行时约束   |
| LiteralString（字符串字面量类型）                            | 表示输入的参数不允许进行动态构建。如：f"test, {sqli}"        | typing.LiteralString                                         | python 3.11 +                                                | 低于python 3.11版本的python可用from typing_extensions import LiteralString |
| Generic(泛型)                                                | 泛型。下面详细介绍                                           |                                                              |                                                              |                                                              |
| 类对象类型(type/Type)                                        | 类对象类型                                                   | type[User]typing.Type[User]                                  | type[User] 3.9+typing.Type 3.6+                              | type[User]与直接使用User的区别是User是指类的实例。如：User('li')。type[User]指类。 |

## 抽象基类

下面举例部分常用抽象基类，完整请看官方文档。

| 类型名         | 作用                                                         | 使用方式                                             | 支持的python版本 | 建议                                                         |
| -------------- | ------------------------------------------------------------ | ---------------------------------------------------- | ---------------- | ------------------------------------------------------------ |
| Sequence       | 一个泛型类型注解，用于表示可迭代且支持按索引访问的不可变序列（如列表、元组、字符串等）（仅逻辑上的不可变性）（需实现 `__getitem__` 和 `__len__`）。 | typing.Sequence[float]                               | python 3.6+      | 适用于需要通用性且不修改序列内容的场景                       |
| **Generator**  | 生成器迭代器。（使用 `yield` 关键字）                        | typing.Generator[YieldType, SendType, ReturnType]    | python 3.6+      | 适用于 `yield` 生成数据的情况，使用 `Generator[int, None, None]`。 |
| **`Iterable`** | 迭代类型。仅支持迭代，不保证索引访问（如集合、字典视图）（需实现 `__iter__` 方法）。 | typing.Iterable[str]                                 | python 3.6+      | 适用于 `list`、`set`、`tuple`，以及所有可迭代对象            |
| **`Iterator`** | 表示迭代器（需实现 `__iter__` 和 `__next__` 方法）。         |                                                      |                  | 适用于所有迭代器，比如 `map()`、`filter()`、`zip()`、手写的 `Iterator` 类 |
| **`Mapping`**  | 字典类结构的接口（需实现 `__getitem__`, `__iter__`, `__len__` 等）。 |                                                      |                  |                                                              |
| Callable       | 表示可调用对象（如函数或实现了 `__call__` 的类）。           | typing.Callable[[argtype, argtype, ...], returntype] | Python 3.6+      |                                                              |

## 泛型﻿

第一次接触泛型可能不太理解这是什么，其实这里的泛型就是类似于数学中的假设变量为x，只不过数学中是咱们自己计算x是多少，泛型是让解析器解析类型是什么。﻿至于还是不理解泛型是什么的可用详细看看上面大佬的链接。

1. **TypeVar(类型变量)**

   python 3.12 + 使用方式

   ```
   # 基本使用方式
   def sample[T](population: Sequence[T], size: int) -> list[T]:
       ...
   
   # 限制泛型的类型范围，指定泛型的范围为int, float, complex
   def sum_nums[T: (int, float, complex)](nums: Sequence[T]) -> T:
       ...
   ```

   python 3.11 - 使用方式

   ```
   # 基本使用方式
   T = typing.TypeVar("T")
   def sample(population: Sequence[T], size: int) -> list[T]:
       ...
   
   # 限制泛型的类型范围，指定泛型的范围为int, float, complex
   T = typing.TypeVar("T", int, float, complex)
   def sum_nums(nums: Sequence[T]) -> T:
       ...
   ```

   限制泛型的类型范围也可使用抽象基类，如使用Hashable，限制类型范围为有\_\_hash\_\_()方法的类型。

2. **TypeVarTuple(类型变量元组)**

   接收任意多个类型参数, 使用名称前的单个星号 (`*`) 来声明。

   python3.11+使用方式

   ```
   T = TypeVar("T")
   Ts = TypeVarTuple("Ts")
   
   # 接收任意多个类型参数
   def move_first_element_to_last(tup: tuple[T, *Ts]) -> tuple[*Ts, T]:
       return (*tup[1:], tup[0])
   ```

   python3.12+使用方式

   ```
   # 接收任意多个类型参数
   def move_first_element_to_last[T, *Ts](tup: tuple[T, *Ts]) -> tuple[*Ts, T]:
       return (*tup[1:], tup[0])
   ```

3. **ParamSpec**（参数规范变量）

   用来将一个可调用对象的参数类型转发给另一个可调用对象的参数类型

   python 3.10 - 3.11 使用方式

   ```
   T = TypeVar("T")
   P = ParamSpec("P")
   def add_logging(f: Callable[P, T]) -> Callable[P, T]:
       '''A type-safe decorator to add logging to a function.'''
       def inner(*args: P.args, **kwargs: P.kwargs) -> T:
           logging.info(f'{f.__name__} was called')
           return f(*args, **kwargs)
       return inner
   ```

   python 3.12 + 使用方式

   ```
   def add_logging[T, **P](f: Callable[P, T]) -> Callable[P, T]:
       '''A type-safe decorator to add logging to a function.'''
       def inner(*args: P.args, **kwargs: P.kwargs) -> T:
           logging.info(f'{f.__name__} was called')
           return f(*args, **kwargs)
       return inner
   ```

4. **Generic** （泛型类）

   自定义泛型类

   python 3.7 + 使用方式

   ```
   T = TypeVar('T')
   
   class LoggedVar(Generic[T]):
       ...
   ```

   python 3.12 + 使用方式

   ```
   class LoggedVar[T]:
       ...
   ```

   泛型类具有 [`__class_getitem__()`](https://docs.python.org/zh-cn/3.13/reference/datamodel.html#object.__class_getitem__) 方法，这意味着泛型类可在运行时进行参数化（例如下面的 **`LoggedVar[int]`**）：

   ```
   def zero_all_vars(vars: Iterable[LoggedVar[int]]) -> None:
       for var in vars:
           var.set(0)
   ```

5. 泛型默认值

   python 3.13+使用方式

   ```
   @dataclass
   class Box[T = int]:
       value: T | None = None
   
   # TypeVar...设置方式
   T = TypeVar("T", default=int)
   @dataclass
   class Box(Generic[T]):
   	value: T | None = None
   ```

   

## Self （自引用类型）

python 3.11+ 使用方式：

```
# 基本使用
class Shape:
	def set_scale(self, scale: float) -> Self:
		self.scale = scale
		return self

```

python 3.11- 使用方式（向前引用，但在子类中会存在些问题）

```
# 基本使用
class Shape:
	def set_scale(self, scale: float) -> "Shape":
		self.scale = scale
		return self

# 存在的问题， 返回值类型被推导为 `Shape`，而不是 `Circle`
class Circle(Shape):
    def set_radius(self, r: float) -> "Circle":
        self.radius = r
        return self

Circle().set_scale(0.5)  # 返回值类型被推导为 `Shape`，而不是 `Circle`

# 解决：
class Shape:
	def set_scale[T: "Shape"](self: T, scale: float) -> T:
		self.scale = scale
		return 
```

## 协议类（Protocol）

python 3.8 + 使用方式

```
# 表示实现了math方法的静态鸭子类型，我觉得就是类似于一个抽象基类
class Proto(Protocol):
	def math(self) -> int:
		...

# 复杂案例，“/”表示前面只能是位置参数，*表示后面只能是关键字参数，
class Logger(Protocol):
    def __call__(self, text: str, /, *, verbose: bool = False) -> None: ...
# 匹配（虽然两处 `verbose` 的默认值不同，但具体的默认值是什么不影响匹配），位置参数名不同也是允许的。
def simple_logger(log: str, /, *, verbose: bool = True):
```

