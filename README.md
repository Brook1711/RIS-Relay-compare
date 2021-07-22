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

# Reference
[1] X. Chen, L. Lei, H. Zhang and C. Yuen, "Large-Scale MIMO Relaying Techniques for Physical Layer Security: AF or DF?," in IEEE Transactions on Wireless Communications, vol. 14, no. 9, pp. 5135-5146, Sept. 2015, doi: 10.1109/TWC.2015.2433291.
