记个简单的增强型GaN笔记

内容主要来源于《氮化镓功率晶体管——器件、电路与应用》这本书

[TOC]



## 器件概述

### 二维电子气与GaN器件结构

#### 2DEG

![image-20210214132350849](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214132350849.png)

在AlGaN和GaN的交界处存在着一层二维电子气（2DEG），当施加电压时,这种2DEG可以有效地传递电子，这便是GaN器件导电性的来源

![image-20210214133000847](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214133000847.png)

通过在栅源极之间施加负压，可以将2DEG抵消，使得导电通路中断

利用这个原理可以很方便地制成耗尽型器件

耗尽型器件常态为通路，必须施加负压才能维持断态，实际应用起来不方便，于是出现了如下的增强型结构

这种结构使得GaN器件常态下断路，只有在栅源间施加正电压才能导通

![image-20210214133153859](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214133153859.png)

常用的增强型GaN器件结构有两种，分别是pGaN Gate Enhancement‐Mode Structure和Hybrid Normally Off Structures

#### pGaN Gate Enhancement‐Mode Structure

![image-20210214134359462](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214134359462.png)

p掺杂的GaN层产生的正电压大于2DEG的挤压电压，使得2DEG被耗尽

#### Hybrid Normally Off Structures

![image-20210214134620785](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214134620785.png)

另外一种做法是将耗尽型GaN管和增强型MOS管串联

由于增加了一个MOS管，这种结构势必会增加系统的损耗，因此这种结构适合用于GaN导通电阻远大于MOS管的情况

由于高导通电阻意味着高耐压，这种方案仅适用于高压系统（高于200V）

#### 反向导通

![image-20210214135655304](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214135655304.png)

2DEG流过电流不分正负，使得增强型GaN同样可以反向导通，其原理与正向导通相似

只要栅源极施加的负压大于$V_{th}$，2DEG就会联通，反向电流流过2DEG

上面介绍的MOS混合结构同样可以流过反向电流，不过反向电流要同时流过MOS的体二极管和耗尽型GaN

### 器件参数

#### 击穿电压$BV_{DSS}$ 

用于描述器件承受关断电压的能力

初了这个参数外，芯片厂商的手册里还可能会有高于击穿电压的瞬态电压，这意味着器件可以在短时间内承受更高的关断电压，这一参数和MOSFET的雪崩能力有些相似

#### 泄漏电流$I_{DSS}$

器件处于关断状态时，仍存在小的泄漏电流$I_{DSS}$

该参数与温度和$V_{DS}$都正相关

#### 导通电阻$R_{DS(on)}$

晶体管的导通电阻是指其所有组成部分电阻之和

$R_{DS(on)}$的每个组成部分都会随温度变化，因此$R_{DS(on)}$的温度系数取决于GaN器件的设计

$R_{DS(on)}$还与$I_D$和$V_{GS}$有关

#### 阈值电压$V_{th}$

GaN器件的$V_{th}$基本不随温度变化，然而MOS混合结构的阈值电压取决与MOS的阈值电压，而MOS的$V_{th}$具有负的温度系数

#### 电容与电荷

与FET相关的电容主要有$C_{GS}$、$C_{DS}$和$C_{GD}$，通过对这些电容充电可以控制FET的开通与关断

手册中可能会给我们输入端电容$C_{ISS}$或输出端电容$C_{OSS}$，上述电容的换算如下所示

* $C_{OSS}=C_{GD}+C_{DS}$
* $C_{ISS}=C_{GD}+C_{GS}$
* $C_{RSS}=C_{GD}$

这些电容不是固定值，而是对应的极电压的函数，因此使用时通常将其积分，得到对应的所需电荷$Q_G$、$Q_{GS}$、$Q_{GD}$、$Q_{OSS}$等，这些值不仅和器件有关，还和需要达到的电压有关

#### 反向传导

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214145155474.png" alt="image-20210214145155474" style="zoom: 50%;" />

虽然不像MOS那样有体二极管，但2DEG为GaN器件提供了反向导通的通道

正反电流都流过2DEG，因此正向导通和反向导通的$R_{DS}$具有相同的值和温度系数

此外，如果将栅极电压降至0以下，降低的值会反映在反向压降上

比如，假如$V_{GS}=0$时$V_{SD}=1.8V$，那么$V_{GS}=1V$时$V_{SD}=2.8V$

#### 热阻

//TODO

## GaN的驱动

### 驱动过程

![image-20210214153039704](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214153039704.png)

与MOSFET相似，GaN的驱动就是给其寄生电容充电使之达到要求电压

如图，GaN器件的开通过程可以分为四个部分，每部分都有对应的所需电荷

* $V_{GS}$从零达到$V_{th}$，对应$Q_{GS1}$
* $V_{GS}$达到平台电压，在这期间$I_{DS}$开始增大，对应电荷$Q_{GS2}$
* $V_{GS}$保持平台电压，在这期间$V_{DS}$逐渐减小至零，器件完全导通，对应电荷$Q_{GD}$
* $V_{GS}$达到稳态栅极电压

$Q_G$为驱动过程中总的充电电荷，$Q_{GS}$为将栅极电压加至平台电压所需电荷

为了描述器件优劣，定义品质因数FOM，其值为$Q_G$和$R_{DS(on)}$的乘积

### 栅极驱动电压

与MOS最高可达$\pm18V$的栅极电压不同，许多GaN器件栅极最大电压只有+6V/-5V

与此同时，GaN器件要达到完全开通需要+4V以上的电压，一般使用时将稳态栅极电压设定为+5V

为了满足这一要求，通常将栅极驱动器以接近临界阻尼的方式接通回路

![image-20210214155036817](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214155036817.png)

器件开通和关断时回路如图所示

开通时，栅极驱动器、晶体管、栅极驱动旁路电容（$C_{VDD}$）以及其线路电感组成LCR谐振电路

为了严格抑制该电路，防止过冲，线路总的等效电阻必须满足下式

$$
R_{G(eq)}\geq\sqrt{\frac{4L_{G}}{C_{GS}}}
$$
![image-20210214163957904](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214163957904.png)

对于下降沿，-5V实际上不存在实际限制，为了更快关断允许一些负振荡

此外，由于少了$C_{VDD}$，线路此环路的感抗更小

不过要注意负震荡随后的正向震荡不要超过$V_{th}$

由于开通和关断的要求不同，可以使用两个不同的栅极电阻来独立地调整回路阻尼

### 自举和浮动电源

驱动半桥时，我们常用下面的结构驱动

![image-20210214165026466](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214165026466.png)

当底边导通，高边关断时，电源总线通过二极管给$C_{Boot}$充电；底边关断后，$D_{Boot}$关断，$C_{Boot}$提供电压驱动高边器件

理想情况下，$C_{Boot}$会被充电至总线电压减去二极管导通电压（大约0.7V）的大小

但实际应用中，在下管关断而上管尚未开通时，会有电流反向流过下管进行续流，在下管产生的压降会使得$C_{Boot}$被过充电（$V_{DD}-V_{D_{Boot}}+V_{SD}$），如果算上线路寄生电感，过充会更大

这一过充可能会影响上管驱动，损坏上管

有三种方法可以减少过充：

* 在开关互补的应用中，减少死区时间可以减少电容被过充的时间
* 在下管反并联一个肖特基二极管以减小过充电压
* 在开关不互补的应用中，使用专用驱动芯片

### $dv/dt$抗性

![image-20210214173137149](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214173137149.png)

低端管漏极可能会有较大的$dv/dt$产生，这一$dv/dt$会使$C_{DS}$充电，连带着$C_{GD}和C_{GS}$都会充电，使得$V_{GS}>V_{th}$，电路误导通

假设$C_{GD}$电流全部流过$C_{GS}$，则只有密勒比$Q_{GD}/Q_{GS1}>1$时，才能保证下管不会误导通，然而实际的GaN管的密勒比很少有小于1的

为了防止误导通，通常在驱动处增加下拉电阻$R_{Sink}$增加电流回路，分流流经$Q_{GD}$的电流，防止误导通

此外，在上下开关互补的应用中，可以调整死区时间，使上管恰好在下管关断震荡为负时开通，不过这一方法使得时许固定，不易实现

![image-20210214180536812](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214180536812.png)

### $di/dt$抗性

![image-20210214180740702](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214180740702.png)

如图所示，栅极驱动回路和功率回路存在共源电感（CSI）

在开关管关断时，高$di/dt$使得共源电感感应出电压，这一电压使得$C_{GS}$被驱动到负值，并经阻尼不足的LRC环路谐振得到正的震荡电压使开关管导通

通过增加$R_{Sink}$的阻值来增加环路的谐振可以解决这一问题，但增大$R_{Sink}$会对$dv/dt$抗性产生负面影响

另一个方法是改进封装和电路以减小CSI，比如将源极分为两路，一路作为驱动回路，一路作为功率回路

### 逻辑信号失真

![image-20210214182805919](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214182805919.png)

该图展示的是将CSI最小化后的电路，$di/dt$在CSI产生了感应电压

当逻辑电路和功率回路共地，驱动电路的地是源极，由于源极和地之间的电压波动，逻辑电路的输出相对驱动电路会产生波动，若波动足够大甚至会影响逻辑输出值

最佳解决方法是将逻辑电路的地接到源极上，但是对于含有多个低端开关管的电路来说并不实用

![image-20210214192820955](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214192820955.png)

增加一个RC滤波电路可以有效地滤除这些杂波，但会带来延迟问题和宽度失真

第二种方法是增加电平移位器或隔离器，不过会增加电路元件数量

![image-20210214193110769](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214193110769.png)

在半桥电路中，高端驱动会在高电平和低电平之间转换，电容上电压改变会产生共模电流

共模电流同样会影响逻辑输出，严重时改变逻辑状态

![image-20210214193808372](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210214193808372.png)

减小共模电流，可以通过选用电容较小的器件或和减小回路面积来实现

## PCB布线与测量

//TODO

## 硬开关损耗分析

硬开关变换器的损耗包括以下几个部分：

* 电流电压重合造成的开关损耗$P_{SW}$
* 输出电容充放电所造成的输出电容损耗$P_{OSS}$
* 栅源电容充放电所造成的栅极电荷损耗$P_{G}$
* 体二极管续流所造成的反向传导损耗$P_{SD}$
* 关闭体二极管所造成的反向恢复损耗$P_{RR}$，GaN虽然自身没有反向恢复损耗，但有些应用中可能需要并联一个肖特基二极管

### 开关损耗

通过下图的曲线，可以近似得到开关损耗

![](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210215230308935.png)

将开关损耗分成电压转换造成的功率损耗$P_{Vt}$和电流转换所造成的功率损耗$P_{Ct}$并求和
$$
P_{SW}=P_{Vt}+P_{Ct}=(1/2)V_{BUS}I_{DS}(t_{xR}+t_{xF})f_{SW}
$$
式中V、I、f都是已知的，只要得到开关时间便能估算出功率

#### 电压转换周期

电压转换时间为密勒电容充放电的时间，即$C_{GD}$充放电的时间

电容充放电时间通常通过下式计算
$$
t=\frac{Q}{I}
$$
对于$Q_{GD}$，可以通过对$C_{GD}(C_{RSS})$积分的获得
$$
Q_{GD}=\int_0^{V_{BUS}}C_{RSS}(v_{DS})dv_{DS}
$$


$C_{OSS}$的值通常由芯片手册曲线给出

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210216103136911.png" alt="image-20210216103136911" style="zoom:50%;" />

栅极电流受到驱动电路阻抗、电感等因素的影响，可以分别由下式得出：
$$
I_{Gvon}=\frac{V_{DR}-V_{PL}}{R_{Gon}}
$$

$$
I_{Gvoff}=\frac{V_{PL}}{R_{Goff}}
$$

式中$V_{PL}$为台阶电压，$V_{DR}$为驱动电压

#### 电流转换周期

电流转换时间为栅极电压从阈值电压到平台电压之间的一段时间，即给$Q_{GS2}$充电的时间，这里假如我们要得到100A漏源电流下的充电时间

![image-20210216141003926](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210216141003926.png)

器件通常会给出如下的传输图，根据漏源电流可以得到平台电压，如33A对应2.3V平台电压，100A对应2.8V平台电压

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210216141200698.png" alt="image-20210216141200698" style="zoom: 50%;" />

$Q_{GS1}$可以由下式得到
$$
Q_{GS1}=\frac{Q_{GS}}{V_{pl}}\cdot V_{th}
$$
$Q_{GS(op)}$则由下式得到
$$
Q_{GS(op)}=\frac{Q_{GS}}{V_{pl}}\cdot V_{pl(op)}
$$


导通和关断的栅极电流可由下式估算
$$
I_{Gcon}=\frac{V_{DR}-(\frac{V_{pl}+V_{th}}{2})}{R_{Gon}}
$$

$$
I_{Gcoff}=\frac{V_{pl}+V_{th}}{2R_{Goff}}
$$



将上述各式联立可以得到器件的导通和关断损耗
$$
P_{on}=P_{Vt}+P_{Ct}=\frac{V_{BUS}I_{DS}f_{SW}R_{Gon}}{2}\cdot [\frac{Q_{GD}}{V_{DR}-V_{pl}}+\frac{Q_{GS2}}{V_{DR}-(\frac{V_{pl}+V_{th}}{2})}]
$$

$$
P_{off}=P_{Vt}+P_{Ct}=\frac{V_{BUS}I_{DS}f_{SW}R_{Goff}}{2}\cdot [\frac{Q_{GD}}{V_{pl}}+\frac{Q_{GS2}}{(\frac{V_{pl}+V_{th}}{2})}]
$$

总开关损耗为$P_{on}$和$P_{off}$之和
$$
P_{SW}=P_{on}+P_{off}
$$

### 输出电容($C_{OSS}$)损耗

$$
P_{OSS}=f_{sw}E_{OSS}=f_{sw}\int_0^{V_{BUS}}v_{DS}C_{OSS}(v_{DS})dv_{DS}
$$

输出电容损耗是否存在取决于电路的拓扑和工作状态

### 栅极电荷($Q_G$)损耗

$$
P_G=Q_GV_{DR}f_{sw}
$$

### 正向导通损耗

正向导通损耗发生在死区时间，续流电流会流过关断的晶体管的体二极管造成损耗

![image.png](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/1610073035896-78f33a25-181a-42bb-904a-1e3ccb62232c.png)

其功耗由下式给出
$$
P_{SD}=V_{SD}I_{DS}t_{SD}f_{sw}
$$
正向导通时间须根据工作状态来确定，改时间虽然与逻辑死区时间有关，但并不相同

下图中$t_{eff}$是实际的死区时间，$t_{SD}$是电流正向流过体二极管的时间，也就是产生正向导通损耗的时间

两者的时间差是换向时间，用$t_{ZVS}$表示

![image-20210216150134777](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210216150134777.png)

在$t_{ZVS}$这段时间内，外部电流持续给器件$Q_1$和$Q_2$的输出电容充电，因此可以由下式得到$t_{ZVS}$
$$
t_{ZVS}=\frac{Q_{OSS\_Q1}+Q_{OSS\_Q2}}{T_{Turn-off}}
$$
其中$Q_{OSS}$通过对电容曲线积分获得
$$
Q_{OSS}=\int_0^{V_{BUS}}C_{OSS}(v_{DS})dv_{DS}
$$
反向传导时间通过死区时间减去换相时间获得
$$
t_{SD}=t_{eff}-t_{ZVS}
$$
<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210216202156592.png" alt="image-20210216202156592" style="zoom: 50%;" />

如果$Q_2$在换向完成之前开通，那么用上式得到的$t_{DS}$为负，此时电路不存在正向导通损耗

在这种情况下，$Q_2$导通时的开关节点电压由下式得到
$$
V_{PZVS}=\frac{I_{Turn-off}t_{eff}V_{BUS}}{Q_{OSS\_Q1}+Q_{OSS\_Q2}}
$$

### 反向恢复($Q_{RR}$)损耗

增强型GaN晶体管没有反向恢复损耗

共源共栅型晶体管由于串联了MOSFET而具有少量的反向恢复电荷

反向恢复功耗由下式得到
$$
P_{RR}=E_{RR}f_{SW}=Q_{RR}V_{BUS}f_{sw}
$$
$Q_{RR}$并不容易计算，器件手册中一般只给出典型工作条件下的值

### 共源电感的影响

在给$C_{GS2}$充电期间，漏源极流过增长的电流，共源电感感应出电压，使得驱动电流减小，开关时间增长，极大地增加了器件导通时间

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210218201916070.png" alt="image-20210218201916070" style="zoom: 50%;" />

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210218192802785.png" alt="image-20210218192802785" style="zoom: 50%;" />

假定感应电压不变，且忽略驱动回路中寄生电感的影响
$$
V_{DD}=V_G+V_{GS}+V_{LS}
$$

$$
V_{DD}=R_GI_G+V_{GS}+\frac{L_SI_{DS}}{t_{CR\_CSI}}
$$

栅极电流$I_G$由下式给出
$$
I_G=\frac{Q_{CS2}}{t_{CR\_CSI}}=\frac{C_{GS}I_{DS}}{t_{CR\_CSI}g_m}
$$


$g_m$为跨导,其值为$I_{DS}/V_{GS}$

由上式可以推出$t_{CR\_CSI}$的表达式
$$
t_{CR\_CSI}=\frac{I_{DS}}{V_{DD}-V_{GS}}\frac{C_{gs}}{g_m}(\frac{L_sg_m}{C_{GS}}+R_G)
$$
该式可以提取出等效共源电感电阻$R_{CSI}$
$$
R_{CSI}=\frac{L_sg_m}{C_{GS}}
$$
该等效电阻可以用于计算电流转换周期

### 功率环路电感的影响

关断期间，功率环路电感会减慢转换速度，并增加漏极和源极上的电感

开通期间，该电感会降低漏源电压以降低损耗

总体来说该电路对电路产生的影响依然是负面的

### 并联肖特基二极管

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210218203913830.png" alt="image-20210218203913830" style="zoom: 50%;" />

如图所示，相比于MOSFET，增强型GaN的正向导通压降更高，因此会产生更大的功耗

可以通过反并联肖特基二极管来减少该损耗

不过反并联二极管会带来反向恢复电荷的损耗

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210218204310355.png" alt="image-20210218204310355" style="zoom:50%;" />

