# 《GAMES104-现代游戏引擎：从入门到实践》

## 1.游戏引擎导论
### 游戏引擎的意义
**虚拟世界**的很多背后技术都是游戏引擎技术，比如：虚拟人，影视行业，军事模拟，数字孪生等

![](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/20220412204700.png)

### 游戏引擎的历史

- 红白机时代对游戏性能极为苛刻，是游戏野蛮生长的阶段，涌现出大量有创意有想法的游戏；

- John Carmark：卡神，3D射击游戏之父，第一个定义并开发Game Engine的人；

- Quake：第一款系统研究联网的游戏，开拓了现代游戏引擎，使用了3D技术；

- 硬件的发展推动了游戏引擎的进步；

#### Family of Game Engines

<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/20220412211723.png"  />

#### Middleware of Game Engines
<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/20220412212002.png" style="zoom: 50%;" />

### 什么是游戏引擎
- Technology Foundation of Matrix;

- Productivity Tool of Creation;

- Art of Complexity;

- 图形学只是游戏引擎的一部分:
  ![image-20220412212449227](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220412212449227.png)
  
  ![image-20220412212516345](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220412212516345.png)
  
- 游戏引擎的挑战：算力（如CPU，硬盘，内存，带宽，延迟等）
- 引擎是给程序员和**艺术家**们使用的**可协作**的生产力工具
- 引擎也需要不断迭代完善

课程会涉及到 <u>基础架构，数据管理，Rendering，Animation，Physics，GamePlay，Effects，Navigation，Camera，ToolSet（C++ Reflection, Data Schema, Online, Motion Matching, Procedural Content Generation, Data-Oridnted Programming，JobSystem，Lumen，Nanite 等）</u>

## **2**.引擎架构分层
### 概览
从顶层到底层，分别为如下（5+1）

![image-20220413200104937](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413200104937.png)

### Tool Layer
编辑器的**工具层**，是我们在接触引擎的时候最直观的最直接交互的层级
![image-20220413200256566](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413200256566.png)

### Function Layer
**功能层**，是游戏可见，可移动，具有可玩性等
- Rendering：三维数据转成二维图片

- Animation：使角色动起来

- Physics：具有真实世界的物理属性

- Script，FSM，AI：使其具有游戏性

- Camera，HUD，Input：人机界面的交互
  ![image-20220413200750711](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413200750711.png)

### Recource Layer
**资源层**，游戏运行依赖大量的文件，如模型文件、贴图文件、声音文件、配置文件等等，资源层负责对其加载和管理

资源层给功能层提供“弹药”

![image-20220413200942707](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413200942707.png)

### Core Layer
**核心层** ，是为上层各系统，提供帮助的工具箱，会频繁使用如<u>容器创建，内存分配，多线程，数学库</u>等底层功能

![image-20220413201409425](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413201409425.png)

### Platform Layer
**平台层**，应对各种平台的不同，包括硬件和软件。引擎或游戏最终需要提供给客户，但是客户使用的平台和输入设备都有差异



![image-20220413201722095](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413201722095.png)

### 3rd Party Libraries
**第三方工具** ，有些是通过库的方式，有些是提供了一个额外的工具

![image-20220413201905191](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413201905191.png)

### 为什么要分引擎层
- 封装解耦，每一层只关注自己的事情，减低复杂度

- 底层提供基础服务

- 顶层不需要知道底层的具体实现

- 顶层部分迭代频繁，底层相对稳定

- 一般只允许顶层调用底层

  ![image-20220413202125342](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/image-20220413202125342.png)

### 资源层-How to Access My Data（怎么访问我的数据）
#### 引擎化（importing）
各种资源（Resource）的格式都是**不同**的，而且类似Maya的数据，里面有很多引擎不需要的数据，只是为了方便在Maya里面操作的信息，因为我们需要一次转换，将这些数据转换成引擎能够使用的高效数据，成为资产（Asset）

如贴图，可以是Png或者Jpg等，他们有自己的压缩算法，如果直接在Gpu中使用会很费性能，可以将其统一转化为dds格式进行使用。

#### 资产关联
如对一个可以动起来的模型，其上面的网格，贴图，材质，动画等资源都是关联在一起的，我们可以定义另外一种资产进行管理指定（Composite Assert）

![image-20220419223823151](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192305396.png)

这些引用到的资产可以通过GUID（唯一编号）等进行标识，可以避免资产位置改变而导致路径找不到的问题（路径无关性）

#### Runtime Manager
- 管理资产引用

- 实时的通过引用去加载/卸载各种资产

- 管理资产生命周期

- 资源池分配

  ![image-20220419224136091](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192305886.png)

#### 功能层
##### Tick
使世界动起来，一个**周期**（Tick）里跑完一边逻辑和绘制渲染

![image-20220419224327203](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192305191.png)

整个Tick整体可以分开为两个部分：

1. 逻辑

2. 渲染

   ![image-20220419224516009](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192305484.png)

   

#### 复杂
功能层所涉及的东西很多，且其中的有些模块会在游戏引擎中又会在游戏中使用

![image-20220419224725016](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192305874.png)

#### 多线程
一核有难,多核围观?
初始,分为三个线程:Logic，Render，Simulation

![image-20220419224940829](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192306757.png)

主流引擎中，会把特别容易并行的东西，如物理等，fork出来，分散到别的线程去做

![image-20220419225055110](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192306717.png)

未来引擎中，把任务变成原子的，把每个核都能吃的满满的

![image-20220419225139532](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202204192306990.png)



#### 核心层

![image-20220505210354960](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052103007.png)

#### 数学库
除了一些特定的领域需要高深的数学，如物理。一般来说了解线性代数只是就能应付大多数问题。
![image-20220505210431680](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052104742.png)
但是数学库需要很注重**效率**，可能一个很简单的运算为了减少时长要用到更多代码和理论。

SIMD（Single Instruction Multiple Data），单指令多数据流，能够复制多个操作数，并把它们打包在大型寄存器的一组指令集，运算性能上有优势。

#### 数据结构和内存管理
一些编程语言的数据结构可能不满足我们的要求，比如C++的Vector当空间不够时，会开辟2倍的空间，删除时可能会产生碎片等，需要我们自己实现一套数据结构们这里也涉及到需要我们自己管理内存，减少碎片，实现内存分配等。

![image-20220505210854268](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052108326.png)

高效的内存管理

![image-20220505210919357](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052109426.png)

#### 平台层
跨平台，掩盖平台的差异性



![image-20220505211331996](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052113033.png)

如对于图形API来说，有OpenGL，DirectX，Vulkan等，需要一个RHI（Render Hardware Interface）封装各个平台的SDK的区别；



![image-20220505211507853](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052115910.png)

#### 工具层
##### 自己开发
允许别人在引擎里去创造游戏的内容，以**Level Editor** 为中心，形成的一些列的编辑器

![image-20220505211653173](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052116406.png)
为什么在别的工具里已经有了编辑器的情况下还要有一个游戏的编辑器；为了能实时在我们的游戏引擎中调整效果，保证我们在引擎编辑中看到的效果和实际游戏运行起来的效果一致，提升开发效率；

工具层的代码量占的比例很多。
#### 第三方开发
第三方，如Maya，Houdini，Max，Blender等
自己开发和第三方开发的工具，所产生的数据都会通过**Asset Conditioning Pipeline** 进行传输。

![image-20220505212009749](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205052120012.png)

## 3.如何构建游戏世界
### 游戏世界
### GameObject
游戏世界通常由动态物体，静态物体，环境，其他物体(Trigger，导航网格，规则等)等元素组成。

<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222210662.png" alt="image-20220522221001466" style="zoom: 25%;" />

<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222210320.png" alt="image-20220522221031134" style="zoom:25%;" />

<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222210320.png" style="zoom:25%;" />

<img src="https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222211151.png" alt="image-20220522221150981" style="zoom:25%;" />



以上这些元素基本上都可以抽象为一个**GameObject** ;

对于这些GameObject的描述基本可以分为两部分：属性（**Property** ）和行为(**Behavior** )，可以通过标准的面向对象的思想和继承之类的来实现；

以无人机为例，其有位置，生命等属性，有移动，侦查等行为。攻击型无人机可以继承无人机类，增加Fire接口；

### GmaeObject 

![image-20220522221641286](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222216386.png)

![image-20220522221650699](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222216785.png)



### Component

但是对于另外一些GameObject，通过继承方式不理想，比如一个水陆两栖坦克，既有船的能力，也有坦克的能力，那么它应该继承自谁呢？此时我们可以利用组合的方式，通过**Component** （组件），可以自定义的组合他们的能力；

![image-20220522221841464](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222218582.png)

仍以无人机为例，此时各种能力和属性都可以抽成一个个的组件，如TransformComp(位置)，ModelComp（外形），MotorComp（运动），AIComp，PhysicsComp等，此时无人机里只需要有一个ComponentBase的数组即可：

![image-20220522222046248](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222220352.png)

用组件的方式另外一个好处就是灵活，可替换，需要哪个能力就用哪个组件，不需要就卸掉

![image-20220522222706368](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222227458.png)

很多商业引擎也用的上述Component思想

![image-20220522222745682](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222227774.png)

注意Unreal和Unity中的Object与上述的GmeObject并不相同。Unreal中Actor是类似GameObject的概念，Uobject更类似于高级语言中的Object。

### Tick

每隔一段时间让世界向前走一步：让每个Gameobject中的每一个Component去Tick一次

![image-20220522223001303](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222230395.png)

在实际中，现代游戏引擎更多的通常不是逐对象逐组件去Tick，而是逐系统，比如先去做所有的和碰撞相关的事情，再去做所有和动画相关的事情，这样用类似流水线的方式，更有效率。

![image-20220522223153949](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222231029.png)

### 一个Tick的时间过长怎么处理？

- Tick的时候将步长传入，依赖步长进行数据的补偿；

- 直接跳过下一个Tick；

- 通常还是需要进行策略优化

### Event

  对于GmeObject之间的交互，如一个炸弹爆炸，对其他不同的GO的影响，早期硬编码如下：

![image-20220522223545099](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222235226.png)

这样的代码明显违反设计模式的原则，由此引出Event（事件），有点类似观察者模式，通过事件派发的方式进行解耦，接收/绑定了这个事件的对象再去处理该事件。

![image-20220522223726566](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222237689.png)

在实际的时间传递中，时序很重要吗，如游戏的回放功能，实际是记录用户的输入，如果各用户是依次执行，则没有问题，但如果用户同时进行同一事件的操作则会有问题，因此需要引入一个“**邮局** "，各个操作先发到邮局，邮局去保证时序接着进行发送，常用引擎中的类似**PreTick,PostTick** 的函数都是为了处理时序问题



主流游戏引擎示例如下：

![image-20220522224119972](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222241097.png)

### Scene Manage

- 通过唯一ID进行标识管理；
- 通过Object的位置进行管理；

扔以爆炸为例，可以通过遍历场景中半径范围内所有的对象，进行消息的通知，也可以通过画格子的方式进行通知管理，先找临近的格子，没有就不再找别的格子了。

![image-20220522224326607](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222243702.png)

但由于实际场景中物体分布不均匀，比如有时战壕里的人明显会多余空地，此时可以通过分层的方式进行划分。

![image-20220522224418871](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222244977.png)

常用分层如下：

![](https://raw.githubusercontent.com/ShayouGs/FigureBed/main/202205222244593.png)


## 4. 游戏引擎中的渲染实践







