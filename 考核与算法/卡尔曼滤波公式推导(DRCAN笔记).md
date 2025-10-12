本教程来源于bibili up主DR\_CAN的笔记整理，建议读者配合视频食用，视频链接如下：

[【卡尔曼滤波器】1\_递归算法\_Recursive Processing\_哔哩哔哩\_bilibili](https://www.bilibili.com/video/BV1ez4y1X7eR?spm_id_from=333.999.0.0 "【卡尔曼滤波器】1_递归算法_Recursive Processing_哔哩哔哩_bilibili")

在此非常感谢DR\_CAN老师精彩讲解，在此表示崇高的敬意。

## 1 卡尔曼滤波介绍

卡尔曼滤波算法实际是一个观测器，可以用来估计下一个状态，具有非常好的实时性。

其公式为：

假定我们要对一个未知长度的物体进行测量，每次的测量值为 $Z_k$，取前 $k$ 次的平均值作为当前的估计值（最优估计），则可得到以下公式：

$$
\hat{x_k} = \frac{Z_1+Z_2+....+Z_k}{k}
$$

$$
= \frac{1}{k}(Z_1+Z_2+....+Z_{k-1}+\frac{1}{k}Z_k
$$

$$
= \frac{1}{k}\frac{k-1}{k-1}(z_1+z_2+....+z_{k-1})+\frac{1}{k}z_k
$$

$$
= \frac{k-1}{k}\hat{x_{k-1}}+\frac{1}{k}z_k
$$

$$
= \hat{x_{k-1}}-\frac{1}{k}\hat{x_{k-1}}+\frac{1}{k}z_k
$$

整理之后可以得到：

$$
\hat{x_k} = \hat{x_{k-1}}+K_k(Z_k-\hat{x_{k-1}}) \tag{1.1}
$$

其中 $K_k=\frac{1}{k}$（仅在本示例中等号成立），被称作卡尔曼增益（卡尔曼系数），随着测量次数 $k$ 的增加可以看到 $\hat{x_k}\to{\hat{x_{k-1}}}$，即随着测量次数增加，每次的测量值对最后的最优估计值起作用的效果越来越低，符合基本常识。

在广义的卡尔曼滤波中，卡尔曼增益 $K_k$ 的值应被如下定义：

$$
K_k = \frac{e_{EST_{k-1}}}{e_{EST_{k-1}}-e_{MEA_k}} \tag{1.2}
$$

式中：$e_{EST_{k-1}}$ 为上一次的估计误差，$e_{MEA_{k}}$ 为当前的测量误差。

可以对式1.2做一个分析，

$$
\begin{cases} 
& \text{ 当 } e_{EST_{k-1}}\gg e_{MEA_{k}}\text{时} &K_k\to1 & \hat{x_k}=\hat{x_{k-1}}+Z_k-\hat{x_{k-1}}=z_k &①\\
& \text{ 当 } e_{EST_{k-1}}\ll e_{MEA_{k}}\text{时} &K_k\to0 &\hat{x_k}=\hat{x_{k-1}} &② 
\end{cases}
$$

①式表明，当前一次（或前几次）的估计误差远大于当前测量误差时候，则其最优估计以当前测量值为准；

②式表明，当前一次（或前几次）的估计误差远小于当前测量误差时候，则其最优估计以前一次（或前几次）的估计值为准。

## 2 卡尔曼滤波计算过程

step1：计算卡尔曼增益 
$$
$K_k=\frac{e_{EST_{k-1}}}{e_{EST_{k-1}}-e_{MEA_k}}$
$$
step2：计算当前最优估计 $\hat{x_k}=\hat{x_{k-1}}+K_k(Z_k-\hat{x_{k-1}})$
$$
$\hat{x_k}=\hat{x_{k-1}}+K_k(Z_k-\hat{x_{k-1}})$
$$
step3：更新当前估计误差 $e_{EST_k}=(1-K_k)\times e_{EST_{k-1}}$，（后面会进行详细推导这个公式）
$$
 $e_{EST_k}=(1-K_k)\times e_{EST_{k-1}}$
$$
例如，对一个实际长度 $x=50mm$（此长度认为是真实长度，是未知的）的物体进行测量，第一次测量的测量值 $Z_1=51mm$，第0次的最优估计为 $\hat{x_0}=40mm$，第0次的估计误差为 $5mm$，测量误差为常值 $e_{MEA_1}=3mm$。则第一次迭代可得：

$$
\begin{aligned} 
k=1\qquad K_k &= \frac{e_{EST_0}}{e_{EST_0}+e_{MEA_1}}=\frac{5}{5+3}=0.625\\
\hat{x_k} &= 40+0.625\times(51-40)=46.875\\
e_{EST_k} &= (1-0.625)\times5=1.875 
\end{aligned}
$$

第二次迭代可得：

$$
\begin{aligned} 
k=2\qquad K_k &= \frac{e_{EST_1}}{e_{EST_1}+e_{MEA_2}}=\frac{1.875}{1.875+3}=0.3846\\
\hat{x_k} &= 46.875+0.3846\times(48-46.875)=47.308\\
e_{EST_k} &= (1-0.3846)\times1.875=1.154 
\end{aligned}
$$

多次测量，可得到如下表格：  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/da7e228f8f7b8d7d3b3b7a893099158f.png#pic_center)  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/e27738c0075067107cb096fd8a44949b.png#pic_center)

可以看到 $k<16$ 时候，随着测量次数增大，最优估计 $\hat{x_k}$ 趋近于真实值50，但当实际测量值增大时（真实值也增大），此时最优估计值无法跟踪，当增大的测量值趋于稳定后（真实值趋于稳定）此时重新设置 $\hat{x_{22}}=86$ 后，后续的卡尔曼滤波才可以趋于稳定，继续跟踪，这是因为，目前只涉及到卡尔曼滤波的的第一个方程，**状态改变的情况暂时未考虑进内**，所以呈现目前这种趋势。

## 3 前期知识准备

### 3.1 数据融合

举例：假设用两个不同的称来称一个的物体质量。测得其测量值为：  

$$
\begin{aligned} 
Z_1 &= 30g \qquad \sigma1=2g\\
Z_2 &= 32g \qquad \sigma2=4g\\
\end{aligned}
$$

其中 $\sigma$ 为标准差（standard Deviatim），则其测量值满足正态分布。接下来用这两组数据估计真实值 $\hat{Z}$。

利用第2节的卡尔曼滤波公式不难求得： $\hat{Z}=Z_1+K(Z_2-Z_1)$。

$$
\begin{aligned} 
K\in[0,1] \qquad K&=0,\hat{Z}=Z_1\\
K&=1,\hat{Z}=Z_2 
\end{aligned}
$$

若得到最优估计，则要使得 $\hat{Z}$ 的标准差最小，或者说 $\hat{Z}$ 的方差（Var）最小，则可得出 $\hat{Z}$ 的方差为：  

$$
\begin{aligned} 
\sigma^2_z &= Var(Z_1+K(Z_2-Z_1)\\
&= Var[(1-K)Z_1]+Var(KZ_2)\\
&= (1-K)^2Var(Z_1)+K^2Var(Z_2)\\
&= (1-K)^2\sigma^2_1+K^2\sigma^2_2 
\end{aligned}
$$

上式对K求导可得：  

$$
\frac{\mathrm{d} \sigma^2_z}{\mathrm{d} K} = -2(1-K)\sigma^2_1+2K\sigma_2^2=0\qquad K=\frac{\sigma_1^2}{\sigma_1^2+\sigma^2_2}
$$

带入可得到 $K = 0.2$，$\sigma_z^2 = 3.2$，$\sigma_z = 1.79$，$\hat{Z} = 30.4$。在最小方差的基础上我们得到了最优估计为30.4mm。表示在下图，蓝线表示 $Z_1$ 的正态分布，红线表示 $Z_2$ 的正态分布。黑线表示 $\hat{Z}$ 的正态分布，可以看到方差/标准差明显减小。  
![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/b5b72b9eccb10e4bc4e213c537bc7d58.png#pic_center)

### 3.2 协方差矩阵

#### 3.2.1 简要介绍

方差、协方差表示的是变量之间的联动关系。

举例：计算球员身高、体重、年龄的方差和协方差。

| 球员 | 身高(x) | 体重(y) | 年龄(z) |
| --- | --- | --- | --- |
| 瓦尔迪 | 179 | 74 | 33 |
| 奥巴梅杨 | 187 | 80 | 31 |
| 萨拉赫 | 175 | 71 | 28 |
| 平均值 | 180.3 | 75 | 30.7 |

则方差为：  

$$
\begin{aligned} 
\sigma_x^2 &= \frac{1}{3}[(179-180.3)^2+(187-180.3)^2+(175-180.3)^2=24.89\\
\sigma_y^2 &= 14\\
\sigma_z^2 &= 4.22 
\end{aligned}
$$

协方差为：  

$$
\begin{aligned} 
\sigma_x\sigma_y &= \frac{1}{3}[(179-180.3)(74-75)+(187-180.3)(80-75)+(175-180.3)(71-75)]=18.7=\sigma_y\sigma_x\\
\sigma_x\sigma_z &= 4.4=\sigma_z\sigma_x\\
\sigma_y\sigma_z &= 3.3=\sigma_z\sigma_y 
\end{aligned}
$$

协方差表示的是两个元素的相关性，如果数值越大，说明两个变量相关性越大。

上述协方差矩阵可以表示为：  

$$
P= \begin{bmatrix} \sigma_x^2&\sigma_x\sigma_y&\sigma_x\sigma_z\\ \sigma_y\sigma_x&\sigma_y^2&\sigma_y\sigma_z\\ \sigma_z\sigma_x&\sigma_z\sigma_y&\sigma_z^2\\ \end{bmatrix}= \begin{bmatrix} 24.89&18.7&4.4\\ 18.7&14&3.3\\ 4.4&3.3&4.22\\ \end{bmatrix}
$$

#### 3.2.2 期望，方差、协方差重要公式

数学期望的标准定义为：  

$$
E(x)=\int_{-\infty}^{+\infty}xf(x)dx
$$

其中 $f(x)$ 称作是x的概率密度。

数学期望的性质：

①如果C是常数，则有 $E(C)=C$。

②设X为随机变量，C是常数，则 $E(CX)=CE(X)$。

③设X，Y为两个随机变量，则有 $E(X+Y)=E(X)+E(Y)$。

④设X，Y为两个**相互独立**的随机变量，则有 $E(XY)=E(X)E(Y)$。第④条性质用数学期望的定义来证明。

方差D(X)或者叫做Var(X)的标准定义为：  

$$
\begin{aligned} 
D(X) &= E\left \{[X-E(X)]^2\right\}\\
&= E[X^2-2XE(X)+E^2(X)]\\
&= E(X^2)-2E(X)E(X)+E^2(X)\\
&= E(X^2)-E^2(X) 
\end{aligned}
$$

协方差的标准定义为：  

$$
\begin{aligned} 
Cov(X,Y) &= E\left\{[X-E(X)][Y-E(Y)]\right\}\\
&= E[XY-XE(Y)-YE(X)+E(X)E(Y)]\\
&= E(XY)-E(X)E(Y)-E(X)E(Y)+E(X)E(Y)\\
&= E(XY)-E(X)E(Y) 
\end{aligned}
$$

#### 3.2.3 通过矩阵运算计算协方差

首先求出过渡矩阵：  

$$
a= \begin{bmatrix} x_1&y_1&z_1\\ x_2&y_2&z_2\\ x_3&y_3&z_3\\ \end{bmatrix} -\frac{1}{3} \begin{bmatrix} 1&1&1\\ 1&1&1\\ 1&1&1\\ \end{bmatrix} \begin{bmatrix} x_1&y_1&z_1\\ x_2&y_2&z_2\\ x_3&y_3&z_3\\ \end{bmatrix}
$$

其中  

$$
\frac{1}{3} \begin{bmatrix} 1&1&1\\ 1&1&1\\ 1&1&1\\ \end{bmatrix} \begin{bmatrix} x_1&y_1&z_1\\ x_2&y_2&z_2\\ x_3&y_3&z_3\\ \end{bmatrix}
$$

表示的是x，y，z的平均值。

则可得到协方差矩阵为：

$$
P=\frac{1}{3}a^Ta
$$

### 3.3 状态空间方程

举例：对于一个弹簧阻尼系统。

![在这里插入图片描述](https://i-blog.csdnimg.cn/blog_migrate/f73f0486c9a11acd92d7182717545d07.png#pic_center)

可得到其动力学方程为： $m\ddot{x}+B\dot{x}+kx=F$，令F作为输入u，则上式可以表示为：  
$m\ddot{x}=u-B\dot{x}-kx$
设置两个状态变量， $x_1=x, x_2=\dot{x}$，同时我们定义测量值， $z_1=x=x_1$（测位置）， $z_2= \dot{x} =x_2$（测速度）,则可得到以下公式：  

$$
\begin{aligned} 
\dot{x_1} &= x_2\\
\dot{x_2} &= \frac{1}{m}u-\frac{B}{m}x_2-\frac{k}{m}x_1\\
z_1 &= x=x_1\\
z_2 &= \dot{x}=x_2 
\end{aligned}
$$

可以得到矩阵形式为：  

$$
\begin{aligned} 
\begin{bmatrix} \dot{x_1}\\ \dot{x_2}\\ \end{bmatrix} &= \begin{bmatrix} 0&1\\ -\frac{k}{m}&-\frac{B}{m}\\ \end{bmatrix} \begin{bmatrix} x_1\\ x_2\\ \end{bmatrix} + \begin{bmatrix} 0\\ \frac{1}{m} \end{bmatrix} u\\
\begin{bmatrix} z_1\\ z_2\\ \end{bmatrix} &= \begin{bmatrix} 1&0\\ 0&1\\ \end{bmatrix} \begin{bmatrix} x_1\\ x_2\\ \end{bmatrix} 
\end{aligned}
$$

进而我们可以得到系统状态随时间别的变化趋势为：  

$$
\begin{aligned} 
\dot{x}(t) &= Ax(t)+Bu\\
z(t) &= Hx(t)\\
\end{aligned}
$$

对此方程进行离散化操作（这部分对离散化后的下标是k还是k-1存在疑问），k表示采样的时间单位，则有  

$$
\begin{aligned} 
x_k &= Ax_{k-1}+Bu_{k-1}\\
z_k &= Hx_k 
\end{aligned}
$$

对等式进行进一步叠加，引入过程噪音 $w_{k-1}$ 和测量噪音 $v_k$ 后可以得到下列公式：  

$$
\begin{aligned} 
\text{计算方程:}\qquad x_k &= Ax_{k-1}+Bu_{k-1}+\omega_{k-1}\\
\text{测量方程:}\qquad z_k &= Hx_k+v_k 
\end{aligned}
$$

可以看到在加入过程噪音和测量噪音后，计算方程和测量方程都会变得不准确，在这种情况下，如何计算出尽可能接近真实值的**最优估计** $\hat{x}$，就是卡尔曼滤波所要解决的问题。

## 4 卡尔曼滤波算法详细推导

### 4.1 线性系统

#### 4.1.1 系统分析

设系统的状态空间方程为：

$$
\begin{aligned} 
x_k &= Ax_{k-1}+Bu_{k-1}+\omega_{k-1}\\
z_k &= Hx_k+v_k\\
\end{aligned} \tag{4.0}
$$

其中 $w$ 为**系统噪声**，满足正态分布 $P(\omega)\sim(0,Q)$,期望 $E(\omega)=0$，协方差矩阵为 $Q=E(ww^T)$，同理 $v$ **为测量噪声**，满足正态分布 $P(v)\sim(0,R)$,期望 $E(v)=0$，协方差矩阵为 $R=E(vv^T)$。

> 下面先对 $Q=E(ww^T)$ 进行推导说明：
>
> 假设有两个状态
> $$
> \begin{bmatrix}x_1\\x_2\end{bmatrix}
> $$
> ，对应的系统噪声为
> $$
> \begin{bmatrix}\omega_1\\\omega_2\end{bmatrix}
> $$
> ，则 
> $$
> \begin{bmatrix}\omega_1\\\omega_2\end{bmatrix}\begin{bmatrix}\omega_1&\omega_2\end{bmatrix}=\begin{bmatrix}\omega_1^2&\omega_1\omega_2\\\omega_2\omega_1&\omega_2^2\end{bmatrix}
> $$
> ，对这个式子求期望得到 
> $$
> \begin{bmatrix}E(\omega_1^2)&E(\omega_1\omega_2)\\E(\omega_2\omega_1)&E(\omega_2^2)\end{bmatrix}
> $$
> 。

根据方差公式和协方差公式
$$
D(X)=E(X^2)-E^2(X)
$$

$$
Cov(X,Y)=E(XY)-E(X)E(Y)
$$



> 因为
> $$
> E(\omega)=0
> $$
> ，所以 
> $$
> \begin{bmatrix}E(\omega_1^2)&E(\omega_1\omega_2)\\E(\omega_2\omega_1)&E(\omega_2^2)\end{bmatrix}=\begin{bmatrix}\sigma_{\omega_1}^2&\sigma_{\omega_1}\sigma_{\omega_2}\\\sigma_{\omega_2}\sigma_{\omega_1}&\sigma_{\omega_2}^2\end{bmatrix}=Q
> $$
> ，这里的
> $$
> \sigma_{\omega_1}\sigma_{\omega_2}
> $$
>  表示的是协方差，不是两个标准差相乘。

接下来引入估计值：  

$$
\hat{x_k}^-=A\hat{x_{k-1}}+Bu_{k-1}(\hat{x_k}^-称为先验估计值)\tag{4.1}
$$

$$
z_k=Hx_k\to\hat{x_{MEAk}}=H^{-1}z_k\tag{4.2}
$$

其中（4.1）是算出来的估计值，（4.2）是测出来的估计值。利用3.1节数据融合的理论，我们可以得到另一组公式：  

$$
\begin{aligned} 
\hat{x_k} &= \hat{x_k}^-+G(\hat{x_{MEA_k}}-\hat{x_k}^-)\\
&= \hat{x_k}^-+G(H^{-1}z_k-\hat{x_k}^-) 
\end{aligned}
$$

可以看到  

$$
\left\{\begin{matrix} 当G=0时， &\hat{x_k}=\hat{x_k^-} \\ 当G=1时， &\hat{x_k}=H^{-1}z_k=\hat{x_{MEAk}} \end{matrix}\right.
$$

我们令 $G=K_kH$，则可得到：  

$$
\hat{x_k}=\hat{x_k}^-+K_k(z_k-H\hat{x_k}^-),K_k\in[0,H^{-1}]\tag{4.3}
$$

**卡尔曼滤波的目标是，找到一个** $K_k$，**使得** $\hat{x_k}\to{x_k}$，$x_k$ **指的是真实值**。

#### 4.1.2 如何找到满足条件的卡尔曼增益?

设误差 $e_k=x_k-\hat{x_k}$，误差满足正态分布 $P(e_k)\sim(0,P)$，若误差最小，则必须误差接近于误差的期望0，意味着方差必须尽可能小。  

$$
P=E[ee^T]= \begin{bmatrix}\sigma_{e_1}^2&\sigma_{e_1}\sigma_{e_2}\\\sigma_{e_2}\sigma_{e_1}&\sigma_{e_2}^2\end{bmatrix}\tag{4.4}
$$

要使得误差的方差最小，则要使得矩阵P的**迹**最小，即 $tr(p)=\sigma_{e_1}^2+\sigma_{e_2}^2$ 最小，将4.0和4.3带入 $e_k$ 计算公式可得  

$$
\begin{aligned} 
e_k=x_k-\hat{x_k} &= x_k-[\hat{x_k}^-+K_k(z_k-H\hat{x_k}^-)]\\
&= x_k-\hat{x_k}^--K_kz_k+K_kH\hat{x_k}^-\\
&= x_k-\hat{x_k}^--K_kHx_k-K_kv_k+K_kH\hat{x_k}^-\\
&= x_k-\hat{x_k}^--K_kH(x_k-\hat{x_k}^-)-K_kv_k\\
&= (I-K_kH)(x_k-\hat{x_k}^-)-K_kv_k\\
&= (I-K_kH)e_k^--K_kv_k 
\end{aligned} \tag{4.5}
$$

将4.5带入4.4可得  

$$
\begin{aligned} 
P_k=E[e_ke_k^T] &= E[(I-K_kH)e_k^--K_kv_k][(I-K_kH)e_k^--K_kv_k]^T\\
&= E[(I-K_kH)e_k^-e_k^{-T}(I-K_kH)^T-(I-K_kH)e_k^-v_k^TK_k^T-K_kv_ke_k^{-T}(I-K_kH)^T+K_kv_kv_k^TK_k^T] 
\end{aligned}
$$

把E放进去，因为 $e_k^-$ 和 $v_k$ 相互独立，第二项和第三项中 $E(e_k^-v_k^T)=0$，上式继续化简为  

$$
P_k=(I-K_kH)E(e_k^-e_k^{-T})(I-K_kH)^T+K_kE(V_kV_k^T)K_k^T
$$

因为 $P_k=E(e_ke_k^T)$，因此，$P_k^-=E(e_k^-e_k^{-T})$，同理 $R=R_k=E(V_kV_k^T)$，上式继续化简为  

$$
\begin{aligned} 
P_k &= (I-K_kH)P_k^-(I-K_kH)^T+K_kRK_k^T\\
&= (P_k^--K_kHP_k^-)(I-H^TK_k^T)+K_kRK_k^T\\
&= P_k^--K_kHP_k^--P_k^-H^TK_k^T+K_kHP_k^-H^TK_k^T+K_kRK_k^T\\
&= P_k^--K_kHP_k^--(K_kHP_k^-)^T+K_kHP_k^-H^TK_k^T+K_kRK_k^T\\
\end{aligned} \tag{4.6}
$$

对4.6式处理，求 $P_k$ 的迹  

$$
tr(P_k)=tr(P_k^-)-2tr(K_kHP_k^-)+tr(K_kHP_k^-H^TK_k^T)+tr(K_kRK_k^T)\tag{4.7}
$$

> 此处对两个公式进行说明：  
> 
$$
\begin{aligned} 
\frac{dtr(AB)}{dA} &= B^T\\
\frac{dtr(ABA^T)}{dA} &= A(B+B^T)\\
\text{当B为对称矩阵时}\frac{dtr(ABA^T)}{dA} &= 2AB\\
\end{aligned}
$$

使 $tr(P_k)$ 对卡尔曼增益 $K_k$ 进行求导，求极小值，令导数为0，求得最优的 $K_k$  

$$
\begin{aligned} 
\frac{dtr(P_k)}{dK_k} &= 0-2(HP_k^-)^T+2K_kHP_k^-H^T+2K_kR=0\\
K_k(HP_k^-H^T+R) &= P_k^-H^T 
\end{aligned}
$$

得到  

$$
\begin{aligned} 
K_k &= \frac{P_k^-H^T}{HP_k^-H^T+R}\\
K_k &= P_k^-H^T(HP_k^-H^T+R)^{-1}\text{严格表示} 
\end{aligned} \tag{4.8}
$$

> 对应于 $\hat{x_k}=\hat{x_k}^-+K_k(z_k-H\hat{x_k}^-)$，$K_k\in[0,H^{-1}]$ 可以分析如下
>
> 当R非常大时，说明测量值很不准确，此时
> $$
> K_k\to{0}
> $$
> ，
> $$
> \hat{x_k}\to{\hat{x_k^-}}
> $$
> ，最优估计以先验值（或者说上一次的估计值，或者说计算得到的值）为准；
>
> 当R比较小时，说明测量值比较准确，此时 $K_k\to{H^-}$，$\hat{x_k}\to{z_k}$，最优估计以当前测量值为准；

在4.8式中只有 P_k^- 
$$
P_k^-
$$
未知，现在求 P_k^-。

#### 4.1.3 求误差的协方差矩阵的先验值

首先求出 $e_k^-$  

$$
\begin{aligned} 
e_k^-=x_k-\hat{x_k^-} &= Ax_{k-1}+Bu_{k-1}+\omega_{k-1}-A\hat{x_{k-1}}-Bu_{k-1}\\
&= A(x_{k-1}-\hat{x_{k-1}})+\omega_{k-1}\\
&= Ae_{k-1}+\omega_{k-1}\\
\end{aligned}
$$

然后求 $P_k^-$  

$$
\begin{aligned} 
P_k^- &= E(e_k^-e_k^{-T})\\
&= E[(Ae_{k-1}+\omega_{k-1})(Ae_{k-1}+\omega_{k-1})^T]\\
&= E[(Ae_{k-1}+\omega_{k-1})(e_{k-1}^TA^T+\omega_{k-1}^T)]\\
&= E(Ae_{k-1}e_{k-1}^TA^T+Ae_{k-1}\omega_{k-1}^T+\omega_{k-1}e_{k-1}^TA^T+\omega_{k-1}\omega_{k-1}^T) 
\end{aligned} \tag{4.9}
$$

因为 $e_{k-1}$ 和 $\omega_{k-1}$ 相互独立，所以第二项和第三项的期望为0。

> 说明  
> 
$$
\begin{aligned} 
e_{k-1} &= x_{k-1}-\hat{x_{k-1}}\\
x_k &= Ax_{k-1}+Bu_{k-1}+\omega_{k-1}\\
\end{aligned}
$$
> 
> 可以看到 $e_{k-1}$ 表示的是k-1次的误差，$\omega_{k-1}$ 作用的是第k次的x，所以二者相互独立。

继续推导4.9式  

$$
\begin{aligned} 
P_{k}^- &= AE(e_{k-1}e_{k-1}^{T})A^T+E(\omega_{k-1}\omega_{k-1}^T)\\
&= AP_{k-1}A^T+Q 
\end{aligned} \tag{4.10}
$$

#### 4.1.4 求误差的协方差矩阵

根据4.6式我们得到  

$$
P_k=P_k^--K_kHP_k^--(K_kHP_k^-)^T+K_kHP_k^-H^TK_k^T+K_kRK_k^T
$$

化简  

$$
P_k=P_k^--K_kHP_k^--P_k^{-T}H^Tk_k^T+K_kHP_k^-H^TK_k^T+K_kRK_k^T
$$

因为 $P$ 为实对称矩阵，因此 $P_k^{-T}=P_k^-$，上式等于  

$$
P_k=P_k^--K_kHP_k^--P_k^{-}H^Tk_k^T+K_k(HP_k^-H^T+R)K_k^T
$$

将 $K_k=\frac{P_k^-H^T}{HP_k^-H^T+R}$ 带入，得到  

$$
P_k=P_k^--K_kHP_k^-
$$

$$
P_k=(I-K_kH)P_k^-\tag{4.11}
$$

自此卡尔曼滤波五大公式推导完毕。

#### 4.1.5 卡尔曼滤波算法五大公式

先验：
$$
\hat{x_k}^--=A\hat{x_{k-1}}+Bu_{k-1}
$$
先验误差协方差：P_{k}^-=AP_{k-1}A^T+Q
$$
P_{k}^-=AP_{k-1}A^T+Q
$$
卡尔曼增益：
$$
K_k=\frac{P_k^-H^T}{HP_k^-H^T+R}
$$
误差协方差：
$$
P_k=(I-K_kH)P_k^-
$$
最优估计（后验估计）：
$$
\hat{x_k}=\hat{x_k}^-+K_k(z_k-H\hat{x_k}^-)
$$


#### 4.1.6 卡尔曼滤波示例加编程

示例，研究一个物体匀速前进的系统，状态有两个，一个是位置设为 $X_1$，一个是速度设为 $X_2$，则存在以下方程：  

$$
\begin{aligned} 
\text{位置：}x_{1,k} &= x_{1,k-1}+\Delta{T}x_{2,k-1}+\omega_{1,k-1}\\
\text{速度：}x_{2,k} &= x_{2,k-1}+\omega_{2,k-1} 
\end{aligned}
$$

我们令 $\Delta{T}=1$，则有

$$
\begin{aligned} 
\text{位置：}x_{1,k} &= x_{1,k-1}+x_{2,k-1}+\omega_{1,k-1}\\
\text{速度：}x_{2,k} &= x_{2,k-1}+\omega_{2,k-1} 
\end{aligned}
$$

测量方程可以写为  

$$
\begin{aligned} 
Z_{1,k} &= x_{1,k}+v_{1,k}\\
Z_{2,k} &= x_{2,k}+v_{2,k}\\
\end{aligned}
$$

因此，状态方程可以写为：

$$
\begin{aligned} 
\begin{bmatrix} x_{1,k}\\ x_{2,k} \end{bmatrix} &= \begin{bmatrix} 1&1\\ 0&1 \end{bmatrix} \begin{bmatrix} x_{1,k-1}\\ x_{2,k-1} \end{bmatrix} + \begin{bmatrix} \omega_{1,k-1}\\ \omega_{2,k-1} \end{bmatrix}\\
\begin{bmatrix} z_{1,k}\\ z_{2,k} \end{bmatrix} &= \begin{bmatrix} 1&0\\ 0&1 \end{bmatrix} \begin{bmatrix} x_{1,k}\\ x_{2,k} \end{bmatrix} + \begin{bmatrix} v_{1,k}\\ v_{2,k} \end{bmatrix} 
\end{aligned}
$$

在本示例中我们需要预先知道的初值有：  

$$
\begin{aligned} 
Q &= \begin{bmatrix} 0.1&0\\ 0&0.1 \end{bmatrix}， P= \begin{bmatrix} 1&0\\ 0&1 \end{bmatrix}， A= \begin{bmatrix} 1&1\\ 0&1 \end{bmatrix}， H= \begin{bmatrix} 1&0\\ 0&1 \end{bmatrix}， \text{单位矩阵} E= \begin{bmatrix} 1&0\\ 0&1 \end{bmatrix}\\
P_0 &= \begin{bmatrix} 1&0\\ 0&1 \end{bmatrix}， x_{(1,2)_0}=\hat{x_{(1,2)_0}}= \begin{bmatrix} 0\\ 1 \end{bmatrix} 
\end{aligned}
$$

根据上述方程和卡尔曼滤波公式，递推300次，编写以下程序：