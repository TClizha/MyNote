## EMI Noise Reduction Techniques for High Frequency Power Converters

#### EMI来源

电磁干扰按照干扰途径分为四种：

* 辐射干扰
* 传导干扰
* 感性干扰
* 容性干扰

#### EMI测量方式

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210205230726877.png" alt="image-20210205230726877" style="zoom:33%;" />

FCC测量传导干扰的方式如上图所示

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210205232620089.png" alt="image-20210205232620089" style="zoom: 33%;" />

LISN（Line Impedance Stabilization Network）用于隔离电网和受测设备，并提供一个稳定的负载阻抗

测试时，交流电网的两根线都要安装LISN，分析仪接在其中一个LISN的50Ω电阻上

传导干扰有三种测量模式，分别是peak, average and quasi-peak

peak模式下只跟踪峰值

average只计算噪声的平均值

quasi-peak则是峰值和频率的折衷，它通过一个充电速度远大于放电速度的检波器检测噪声，当噪声频率增加时，其平均值也会随之增加

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210205234445904.png" alt="image-20210205234445904" style="zoom: 67%;" />

#### 噪声分析

EMI由共模噪声和差模噪声组成，其流经路径分别如图所示

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/20210205233814.png" style="zoom:33%;" />

根据EMI standard EN55022
$$
\begin{cases}
v_1=50(i_{DM}-i_{CM})\\
v_2=50(-i_{DM}-i_{CM})
\end{cases}
$$
将公式变形，可以得到共模噪声电压和差模噪声电压
$$
\begin{cases}
v_{DM}=50i_{DM}=\frac{v_1-v_2}{2}\\
v_{DM}=-50i_{CM}=\frac{v_1+v_2}{2}
\end{cases}
$$
共模噪声一般更难处理

#### 减少共模噪声的传统方法

为了减少共模噪声，人们提出了三种方法，分别是symmetry, balance和shielding

##### symmetry（对称）

共模噪声电流来自于寄生电容上的$dv/dt$，如果我们能产生一对大小相等，方向相反的的$dv/dt$，就可以把噪声抵消掉

以LLC全桥为例

![image-20210206001953250](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206001953250.png)

当$V_1$产生一个正向的$dv/dt$时，$V_2$会同时产生一个方向相反幅值相同的$dv/dt$，这就把共模噪声消除了

然而在实际电路中，$V_1$和$V_2$和通断时间难以完全吻合，这就导致噪声不能恰好抵消，降低了实际应用的效果

![image-20210206002550536](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206002550536.png)

另一个问题是现实中许多电路并不是对称的，如buck、boost等，为了构成对称电路，需要修改电路拓扑乃至增加电路部件

如boost电路，若将其修改为对称boost电路，需要增加一个二极管，并将原来的电感分成感量相同的两份

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206002833415.png" alt="image-20210206002833415" style="zoom: 33%;" />

尽管实验证明这种方式可以减少共模噪声，但增加的部件会增加电路损耗并且增加成本，得不偿失

#### balance（平衡）

balance基于Wheatstone bridge结构，力求不增加额外部件的情况下减小噪声

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206003420628.png" alt="image-20210206003420628" style="zoom:33%;" />

Wheatstone bridge如图所示，当负载满足下式要求时，BD间电压为零
$$
\frac{Z_1}{Z_2}=\frac{Z_3}{Z_4}
$$
同样是以boost电路为例，将boost电路电感分为两部分，修改为下图

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206003751209.png" alt="image-20210206003751209" style="zoom: 33%;" />

其中$C_d$为MOSFET漏极与地（外壳）的寄生电容，$C_a$为二极管和地之间的寄生电容，$C_c$为负载、线路与地之间的寄生电容

正常工作时，MOSFET可以看作是一个方波电压源，用方波替代MOSFET得到下图

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206004849191.png" alt="image-20210206004849191" style="zoom: 50%;" />

噪声为高频交流信号，在高频频带中，$V_{in}$、$C_L$和二极管可看作短路**(此处不太明白)**，这样电路便构成了Wheatstone bridge

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206005233659.png" alt="image-20210206005233659" style="zoom:50%;" />

利用（3）式，可以得到CM完全消失的条件
$$
\frac{Z_{L1}}{Z_{L2}}=\frac{Z_{Cd}}{Z_{Ca}+Z_{Cc}}
$$
由于阻抗比不一定为1，balance方法电路灵活度高，而且无需增加部件

然而，balance方法仅在低频有效

在高频下，在电感中寄生电阻和寄生电容会改变阻抗比，进而导致高频下平衡被破坏

![image-20210206010014772](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206010014772.png)

如图所示，balance对超过2MHZ的噪声几乎不起作用

解决这一问题的方法是令$L_1$和$L_2$耦合，如下图所示

![image-20210206010508340](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206010508340.png)

这样balance便可以被简化为
$$
n=\frac{C_b}{C_d}
$$
//TODO: WHY?

然而，这一公式是在假定电感耦合系数是1的情况下推出的，一般情况下，使用利兹线的耦合电感耦合系数不会超过0.8，这一问题导致实际应用中高频下抑制共模噪声的能力依然不尽人意

![image-20210206012026277](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206012026277.png)

#### shielding（屏蔽）

对于隔离DC/DC变换器而言，共模噪声的主要传播路径是变压器初次级之间的耦合电容

以flyback电路为例，该电路有两个干扰源，都要经过变压器

因此，shielding方法成了抑制共模噪声的重要方法

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206012357682.png" alt="image-20210206012357682" style="zoom: 50%;" />

shielding的一种实现方法如图所示，寄生电容主要排布在BB'和CD之间

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206012544829.png" alt="image-20210206012544829" style="zoom: 50%;" />

假设线路内阻与长度成正比，那么同一层下线路压降与线长成正比，L与V的关系可以表现如下

![image-20210206012813353](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206012813353.png)

通过仔细的设计和计算，我们可以将BB'和CD上每一根对应的线上电势设计得一样高，那么寄生电感两端电压为零，自然就没有共模噪声流过

然而，该方法要求变压器的初次级配合得非常好，稍有偏差，就会如下图一样，起不到应有的屏蔽作用

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206013313282.png" alt="image-20210206013313282" style="zoom:50%;" />

shielding的另外一种实现方法如图所示，通过增加橙色的屏蔽层，寄生通道可以被分为两部分

一部分是上层的次级到屏蔽层的通道，该通道流过$i_2$；一部分是下层的初级到次级的通道，该通道流过$i_1$

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206013401721.png" alt="image-20210206013401721" style="zoom:50%;" />

<img src="https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210206014025096.png" alt="image-20210206014025096" style="zoom:50%;" />

当$i_1=i_2$时，共模噪声被完全限制在了设备内部，不再影响外部

此时电路满足的条件为
$$
C_{AC}\frac{dV_A}{dt}=C_{BD}\frac{dV_D}{dt}
$$
然而，这个方式有两个问题

* 初次级的$dv/dt$来自于不同的设备，它们之间很可能没有一个稳定的比值
* 在实际生产中，$C_{AC}$和$C_{BD}$非常难计算和控制