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
