# Unity

## GameObject与Component

- 每一个场景里存在的东西都是一个GameObject
- 每个GameObject的功能是由它身上的Component组成的
- 平常我们写的代码都是一个依附在GameObject上的Component

 ## Hierarchy与Inspector

分别显示GameObject与Component

## Scene与Game界面

### Scene

- 显示当前场景并编辑物体的Transform

- 操作：1.操作相机：WASD+右键（shift可加快）

  ​			2.移动：W  

  ​			3.旋转：E

  ​			4.缩放：R

  ​			5.方块工具：T

- Gizmos

### Game

- 显示从当前相机的视角看出去的样子
- 无Gizmo

## MonoBehaviour生命周期

一个继承自Mono Behaviour的类有以下的生命周期（部分）：

- **Awake** 在Script被加载时执行一次

- **Start**     在游戏开始运行时执行一次

- **FixedUpdate** 每隔一段时间固定执行一次（默认为0.02s）

- **Update** 每帧之后执行一次

- 其余需要时自行查询



# 几个Unity中重要的类



## class GameObject

- GameObject类包含了一个物体本身的信息，并提供了一些编辑该物体的方法
- 访问该Game Object时，直接写gameObject即可，因为Component类中有定义这个变量，而MonoBehaviour继承自Component
- 常用字段（变量）：name，tag，layer，transform等等
- 常用方法：SetActive，Destroy，Instantiate，AddComponent等等

## class Component

- Component类包括了所有的组件，包括Unity自带的和我们创建的
- MonoBehaviour继承自Component



- 最重要的方法之一：GetComponent<类型>();

  例： SpriteRenderer sr = GetComponent<SpriteRenderer>();

  ​		 sr.color = Color.white;(更改颜色)

  注：比较耗性能，节约用，一般在Awake/Start里使用，不要在Update里一直执行

## struct Vector3

- 代表了一个三位向量，可表示位置，距离，方向等
- 新建：Vector3 v = new Vector3（1.2，1.5f，-5.0f）
- 获取分量：v.x，v.y，v.z
- 内置模板：Vector3.up（表示（0.1.0)的向量），Vector3.left（表示（-1.0.0）的向量），Vector3.forward/back（表示（0，0，1）/（0，0，-1）的向量，Vector3.one(表示（1，1，1)的向量）
- 获取两者之间的距离：Vector3.Distance（v,u）
- 同理，Vector2为两个方向的向量

## class Transform

- 管理GameObject的**位置，旋转与缩放**

- 每一个GameObject都有Transform

- 常用字段：position，rotation，scale

- 常用方法：Translate，GetChild( )(获取物品)  等等

  例：transform.Translate(new Vector(5,0,0));

  ​		transform.position = new Vector3(transform.position.x +5,transform.position.y.transform.position.z);

  ​		(两者等价)
  
  - 具体操作的例子
  
  ![Screenshot_20241020_173049_tv.danmaku.bili](C:\Users\wyz2t\Pictures\Screenshots\Screenshot_20241020_173049_tv.danmaku.bili.jpg)
  
  

![Screenshot_20241020_173030_tv.danmaku.bili](C:\Users\wyz2t\Pictures\Screenshots\Screenshot_20241020_173030_tv.danmaku.bili.jpg)



## class Time

- Time.deltaTime     代表一帧的时间长度（不固定）
- Time.fixedDeltaTime 代表两次FixedUpdate之间的时长（固定）

## class input

- 一定要放入update中，一直检测玩家有无输入

- Input.GetKeyDown(KeyCode.W)    ：检测是否按下w

- Input.GetMouseButtonDown():当按下鼠标按键的瞬间为true

- 变体：

  Input.GetKey（）：但一个按键被按着时为true

  Input.GetKeyUp（）：当抬起一个按键的瞬间为true

  。。。

  

# 物理组件

## Tag

- 每一个GameObject都有一个Tag（默认为Untagged)，可以在编辑器里添加／删除Tag
- Tag可以给单个GameObject加上一个标签，他最大的作用是
     1．用来判断一个物体是不是属于这个分类
     2．用来通过分类寻找该物体
- 访问Tag :    gameObject.tag
  寻找物体：GameObject.FindObjectWithTag

## Layer

- 是一个int
- 只有32个Layer可以用
- 每个GameObject都有
- ayer可以给所有GameObject进行分组，它最大的作用是
  1．让相机只渲染一些Layer
  2．让物理运算只在一些Layer中进行
- 访问Layer: gameObject.layer

## Collider2D

- Collider(碰撞盒)定义了一个物体的物理体积
- 有两种行为模式：Collider（有物理体积）与Trigger（无）
- 使用：给物体添加上某种Collider2D组件即可

## RigiBody2D

- 定义了物体的物理行为

- 3种行为模式：1.Dynamic完全参与物理模拟（掉落物）					     

  ​                         2.Kinematic不会主动移动，需要用代码让它移动（程序驱动的角色移动）

  ​						 3.Static完全不会移动（地面、墙壁）

- 常用的参数：Gravity Scale：这个物体受到重力的大小

​               		        Constraints：锁定位移／旋转
​                               Material：控制弹力与摩擦

## Collision2D 事件函数

-  每当两个物理物体相碰撞时Unity都会发出一个物理事件。我们可以在代码中处理这些物理事件，并实现"在碰撞时做某些事"。

- 暂时地，我们只使用Dynamic类型的刚体。

- 只有有RigidBody组件的物体才能接受物理事件。

- Collision事件函数会在有实际物理体积的碰撞盒碰撞时触发。它们是MonoBehaviour生命周期的一部分！（和Start性质一样）

  ​		void OnCollisionEnter2D()   在刚碰到时触发一次

  ​		void OnCollisionExit2D()    在刚离开时触发一次

  ​		void OnCollisionStay2D()   在碰撞时每帧触发

## OnCollisionEnter2D

- void OnCollisionEnter2D(Collision2D collision)

  这里有一个参数Collision2D collision，其中包含了这次碰撞的所有信息，比如：

  1. collision.collider：  对方的碰撞盒
  2. collision.contacts：接触点的位置
  3. collision.gameObject：对方的gameObject

## Trigger2D 事件函数

- void OnTriggerEnter2D(Collider2D collision)
- void OnTriggerExit2D(Collider2D collision)
- void OnTriggerStay2D(Collider2D collision)
- Trigger事件函数会在无实际物理体积（Trigger）的碰撞盒碰撞时触发，它们也是MonoBehaviour生命周期的一部分
- 与Collision不同，这里的参数是der2D，即对方的碰撞盒

## LayerMask

- 是一个int，代表了一些Layer的集合（如一个layermask可以代表第2，3，15个layer）

  int layerMask = 1<<9 表示第九个

	## 射线检测 Raycast

两个参与者，A发出射线，B被碰撞

其中A不需要碰撞盒，B需要

### Physics2D.Raycast 函数

- 实现射线检测的函数，当碰到第一个物体时停止

- 常用参数：

  RaycastHit2D Raycast(Vector2 origin,Vector2 direction,float distance,int layerMask)

  （orgin:发出位置，direction:射线方向，distance：长度，layerMask：参与的layer）

## PlatformEffector2D组件

- 可实现类似从下面跳到平台上的功能
- 使用时，勾选collider上的Used By Effector选项

# 2D渲染

## Sprite Renderer

- 用来显示一张图片

- 属性：Color、Flip、Material等等（color是染色，只会变深）

## SpriteMask

- 用来实现图片遮罩

- 让一些图片只在另一张图片的范围内显示

- 使用：给遮罩物体添加SpriteMask组件，并设置被遮罩物体上SpriteRenderer的MaskInteraction选项

## Sprite and SpriteEditor

- 每张图片都有一个种类，比如Default、Sprite、Cursor、Normal Map等等（注意如果要用在SpriteRenderer里的话必须选择Sprite类型）
- Sprite Editor是用来编辑图片的、。

# 动画系统

## Animator

管理动画播放顺序与逻辑

### States

- 每一个States都代表了一个AnimationClip
- 查看State信息：Motion，Speed，Transition

### Transitions

- 定义了State之间的切换过度形式

- 属性：

  Exit Time  ：当前clip播放到**什么时间**开始过渡到下一个clip

  Transition Duration：过渡的过程要多久

  Interruption Source：该过渡会被什么样的其他过渡打断

### Variables

- 动画机里的变量是用于控制Transition的发生的
- 可以在左侧的面板管理变量
- Animator提供了几种变量类型：float、int、bool、trigger（trigger：是一个在一瞬间为true的布尔值。在那之后会即刻失效）
- 可以在Transition面板下方添加变量条件。当不满足条件时Animator会停在当前
- State；否则会执行Transition

### Scripting

- 我们可以在代码里修改Animator中变量的值来控制动画的播放
- 常用API：

​	   animator.SetBool(string name, bool value) 设定一个bool变量的值

​	   animator.SetTrigger(string name) 激活一个trigger变量

​	   animator.Play(string stateName) 直接跳转到对应的State，**无过渡**

- 重载:

  animator.Play(string stateName, int layer, float normalizedTime)

  比如“**从头开始**播放一个state”：“animator.Play(stateName, 0, 0)

# 相机

![Screenshot 2024-10-27 185455](C:\Users\wyz2t\Pictures\Screenshots\Screenshot 2024-10-27 185455.png)

## 颜色与渲染

一个场景里可以多个相机

- Clear Flags：定义了相机需要在空的地方（没有物体的地方）显示些什么

​				Skybox：显示天空盒

​				Solid Color：显示一个单色

​				Depth Only：什么都不显示（透明）

​				Don’t Clear：保留上一帧的颜色

- Background Color：Clear Flags模式下背景显示的颜色

- Culling Mask：这个相机渲染的layer
- Depth：这个相机本身的层级顺序；Depth更大的相机会被渲染在更上面

# UI系统

- 游戏中显示信息与可以交互的部分
- UI系统直观上是和场景分离的，但本质上是和其他物体一样存在于场景中的
- 最大区别是所有UI组件（图片、文字等等）都需要Canvas组件来渲染；默认下UI并不归相机渲染

## Canvas

- Canvas的作用是渲染UI物体；可以理解为“照向UI的相机”。
- Canvas有几种基本的渲染模式：

1. Overlay（显示在屏幕最上层）

​				这是默认的也是最常见的模式。所有属于Overlay Canvas的UI物体都会显示在场景上方

​				优点：使用简单

​				缺点：不灵活。一般不可以有透视效果；场景物体不可以遮盖住UI

2.Camera（使用相机渲染）

​				将Canvas及其所有子物体的UI组件使用某个相机渲染。

​				优点：比较灵活，可以有透视效果，场景物体可以遮住UI

​				缺点：较复杂，需要处理层级关系及多相机协调

3. World Space（世界坐标渲染）

​				将Canvas放在场景里，和所有其他物体一起被相机渲染

## Canvas Scaler和Graphic Raycaster

- Canvas Scaler一般会调成Scale with Screen Size来和屏幕分辨率适配
- Graphic Raycaster有这个组件后UI系统才会检测玩家输入（比如点击按钮等）

##  Slider

Slider组件（拖动条）可以用来在UI中实现类似进度条或滑条的功能

## Mask

与SpriteMask很像，Mask组件提供了UI中的遮罩功能

使用：

- 给一个UI图像组件物体（比如Image）添加Mask组件
- 所有它的子物体都只能在其范围内被显示（只有子物体受Mask影响）

有时候我们只需要一个矩形的Mask，而非一个任意形状的。这时就可以用RectMask2D组件代替Mask组件，因为Mask组件是较耗性能的

## RectTransform

RectTransform是Transform的一个子类。它提供了一个矩形的位置、旋转、缩放、中心、锚点等信息，并可以根据parent的RectTransform组件调节自身

## **Pivot & Anchor**

- Pivot和Anchor是RectTransform和Transform最大的区别
- **Pivot**即“中心点”，它决定了物体旋转的中心，是一个从(0,0)到(1,1)的值，分别对应左下角和右上角
- **Anchor**即“锚点”，它决定了该物体左下角和右上角的位置，由两个值：Min（左下角）和Max（右上角）决定，它们都是从(0,0)到(1,1)的值，分别对应**parent**的左下角和右上角。注意这里是相对于parent的！如果Min和Max都是（0,0），那么该物体就会固定在parent的左下角

## **Canvas Group**

Canvas Group组件可以将其子物体的所有UI物体视为一个整体，一起调节。

- 透明度
- 是否接受鼠标输入（raycast）
- 可互动性（interactable）

## 其它组件

- **Image**：显示图片
- **Text**：显示文字
- **Button**：提供UI按钮的功能

# Coroutine 协程

Unity中的协程提供了“多线程”的功能，即多份代码在同时以不同的“速度”执行

事实上Coroutine也是Unity**伪装的多线程**而已，内部其实还是单线程            

协程是一个函数

定义：**IEnumerator** <FuncName>(<params>){

​				<content>

​			}

调用：**StartCoroutine(IEnumerator routine)**;

## **yield**

- 协程系统的重要组成部分
- **yield**关键字的作用是将一个协程暂停，并在下一帧继续执行
- 常用结构：yield return null：下一帧继续执行协程

​						  yield return new WaitForSeconds(i)：在i秒后继续执行

例子：IEnumerator MyUpdate(){

​			while(true){

​			DoSomething();

​			yield return null;

​			}

}

每帧执行一次dosomething

## **WaitForSeconds**

要注意WaitForSeconds是一个类

使用new WaitForSeconds()来构造实例：WaitForSeconds w1 = new WaitForSeconds(1); 

# 事件系统

C#特有

# 物体渲染

## 材质Material

材质准确地定义了每个物体在特定情况下显示出来的**颜色**

实际上，材质准确的定义是“**着色器**的一个**实例**

**材质**准确地定义了一个物体每个像素的颜色，但实际上这件事是shader完成

的

## Shader 着色器

**很重要**

1. Shader接收到立方体的顶点位置、法线等数据

2. Shader将这些位置转换到屏幕坐标中

3. Shader对每个像素进行染色（这个颜色是我们设置的）

4. Shader获取到当前的光照，并计算每个像素受光之后的颜色

5. Shader将计算好的颜色输出至屏幕

## Texture 纹理

就是一张图片

其实texture在渲染流程中不过是shader中的一个“texture”类型的**变量**

3. Shader对每个像素进行染色（这个颜色是我们设置的）

4. Shader读取texture中每个像素的颜色，并将该颜色给到对应的物体像素上

5. Shader获取到当前的光照，并计算每个像素受光之后的颜色

6. Shader将计算好的颜色输出至屏幕

## 材质的实质

我们可以以该shader为“模板”，去衍生出一系列**它的实例**，然后修改各个实例的颜色就行了

**这时，我们根据一个shader所创建出来的这些实例就叫做材质**

## 有关渲染的Unity组件

### **MeshFilter**

用于处理模型本身的几何信息的（顶点位置，法线方向，切线方向等等

### **MeshRenderer**

用于**渲染该模型**的（通过一个material）

### **Unity**的默认材质：参数

- Rendering Mode（渲染模式）：这主要与**透明度**相关
- Albedo（反射率）：这其实就是物体的**颜色**（为什么叫“反射率”？）
- Metallic/Smoothness（金属度/平滑度）：这定义了它**与光照的互动强度**
- Normal Map（法线贴图）：这用来**改变物体的法线**，通常可以增加物体的细节程度
- Render Queue：渲染的先后顺序（小的先）

### **Lighting** **光照**

- Directional （平行光）：这种光照类似“太阳光”，它从某个**固定的方向以固定的强度**照射过来，且覆盖范围是无限的。世界中的所有物体都会平等地受到平行光的照射
- Point（点光）：这种光照更像我们日常生活中“灯泡”的光，它从一点均匀地在一定范围内发出光照，且强度会随着距离减弱
- Spot（聚光灯）：这种光照和舞台上的聚光灯相似，它从一点向一个方向发射出圆锥形的光照，且**强度会随着距离减弱**
- . Area（范围光）：这种光照类似有一定面积的LED灯，它**在一个面积上均匀地发射出光线**（就像教室天花板上的长条灯）（非实时）

# **3D** **物理**

Unity中的3D物理和2D物理是**两个独立的系统**，它们互不干涉地运作

大体的概念与2D物理相同，Collider用于定义**物理体积**，而Rigidbody用于定义**物理行为**

## **Collider**

特殊的collider：**MeshCollider**（网格碰撞器）but(**性能消耗很多**)

## **Rigidbody**

和2D大体是相同

- Mass：物体的质量。质量越大受到的重力就越大，对其他的物体施加的力也会更大。
- Drag：线性阻力。Drag越大，减速就越快。
- Use Gravity：是否受重力影响
- IsKinematic：若勾选，则物体仅受动画与代码控制，不参与直接的物理模拟
- Constraints：冻结位置与旋转

# 声音

## **AudioSource**

件用来存放与播放一段音频

参数：

- AudioClip：音频文件
- Mute：是否静音
- PlayOnAwake：是否在开始时直接播放
- Loop：是否循环播放
- Volume：音量
- Pitch：音高

## **AudioSource: API**

常用的API（部分）:

- audioSource.Play() 播放音频（若重复播放会被打断）
- audioSource.PlayOneShot() 播放音频（不会被打断）
- audioSource.Stop() 停止播放音频
- audioSource.Pause() 暂停播放音频
- audioSource.UnPause() 继续播放音频

## **Resources.Load**

用于在运行时加载资源

思路：

1. Unity会解析我们的项目文件夹，并提取出所有名字叫“Resources”的文件夹（这里必须叫“Resources”！）

2. 所有Resources文件夹中的文件都会被“合并至一个大Resources文件夹”

3. 现在我们就可以使用Resources.Load直接加载其中的文件

比如有一个文件是Assets/Resources/sound1.wav，那么我们就可以用Resources.Load(“sound1”);来加载该文件

所谓“加载”，就是获取到代表该文件的一个变量

AudioClip clip = Resources.Load<AudioClip>(“audio/explosion”)

# 粒子系统

平常所见的视觉特效有很大一部分都是通过**粒子系统**实现的，能够模拟一定数量的“粒子”的移动、颜色、外观等等形态

## **Particle System** **组件**

本质上:

1. 创造新的粒子

2. 消灭旧的粒子

3. 根据时间模拟粒子的形态

寿命：由**Start Lifetime**参数决定

初始颜色、速度和大小：这由Start Color、Start Speed、Start Size决定

频率：这由**Emission**中的参数决定

生成范围：由**Shape**中的参数决定

可受重力影响：Gravity Modifier

材质：Renderer

可以根据当前的生命具有不同的颜色、大小、旋转：Color/Size/Rotation Over Lifetime

可以根据当前的生命具有不同的速度：Velocity Over Lifetime

拖尾：Trail

# Render Texture与2D倒影

Render Texture允许我们将相机所拍到的内容映射到它身上

Render Texture是一个装有当前某个相机的图像的Texture

水面倒影：

1. 新建一个Render Texture

2. 新建一个相机拍摄水面上方的场景

3. 新建一个材质，并把Render Texture作为albedo填入

4. 使用该材质渲染水面

# 场景（Scene）

一个场景（scene）本质上就是**一些GameObject的集合**

所谓加载场景，可以理解成把当前场景的物体全部删除，并换成新场景的物体

加载场景主要分为两类：**同步加载**与**异步加载**

同步加载（sync）：当我们下达加载场景的指令时，**游戏会卡住**，等加载完毕后直接切换到新场景

异步加载（async）：当下达指令时，**游戏会继续**而场景会在后台加载。加载完毕后再切换到新的场景

## **Scene和SceneManager**

们在UnityEngine.SceneManagement这个命名空间中

**Scene**代表了一个场景文件，其中包括以下信息：

- buildIndex：该场景在打包（导出）时的顺序。数字越小优先级越高
- name：场景名
- isDirty：场景是否被修改
- GetRootGameObjects()：获得场景中所有的根物体

而**SceneManager**提供了许多用来管理场景的工具

- GetActiveScene()：获得当前场景
- GetSceneByBuildIndex()：通过buildIndex获取场景
- GetSceneByName()：通过文件名获取场景
- LoadScene()：同步加载场景
- LoadSceneAsync()：异步加载场景

## **同步加载**

可以使用SceneManager.LoadScene()方法进行场景的同步加载

- SceneManager.LoadScene(“MainMenu”); 即可加载“MainMenu”这个场景
- SceneManager.LoadScene(7); 即可加载buildIndex为7的场景

## **Async** **异步加载**

可以使用SceneManager.LoadSceneAsync()方法进行场景的异步加载

SceneManager.LoadSceneAsync()方法会返回一个类型**AsyncOperation**的变量，其中记录着这次加载相关的信息

AsyncOperation中有以下常用的成员

- **bool** isDone：该加载是否已经完成
- **float** progress：进度（0-1），这里注意如果allowSceneActivation为false的话，progress会卡在0.9。
- **Action<AsyncOperation>** completed：加载完成时触发的事件
- **bool** allowSceneActivation：是否允许场景加载完成时自动切换至新场景

## **保持在场景之间的物体**

可以使用DontDestroyOnLoad()函数达成这个目的

DontDestroyOnLoad()的作用是标记一个物体为“不被场景加载所销毁

void Awake()

{

​		DontDestroyOnLoad(gameObject);

}

# 存档

## **PlayerPrefs**

很简易的保存信息的方法：PlayerPrefs

可以把PlayerPrefs想象成一个存储在游戏外的**Dictionary**，我们可以通过key访问其中的value

API：

PlayerPrefs.SetString(string key, string value);

PlayerPrefs.GetString(string key);

以及对应的int、float版本

**限制**：

1. PlayerPrefs是不安全的，通过它存储的信息是完全未经加密的，这使得玩家可以轻易地在电脑中修改其中的数值
2. PlayerPrefs支持的类型很有限，仅支持存储int、float、string类型的数值
3. PlayerPrefs的性能并不好，相比于一般的存档方式，PlayerPrefs在处理较大数据时的速度会慢许多
4. PlayerPrefs对多平台并没有足够的支持，在每一种操作系统中PlayerPrefs存储数据的位置都是不一样的，这有可能导致多平台之间的数据丢失

## **Unity** **的数据路径**

Unity提供了内置的数据路径：Application.dataPath

我们可以使用Application.dataPath+”/文件名.txt” 来存储一个文本文件

例如：File.WriteAllText(Application.dataPath+”/Save.txt”, “Hello World!”);

## **Json**与序列化

Json（Javascript Object Notation）是一种数据编码模式。

它可以将一个数据结构（比如一个类）用一篇文本文件来表示。这个操作叫做**序列化**

相反地，我们也可以根据一个Json文件构造出原来的数据结构。这叫做**反序列化**

### **使用**Json进行存档

**存档：**

**第一步：**我们需要将这些信息整合进一个类里，这样就可以一起存储/读取了。（比如叫Save类）

**第二步：**我们需要将这个类序列化为Json格式。Unity提供了**JsonUtility.ToJson**()方法为我们实现这个需求。

**第三步：**我们需要指定一个文件路径并使用**File.WriteAllText**()方法将得到的Json文本写入该文件里。

**读档：**

**第四步：**读档时，我们首先使用File.ReadAllText()读取到存档文件里的文本。

**第五步：**读取到的文本是Json格式的，于是我们使用**JsonUtility.FromJson**()方法将其反序列化为Save类。

**第六步：**得到存档类后，我们逐个获取其中的数据并应用在场景中。

### 限制

1. 部分数据类型不受支持，JsonUtility无法序列化Dictionary、多维数组、泛型类等数据结构。不过大部分常用的数据结构都是受支持的。

2. 不支持null的序列化，当一个数据的值为null时，JsonUtility会直接跳过它

3. 序列化行为不可自定义

# 设计模式

所谓设计模式，其实就是我们写代码时可以选择遵循的一些**模板或思想**。它们通常是前人在遇到**经常出现的问题**时总结出来的解决方案

## **单例模式** **Singleton Pattern**

**最简单、最常用、**但也**最容易被滥用**的设计模式

单例模式可以被用在一个类上，它确保了两点：

1. 始终只允许有一个该类的实例
2. 对于该实例有一个全局的访问点

**public class Singleton{**

 		**private static Singleton instance;**

 		**private Singleton() { }**

 		**public static Singleton Instance{**

 				**get{**

 						**if (instance == null)**

 								**instance = new Singleton();**

 						**return instance;**

 				**}**

 		**}**

**}**

我们游戏中许多的“Manager”类（只有一个manager，且该manager提供许多常用功能）都需要具备这两点特征

### **更好用的单例模式**

我们可以创建一个**通用的单例类**，并让每个真正用到的单例类**继承**自它就好了

**一些前置小知识：**

**protected**访问修饰符：使用protected修饰的物体只可以被**该类以及其子类**所访问

**??**运算符： a??b是一种简写方式，其等价于a!=null?a:b 也就是说当a不为空时返回a，否则返回b

使用**lambda**表达式的**属性getter**：我们可以简写一个属性的getter：

**private int myInt;**

**public int MyInt** **=>** **myInt<0 ? 0 : myInt;**

可以直接使用lambda表达式来创建getter



**public class Singleton< T> where T : class, new(){**

​		 **protected Singleton() {}**

 		**private static T _inst = null;**

​		 **public static T instance => _inst ?? (_inst = new T());**

​		 **public static void Clear()**

 		**{**

 				**_inst = null;**

​		 **}**

**}**

**优点：**

1.  单一性：单例模式确保了我们始终只会有一个实例。这样既方便管理也不用担心会有多个实例造成混乱。
2. 便捷性：全局的访问点使得我们可以很方便地从任何地方访问单例。
3.  懒：单例只会在需要的时候实例化一次。这样不会造成多余的资源浪费

**缺点:**

1.  全局性：有时一个全局的访问点可能并不是好事，因为这样会增加类之间的耦合度，降低可读性，提高出错率。其实很多时候我们可以使用static字段与方法来代替单例。
2. 依赖性：当我们过度使用单例时，每当一个单例类出错，很可能会导致整个系统崩溃

## **指令模式** **Command Pattern**

