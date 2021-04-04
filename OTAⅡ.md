![image-20210307204510753](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image-20210307204510753.png)

与常见的运放不同，gm为跨导放大器，补偿网络接地而不是接在FB上

上图所示为二阶补偿

![Technical Document Image Preview](https://cdn.jsdelivr.net/gh/TClizha/PicCloud/img/image012.jpg)

这个$R_o$不知道有什么用，不知道是不是画多了

其传递函数为
$$
\frac{V_{CNTL}}{V_O}=\frac{R_{F2}}{R_{F1}+R_{F2}}\cdot G_m\cdot\frac{1}{C_{C1}+C_{C_2}}\cdot\frac{1+sR_{C1}C_{C1}}{s\cdot(1+sR_{C1}\cdot\frac{C_1C_2}{C_1+C_2})}
$$
标准形式为
$$
A\cdot \frac{(1+\frac{s}{\omega_{CZ1}})}{s\cdot(1+\frac{s}{\omega_{CP1}})}
$$
bode图如下图所示，具有两个极点和一个零点

![Technical Document Image Preview](https://www.richtek.com/~/media/Richtek/Design%20Support/Technical%20Documentation/AN052/TW/Version1/image011.jpg?file=preview.png)

