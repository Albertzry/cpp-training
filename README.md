# **ADAS Executor**

- 由Config组件进行初始化及配置

- 接收Controller组件的各种移动控制命令（如前进、转向等），维护车的位置状态

## 控制指令

Executor组件提供初始化接口，负责将车初始化在指定位置，输入数据为: (x, y, heading)，其中：x,y对应地点位置（均为int32类型）heading对应四个方向（N、S、E、W）(均为char类型)

Executor组件可以执行如下的移动指令：

M: 前进，1次移动1格

Executor组件可以执行如下的转向指令：

L: 左转90度，位置不变

R: 右转90度，位置不变

### F:  加速指令，接收到该指令，车进入加速状态，该状态下：

M：前进2格（不能跳跃，只能一格一格前进）

L：先前进1格，然后左转90度

R：先前进1格，然后右转90度

### **B:  倒车指令，接收到该指令，车进入倒车状态，该状态下：**

M：在当前朝向上后退一格，朝向不变。注：比如朝向为N时收到M指令，y坐标减1，朝向保持N。

L：右转90度，位置不变

R：左转90度，位置不变

**B和F两个状态可以叠加，叠加状态下**：

M：倒退2格（不能跳跃，只能一格一格后退）

L：先倒退一格，然后右转90度

R：先倒退一格，然后左转90度

再接收一次B指令，对应的状态取消

### TR：掉头指令，收到该指令后，左转90度->向前进1格->左转90度

TR指令不受倒车状态B影响

TR指令会受到加速状态F影响，在加速状态下，收到TR指令后，向前进1格->左转90度->向前进1格->左转90度

TR一起出现时才有效，不考虑异常输入场景

### C：改变车型（Bus 和 Sport Car），默认为Sport Car

**1. Sport Car** 

速度很快，指令控制行为遵循下述描述：

M：前进，1次移动2格(先前进1格再前进1格)

L：先左转90度，再前进1格

R：先右转90度，再前进1格

**2. Bus**

Bus车体较长，长度占2格，它的坐标是车头的坐标。为了行车安全，指令控制行为遵循下述描述：

M：前进，1次移动1格

L：先前进1格，再左转90度

R：先前进1格，再右转90度

## 测试

\# windows 

script\build_and_run.bat


 \# linux/macos

bash script/build_and_run.sh