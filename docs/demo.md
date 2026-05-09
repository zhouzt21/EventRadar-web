Demo Abstract: EventRadar: Wide-Area Kilometric
UAV Detection via Propeller-Aware Sensing
Zhiting Zhou1∗, Wenhua Ding1∗, Julia Gersey2, Xinglin Yu3, Xingchen Liu4, Haoyang Wang1,Jingao Xu5, Pei Zhang2, Xinlei Chen1†
1Shenzhen International Graduate School, Tsinghua University, China 2University of Michigan, USA
3Harbin Institute of Technology, Shenzhen, China 4Sun Yat-sen University, China 5The University of Hong Kong, China


ACM/IEEE Sensys26 Demo Submission #10


Abstract—Small unmanned aerial vehicles (UAVs) pose growing safety and operational risks to civilian infrastructure like
airports. However, reliably detecting small, non-cooperative UAVs
at kilometer-scale stand-off ranges remains challenging, as conventional radar and optical systems lack either the resolution or the reach required for early warning. We propose EventRadar, an active event-based system that exploits propeller event footprints and synthetic aperture radar (SAR) inspired gimbal scanning to jointly achieve kilometric reach and wide-area coverage. Beyond long-range detection and early alerts, EventRadar further estimates propeller rotational speed (RPM) to infer physical attribute and flight intent intrinsically for threat assessment. Preliminary
real-world experiments show that at 300 m, near the range limit of conventional visual methods, EventRadar delivers about 60
times higher mAP than a representative RGB-based method, with propeller signatures remaining clearly more salient up to 1 km. Index Terms—event camera, event-based vision, counter-UAV, long-range detection, gimbal sensing.


讲述资料：

在城市民用空域中，小型旋翼无人机由于起降灵活、操作门槛低，已成为“黑飞”治理的主要对象。相比体积庞大且受场地限制的固定翼飞机，这类非合作目标在复杂环境下的威胁性更显著。传统主动式感知方法（雷达、无线电）易引发对城市设施的电磁干扰且难以小型化，需引入兼具无源隐蔽与便捷部署特性的新型感知系统。
而这样的无源反无感知系统必须尽可能满足三个核心需求：第一是早期预警（Early Warning），必须在目标靠近前就尽早发现它；第二是威胁评估（Threat Assessment），要能分辨出它是合规飞行的送货机还是怀有恶意的入侵者；第三是广域覆盖（Wide Coverage），在空域监测中不能留下监控盲区 。然而，想要在公里级别的超远距离下同时实现这三点，现有的感知手段往往会显得力不从心。  

从技术角度看，这涉及到系统设计的几个核心挑战：
- 空间特征的彻底退化： 当探测距离拉远到公里级，无人机在图像里可能缩减到只有 $$4 \times 4$$ 像素甚至更小 。这时候，视觉方法中所有依赖形状、颜色或纹理的算法都会大幅失效。  
- 信噪比与有效信息的权衡： 在这种极远距离下，目标反射或遮挡产生的亮度变化在传播过程中剧烈衰减。相比于微弱的目标信号，环境背景产生的“底噪”就显得极为庞大。如何在嘈杂的事件数据流中，精准提取出那微弱的目标信号，是极大的技术难点。  
- 窄视野与大动态的矛盾： 为了看得远，系统使用了长焦镜头，但这会导致视野变得极窄。在这种视角下，目标稍微一加速或云台抖一下，无人机就会瞬间飞出画面。  
EventRadar 系统的突破口在于，它重新审视了无人机的控制流后，发现螺旋桨不仅是飞行的动力源，更是连接内部指令与外部状态的“桥梁”：内部的飞行意图和物理属性决定了螺旋桨转速，而转速又决定了外部的飞行状态 。 通过牢牢抓住螺旋桨的特征，EventRadar提出了一种新的感知方法（propeller-aware sensing）。
- 远距离检测（响应早期预警需求）——“以时间换空间”： 既然空间形态特征会退化，那我们就去时间维度（频域）观察无人机特征。EventRadar 利用事件相机微秒级的分辨率，能捕捉螺旋桨旋转带来的极高频亮度变化 。即使目标在画面中只是一个点，它周期性微动依然能被捕捉。  
  - 压缩感知下的信号重建： 而在这个基础上，面对极低信噪比，我们引入了压缩感知的思想，在频域分析这种时间维度特征。因为螺旋桨周期信号在转速频率空间是稀疏的，所谓通过基匹配和稀疏重建，可以从杂乱的噪声中强行恢复出螺旋桨的物理频率。
    - （利用基匹配预设信号的“物理模板”，变被动去噪为主动特征排查，在随机噪声中精准提取微弱频率；从“瞬时”到“相干放大”，将时间段内的信号整体处理，利用周期信号的跨时间相干性实现信噪比“人工放大”，在空间特征退化殆尽时依然能精准锁定目标）
- 内部推理（响应威胁评估需求）——“螺旋桨转速作为桥梁”： 拿到了转速 $$\omega$$，我们就能利用物理模型 $$T_i \approx k_T \omega_i^2$$ （螺旋桨产生的升力和转速平方成正比）来反推无人机的载重和飞行指令，以完成远距离下可解释、高可靠的空域威胁评估任务。
- 广域跟踪（响应广域覆盖需求）——便携式云台+拖尾追踪： EventRadar系统在没有使用光学方法增强的情况下，加上长焦镜头会导致视野变小，为了实现广域覆盖，系统设计了二自由度的便携式云台。而云台为了实现小目标快速追踪，需要重点解决目标丢失问题；EventRadar则开发了事件拖尾追踪引导技术，利用事件相机的极低延迟特性，即使目标由于机动或扰动暂时飞离了画面中心，它也会在事件流中留下“拖尾”的残影 。通过分析这些残影的方向和长度，系统能精准计算出目标的逃逸速度，引导云台追回目标。  
实验证明，EventRadar 将反无人机的能力提升到了一个新的维度：
- 公里级的稳健探测： 实现了超过1公里（目前实验数据上限1500m）的稳定探测与追踪能力 。相比之下其他基线方法（Evpropnet, Effqpdet etc.)，出现检测框大小、位置极不稳定，甚至是直接无法检测出远距离目标的情况。
- 精度代差： 在 300m 测试中，其检测 mAP 约是传统帧相机基线的 60 倍（YOLOMG, 2025），是事件算法基线的 18 倍（Effqpdet, 2025）。  
- 可解释性： 载重估计的相对误差可以控制在 2% 以内，让我们可以通过转速这个“桥梁”，在远处就洞悉目标的真实负载情况。  
EventRadar 系统揭示了一种全新的感知思路：螺旋桨作为桥梁，将不可见的“内部意图”转化为了可观测的高频信号，而这种思路为远距离无人机检测定义了一种全新的范式：在极端环境下的视觉感知，关键并非仅在于捕获多少空间像素，还可以在时间维度（频率）捕捉螺旋桨的本质特征。

