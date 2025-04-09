#                                                                                                C#

与c语言大体相同，以下为具体区别

## 常规

- print 不是 printf

- int a = 5;

​	   print(a);

​       不用加%d

- 开头为void Start ，最后不用return 0；
- 前加public时，可以从任何地方访问，包括类的外部

## 浮点数

- float x ＝1.2457f；


​       小数默认为double，float在小数最后要加f

- float a ＝（11.0/5）；false


​       float a ＝（11f/5）；true

## 函数

- 与C语言不同，函数名不用在开头声明，但必须在所属的类或结构体内部声明

## 数组

- int a = new int[5];( 声明一个数组a有5个元素)

## 类（class）

- C语言中没有

- 一个类是包含了一系列变量和函数的**数据类型**

- 可将 类 类比为工具，里面的函数为这个**工具的功能**

- 类为C#中最基本的构成元素，所有东西都在一个特定的类里

- 定义语法

  class 类名 { 变量与函数 }

- 可继承
- 有一个类叫 Myclass，则可以定义一个函数 Myclass a ，通过这种方法定义的变量成为该类的**实例**

### 类的构造函数Constructor

- 每次我们新建一个类的实例时，其实都是调用了这个类的**构造函数**

- 构造函数本质是返回值为一个类的实例的函数，它只在新建类时使用

- 它的功能主要是**初始化类里面的数据**

- 语法：public 类名 （参数）{代码体}

  例如：public MyClass（int num1，float num2）{...}

- 使用构造函数：new 类名（参数）

  例如：MyClass instance = new MyClass（-1，4.2f）

## 列表（List）

- 数组:类型相同且顺序固定，长度不可变

  List: 类型相同但顺序不固定，长度可动态更改

- 定义：List<类型>名称 = new List<类型>();

  添加元素：list.Add(xxx)

  插入：list.Insert(0.xxx)

  删除：list.Remove(xxx)

  在某一位置：list.RemoveAt(xxx)

  访问：list[i]

  **注**：list 为列表名称，0为插入的位置，xxx为操作的元素

## 枚举（enum）

- 给数字命名

- public enum Direction

  {

  East

  west

  north

  south

  }

  其中East = 0，west = 1.

- 更直观

## C#中的scope

## 字段修饰符

（字段就是变量）

常用修饰符：

- [SerializeField]：让private变量也可以在引擎里显示并修改

- [Hidelnlnspector]：让public变量不在引擎里显示

- [Range(x,y)] 让一个变量以拖动条形式

## 函数重载

- 可接受不同类型的数据

- 只需要再写一个**名字相同**，但**参数不同**的函数即可：

  public int Func1 (int a, int b){}

  public int Func1 (float a){}

（只能是**参数不同**！返回值和函数名必须相同）

## 三元运算符

isGround = hit.collider?true:false

(是否碰到，是则为true，否为false)

## Mathf

为一个结构体，其中使用的都是double类

**Mathf.Lerp**（50，100，0.5）= 75（线性插值）常见用途：制作非线性动画

Vector3 pos = new Vector3(50,0,0);

void Update(){ 

transform.position = Vector3.Lerp(transform.position, pos);

}

**Mathf.Min**(float a, float b)：得到其中较小的值

**Mathf.Clamp**(float v, float min, float max)：将v保持在min到max这个区间内

## 字典（Dictionary）

可以通过一个**“词条”**去查找所对应的**“内容”**

是一个**无序的键值对集合**

例：Dictionary<int, string> dict = new Dictionary<int, string>(); 代表了一个从int对应到string的字典

**添加元素**：dict.Add(key, value)

通过Key访问Value：dict[key]

**删除元素**：dict.Remove(key)

## **ref** **关键字**

C#中函数的参数可以被**ref**关键字修饰。它允许我们在传参时使用**变量的引用**，而非**变量的值**

int a = 1;

AddOne(a);

print(a); // a=1

......

void AddOne(int a){

a++

}

参数“a”是通过值传递的，和函数外的“a”完全没关系



......

int a = 1;

AddOne(ref a);

print(a); // a=2

......

void AddOne(**ref** int a){

a++;

}

有了ref关键字后，参数“a”是通过引用传递的。于是函数内的“a”和函数外的“a”是一个东西了

## 装箱与拆箱

我们有时会在值类型与引用类型之间作转换。这是一个**比较昂贵的操作**，应尽可能减少

装箱（boxing）：从值类型转换到引用类型

​			int a = 9;

​			object o = a;

拆箱（unboxing）：从引用类型转换到值类型

​			object o = 45;

​			int a = o;

# 本质

**.NET**是一个微软为Windows推出的程序开发框架。它包含了**许多常用的类库**和一个称为“Common Language Runtime”（**CLR**）的运行环境；CLR提供了将C#代码翻译成机器指令的功能、内存管理功能（是的，包括GC！）、多线程功能等等。

粗略地说，我们把写好的代码一口气丢给.NET处理就可以了。



.NET虽然很强大，但其主要是运行在Windows上的。于是，为了将.NET适配到多个操作系统，**Mono**诞生了。Mono是.NET的一种多平台实现，或者说是“另外一个.NET的版本”。Mono包含了：C#编译器、Mono Runtime（和CLR类似）、.NET类库和Mono类库。



C#是一门用来开发基于.NET框架的软件的编程语言。（Windows：.NET本体；其他平台：Mono）

Unity作为一个多平台游戏引擎，自然需要Mono的多平台特性。

这也解释了“MonoBahaviour”这个名字的由来：“Mono”指Mono Runtime、“Behaviour”指“物体行为”。所以MonoBehaviour就是“基于Mono Runtime的物体行为脚本”。



**从代码到指令**：

**C#代码**：这是最高层的，人类易懂的语言

**IL/CIL**（Common Intermediate Language，中间语言）：当我们编译C#代码时，编译器会将其转换为跨平台的IL代码。（Windows：.exe）

**机器码**（Machine Code）：当我们运行编译好的程序时，CLR会让一个叫做JIT（Just In Time）的编译器将IL代码编译成当前平台的机器码。这是最底层的，直接被电脑执行的代码

![Screenshot 2024-12-01 210936](C:\Users\wyz2t\Pictures\Screenshots\Screenshot 2024-12-01 210936.png)

# 事件系统

## Action

能够存放的函数必须是**无返回值**的

常用操作：public Action onDamage；

1. 向一个Action中添加一个函数：“+=”

​		比如：onDamage += ScreenFlash；

​		注意，+=右侧的值为想要添加的函数名

​	2. 删除一个函数用 “ -= ”

​	3.调用一个Action中所有的函数：把它当作一个函数来用即可

​		比如：onDamage()

- Action够存放的函数必须是**无返回值**的，但是**可以有参数**
- 每个Action类型的变量中的函数的参数必须都是相同的

​		（ 比如void A(int a)  void B(float b)  void C(float c)，则B和C可以放在同个Action里，而A不行 ）

此时用 public Action<float> onDamage;

​			 onDamage(45.2f);

## Func

**有返回值的函数**  

与Action的唯一区别就是是否有返回值

private Func<int, float> func1; func1中的函数接受1个int类型的变量，并返回float

private Func<string> func2;  func2中的函数不接受变量，返回string

private Func<int, int, int> func3；func3中的函数接受2个int类型的变量，并返回int

# 文件读写

通过File类创建、读取、写入一个文件

重要的API有：

- void **File.WriteAllText**(string path, string contents)

​	将contents写入位于path的文件，若文件不存在，系统会自动创建。此写入会覆盖之前的内容

- string **File.ReadAllText**(string path)

​	读取位于path的文件并返回其中的文本

## **Unity** **的数据路径**

Unity提供了内置的数据路径：Application.dataPath

我们可以使用Application.dataPath+”/文件名.txt” 来存储一个文本文件

例如：File.WriteAllText(Application.dataPath+”/Save.txt”, “Hello World!”);

# 内存

内存中有两个很重要的区域：堆（Heap）和栈（Stack），并不只有这两个

## **堆与栈**

- **堆**是一块较大的区域，其中主要储存我们用**new**关键字创建出的类的实例。它的大小是随着我们的使用不断增长的，最大可达到绝大部分进程可用内存。但是堆的访问相较栈要慢很多
- **栈**是一块较小的区域，其大小是固定的，一般在1-2MB之间。其中主要存储本地变量。栈的访问速度非常快

## GC：垃圾回收机制

C#提供了垃圾回收器（Garbage Collector）来帮助我们清理不需要的内存



堆中的内存是分为3个**Generation**的！

Generation 0：主要为临时变量、刚刚被创建的（较小的）类的实例。**这里的清理频率是最高的**。

Generation 1：这是一个缓冲区，从gen0中“存活下来”的物体会被放入gen1。（这说明它们更常被使用）

Generation 2：从gen1中“存活下来”的物体会被放入gen2。这里主要为长期需要的变量（static变量等），**清理的频率最低**



当第k个generation被GC清理时，所有小于k的generation也都会被一并清理

所以当GC清理gen2的时候可以被认为是一个**“完整的垃圾回收”**



一次垃圾清理分为三个阶段

1. **标记阶段（Marking Phase）**

在这个阶段，GC会找出并标记所有仍被引用的物体

这是通过一个**“引用树”**（Reference Tree）实现的：每一个引用类型的数据都“继承”自一个根节点，而这些物体之间的引用关系构成了一个树状结构

void Mark(objectRef o){

​		if (!InUseList.Exists(o)){

​				InUseList.Add(o);

​				List refs = GetAllChildReferences(o);

​				foreach (objectRef childRef in refs){

​					Mark(childRef);

​				}

​		}

}

可以看到，这是一个递归的操作,我们可以从根节点开始逐渐探索到整个树状结构并将探索到的物体标记为**“被使用”**

 

2.**回收阶段（Collection Phase）**

在这个阶段，GC会从内存中“删除”所有未被引用的物体。（也就是我们上一步没发现的物体）

所谓“删除”，其实**并不是真正地抹去数据**。实际要简单得多：只需要**将该物体所在的内存区域标记为“未使用”**就可以了，这样我们就可以继续使用这块内存做别的事情了。（原来的数据是一直存在的！）



3. **压缩阶段（Compacting Phase）**

经历过回收阶段的清洗之后，内存中会出现许多**空隙**（因为我们“删除”了许多物体）。在这个阶段，GC会把存活下来的

物体重新排列，使得它们都**“沉到堆底”**，从而最小化内存中的空洞。这个过程中物体之间的相对排列顺序是不变的



如何触发？

1. 当可用内存快满时，或内存占用超过GC所设定的某个阈值时
2. 当我们手动调用**GC.Alloc()**方法时

# 数据类型

在C#中，数据类型分为两大类：值类型（value type）和引用类型（reference type）。

它们的区别主要在**数据的存储方式**与**变量之间的数据传递方式**

## **值类型** **Value Type**

**值类型**直接存储数据，并且变量之间是通过拷贝的方式传递的。

比如int是值类型

int a = 7;

int b = a;

b = -1;

很明显，通过修改b的值是不能影响到a的值的。（尽管我们写了int b = a）

这是因为a这个变量中**直接存储**了‘7’这个数据，而b = a实际上是把a中的值**拷贝了一份**再复制给b的。所以b和a没有直接的联系

**值类型的数据是存储在栈上的！**

*****常见的值类型有：所有的基本数据类型(int, float, double, bool, long, char, etc.)、枚举类型（enum）、结构体（struct）

## **引用类型** **Reference Type**

**引用类型**与值类型很不同，它并非直接存储数据本身，而是存储了**指向该数据的地址**。

引用类型的变量之间传递数据时只是**拷贝了其地址**，并没有拷贝数据本身。所以自始至终**只会有一份数据**

Transform是引用类型，考虑以下代码：

Transform tf1 = object1.transform;

Transform tf2 = tf1;

tf2.position = new Vector3(1,0,0);

我们通过修改tf2的position会影响到object1的位置吗？**会！**

这是因为tf1和tf2中只存储了指向object1.transform的**地址**。tf2=tf1这行代码仅拷贝了地址，并没有拷贝object1.transform本身，

所以tf2和tf1指向的是同一个物体

**引用类型本身是存储在栈上的，而引用类型所指向的数据是存储在堆（Heap）上的！**

*****常见的引用类型有：类（class）、字符串（string）、数组（与数组的元素类型无关）、委托（delegate）

# **C#中的string**

string是一种引用类型，但它有许多特别之处，使得它有时会显得和值类型很像

**1.** **不变性**

C#中的string是不变的。这意味着当一个string被创建时，它就**永远不能被改变了**。而我们对string进行的任何修改操作其实都是创建了一个新的字符串变量

例如：string s = “123”; s+=“4”; 我们其实创建了两个string：”123” 和 ”1234”。s这个变量最后指向的是”1234”，而一开始的”123”这个字符串就变成了“垃圾

**2. String Interning**

C#内置了string interning机制。它很像一个“字符串池”，当我们想要引用一个字符串时，如果池中已经有内容相同的字符串，我们可以直接访问该已存在的字符串，不需要创建新的字符串

string str1 = "Hello";

string str2 = "Hello";

bool areEqual = Object.ReferenceEquals(str1, str2);*

这里，areEqual为true！

# **泛型方法** **Generic Methods**

能**让函数“适配”多种类型的输入**

泛型方法可以接受多个类型变量。我们在定义时需要用逗号隔开

在定义一个函数时，我们不仅用圆括号()标记所接受的变量参数，还可以用**尖括号<>**标记接受的类型参数

**T AddGeneric< T>(T a, T b){**

​			**return a+b;**

**}**

这里我们接受了一个类型变量”T”。”T”这个名字是自选的，不过一般的惯例是命名为”T”（Type）

在调用泛型函数的时候，我们需要补充尖括号中的内容：

**float sum = AddGeneric< float>(5f, 7f);**



泛型方法可以接受多个类型变量。我们在定义时需要用逗号隔开

**void Insert<TKey, TValue>(Dictionary<TKey,TValue> dict, TKey key,TValue value){**

​					**dict.Add(key, value);**

**}**

调用时：**Insert<int, string>(dict, 2, “February”);**或**Insert(dict, 2, “February”);**

## 类型限制

有时我们会希望一个函数所接受的泛型变量处于某个范围之内，比如**GetComponent**方法只会接受Component类型的参数

为了解决这个问题，我们可以**在圆括号后使用****where****关键字**对类型变量作出一些限制

where T: struct (T必须为值类型)

where T : class (T必须为引用类型)

where T : new() (T必须有一个无参数的构造函数)

where T : < base class name> (T是继承自一个基类的)

where T : < interface name> (T实现了一个接口)



**T GetComponent< T>()** **where T: Component**{

**T comp;**

**……**

**return comp;**

**}**

限制T继承自Component

## **泛型类** **Generic Classes**

不仅函数支持泛型，类也可以

一个泛型类允许我们在创建该类的实例时传入一个类型参数。思想和泛型方法是一样的

**public class MyList< T> where T: struct{**

​	**List< T> list = new List< T>();**

​	**public void Add(Titem){**

​	**list.Add(item);**

​	**}**

**}**

这是一个**接受值类型**的List类。

在类名的定义之后添加**尖括号**（以及**类型限制**）即可
