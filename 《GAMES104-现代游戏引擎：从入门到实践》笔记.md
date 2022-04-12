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
