# RIS-Relay-compare
 compare RIS with traditional FD/HD AF/DF

[1] 对于AF 和DF的协议说的比较明朗  而且还给出了具体的表达式；他假定发端收端都是single antenna，relay是多天线，这样就构成一个MIMO；relay处的beamforming策略是按照MRT来的。

首先按照这篇文章复现AF和DF系统，然后做RIS仿真对AF、DF进行对比。
# System Model

## LS-MIMO Relay
考虑一个时分大规模MIMO中继通信系统，发端和收端都是单天线，中继是多天线。窃听者（被动窃听者）也是单天线。$N_R\geq100$。该系统工作在半双工状态，所以工作时间分为两个时隙。具体来讲，在第一个时隙，发端向中继发送信号，在第二个时隙中继将处理过后对信号传递给收端。
> [1] Specifically, during the first time slot, the source transmits the signal to the relay, and then the relay forwards the post-processing signal to the destination within the second time slot.

注意到发端到收端到直连链路是不可用的（距离太长）。

> [1] Note that the direct link from the source to the destination is unavailable due to a long distance. Meanwhile, the eavesdropper monitors the transmission from the relay to the destination, and tries to intercepet

在[1]中，作者假设：在窃听者看来，窃听的对象是relay发出的信号，这样假设的理由引用了[1-14], [1-17]。

信道信息表达式为：$\sqrt{\alpha_{i, j}} \mathbf{h}_{i, j}$,S（Source）、R（Relay）、D（Destanation）、E（Evasdropper）。

那么在第一个时隙中继接收发端的信号为：
$$
\mathbf{y}_{R}=\sqrt{P_{S} \alpha_{S, R}} \mathbf{h}_{S, R} s+\mathbf{n}_{R}
$$
Relay会生成一个发送信号${\bf r}$，那么收端和窃听端接收到的信号分别为：
$$
y_{D}=\sqrt{P_{R} \alpha_{R, D}} \mathbf{h}_{R, D}^{H} \mathbf{r}+n_{D}
$$
$$
y_{E}=\sqrt{P_{R} \alpha_{R, E}} \mathbf{h}_{R, E}^{H} \mathbf{r}+n_{E}
$$
保密通信速率借鉴了文献[1-5]:
$$
C_{S E C}=\left[C_{D}-C_{E}\right]^{+}, \text {where }[x]^{+}=\max (x, 0)
$$
> P. K. Gopala, L. Lai, and H. El. Gamal, “On the secrecy capacity of fading channels,” IEEE Trans. Inf. Theory, vol. 54, no. 10, pp. 4687–4698, Oct. 2008.

因为并不存在稳定的信道条件使得安全通信速率保持在一个稳定的值，这里采用安全通信中断概率(secrecy outage capacity)来评价通信的保密程度：
$$
P_{r}\left(C_{S O C}>C_{D}-C_{E}\right)=\varepsilon
$$
## AF relay
当中继接收到信号$\mathbf{y}_{R}$时，会乘以一个$N_{R} \times N_{R}$的处理矩阵$\mathbf{F}$，那么中继发射的信号为：
$$
\mathbf{r}^{A F}=\mathbf{F} \mathbf{y}_{R}
$$
作者假设第一跳的CSI$\mathbf{h}_{S, R}$可以被完美估计，但是第二跳的CSI由于系统存在半双工延时，这能获取估计的CSI$\hat{\mathbf{h}}_{R, D}$
> [1] We assume the relay has full CSI hS,R through channel estimation, and gets partial CSI hR,D by making use of channel reciprocity in TDD systems. Due to duplex delay between uplink and downlink, there is a certain degree of mismatch between the estimated CSI hˆR,D and the real CSI hR,D, whose relation can be expressed as [37]

同时，窃听者的CSI是无法估计的$\mathbf{h}_{R, E}$。所以${\bf F}$根据$\mathbf{h}_{S, R}$和$\hat{\mathbf{h}}_{R, D}$制定的，而和$\mathbf{h}_{R, E}$没有关系。对于${\bf F}$的制定，作者采用了 maximum ratio combination MRC和 maximum ratio transmission MRT。

> [1] Since maximum ratio combination (MRC) and maximum ratio transmission (MRT) can achieve asymptotically optimal performance in LS-MIMO systems with low complexity [27], [28], we design F by combining MRC and MRT

最大比合并的来源参考[2]。
$$
\mathbf{F}=\frac{\hat{\mathbf{h}}_{R, D}}{\left\|\hat{\mathbf{h}}_{R, D}\right\|} \frac{1}{\sqrt{P_{S} \alpha_{S, R}\left\|\mathbf{h}_{S, R}\right\|^{2}+1}} \frac{\mathbf{h}_{S, R}^{H}}{\left\|\mathbf{h}_{S, R}\right\|}
$$
那么AF Relay条件下接收端接收的信号为：
$$
\begin{array}{r}
y_{D}^{A F}=\frac{\sqrt{P_{S} P_{R} \alpha_{S, R} \alpha_{R, D}} \mathbf{h}_{R, D}^{H} \hat{\mathbf{h}}_{R, D} \mathbf{h}_{S, R}^{H} \mathbf{h}_{S, R}}{\left\|\hat{\mathbf{h}}_{R, D}\right\| \sqrt{P_{S} \alpha_{S, R}\left\|\mathbf{h}_{S, R}\right\|^{2}+1}\left\|\mathbf{h}_{S, R}\right\|} s +\frac{\sqrt{P_{R} \alpha_{R, D}} \mathbf{h}_{R, D}^{H} \hat{\mathbf{h}}_{R, D} \mathbf{h}_{S, R}^{H}}{\left\|\hat{\mathbf{h}}_{R, D}\right\| \sqrt{P_{S} \alpha_{S, R}\left\|\mathbf{h}_{S, R}\right\|^{2}+1}\left\|\mathbf{h}_{S, R}\right\|} \mathbf{n}_{R}+n_{D}
\end{array}
$$
其信噪比为：
$$
\begin{aligned}
\gamma_{D}^{A F}=& \frac{\left.\frac{P_{S} P_{R} \alpha_{S, R} \alpha_{R, D} \mid \mathbf{h}_{R, D}^{H}}{\left\|\hat{\mathbf{h}}_{R, D}\right\|^{2}\left(P_{S} \alpha_{S, R} \|\right.}\left\|_{S, R}\right\|^{2}+1\right)}{\frac{P_{R} \alpha_{R, D}\left|\mathbf{h}_{R, D}^{H} \hat{\mathbf{h}}_{R, D}\right|^{2}}{\left\|\hat{\mathbf{h}}_{R, D}\right\|^{2}\left(P_{S} \alpha_{S, R}\left\|\mathbf{h}_{S, R}\right\|^{2}+1\right)}+1} \\
&=\frac{a\left|\mathbf{h}_{R, D}^{H} \hat{\mathbf{h}}_{R, D}\right|^{2}\left\|\mathbf{h}_{S, R}\right\|^{2}}{b\left|\mathbf{h}_{R, D}^{H} \hat{\mathbf{h}}_{R, D}\right|^{2}+\left\|\hat{\mathbf{h}}_{R, D}\right\|^{2}\left(c\left\|\mathbf{h}_{S, R}\right\|^{2}+1\right)}
\end{aligned}
$$
其中，$a=P_{S} P_{R} \alpha_{S, R} \alpha_{R, D}, b=P_{R} \alpha_{R, D}$, and $c=P_{S} \alpha_{S, R}$

# Reference
[1] X. Chen, L. Lei, H. Zhang and C. Yuen, "Large-Scale MIMO Relaying Techniques for Physical Layer Security: AF or DF?," in IEEE Transactions on Wireless Communications, vol. 14, no. 9, pp. 5135-5146, Sept. 2015, doi: 10.1109/TWC.2015.2433291.
[2] T. K. Y. Lo, "Maximum ratio transmission," 1999 IEEE International Conference on Communications (Cat. No. 99CH36311), 1999, pp. 1310-1314 vol.2, doi: 10.1109/ICC.1999.765552.