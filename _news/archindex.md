---
layout: distill
title: Make ArchIndex public at CUPUM 2021
date: 2021-06-13 16:11:00-0400

authors:
  - name: Yichen Mo
    url: https://amomorning.com
    affiliations:
      name: Southeast University
  - name: Baizhou Zhang
  - name: Biao Li

toc:
  - name: 引言
  - name: 方法
  - name: 技术细节
  - name: 应用展示
  
_styles: >
  .fake-img {
    background: #bbb;
    border: 1px solid rgba(0, 0, 0, 0.1);
    box-shadow: 0 0px 4px rgba(0, 0, 0, 0.1);
    margin-bottom: 12px;
  }
  .fake-img p {
    font-family: monospace;
    color: white;
    text-align: left;
    margin: 12px 0;
    text-align: center;
    font-size: 16px;
  }
inline: false
---

1989年，第一届计算机在城市规划和城市管理中应用国际会议（CUPUM）在香港举行，随后两年举办一次，均由世界著名的高校承办。20年来，它一直是学术界交流计算机思想与应用的主要国际会议之一，交流思想与计算机技术的应用，以解决影响城市规划与发展的一系列社会、管理及环境问题。  
{% include figure.html path="/assets/img/2021-06-14-21-38-27.png" class="img-fluid rounded z-depth-1" zoomable=true%}
2021年6月9日-11日，第 17届 CUPUM 2021 会议在芬兰赫尔辛基由阿尔托大学举办，采用线上会议的模式，主题为 “TOGETHER: 
Urban Informatics for Future Cities”，集中讨论协作、多学科和包容性的城市转向，并强调对未来城市的共同创造。

***

> 论文发表内容：  
ArchIndex: A Web-based and Data-driven Retrieval System for City Blocks  
ArchIndex：数据驱动的城市街区检索系统


### 引言
随着城市在物理层面上的城市化、抽象层面上的数字化与运算化，城市街区的表述从人为特征提取，转向基于城市数据的表示学习。从现有的城市数据可提取城市特征构建数据库，通过城市街区样本的相似度检索，以实现城市肌理的认知、发现与利用。目前为止，在城市研究上，研究者倾向通过主观判断提取城市特征，这样的方法不具有普适性，且需要花费大量时间认识城市。

本研究提出了一个数据驱动的城市街区案例库的检索框架，该框架从开源矢量地图数据自动构建数据库，以数据库为基础生成给定城市或城市群的带标签数据样本。数据驱动的方法从样本学习每个街区地块的形态、活力、功能特点，生成高维特征向量，用于城市样本的匹配。

以奥地利维也纳为例，该研究成果以网页形式公开。任意用户可在网页端使用，交互地获得对城市的整体印象，理解城市肌理。对基于给定地块的输入设计，使用者可快速定位相似街区在城市中地理位置分布，认识形态特征，有益于延续城市历史肌理。整个科研成果在10天内完成，这说明在编程工具日益成熟的今天，设计师与科研人员可快速在网页上呈现其运算设计成果。
  
{% include figure.html path="/assets/img/archindex.png" class="img-fluid rounded" %}
<div class="caption">
    图1. ArchIndex 网页设计 （https://index.archialgo.com）
</div>

### 方法
ArchIndex 从形态、活力、功能三个方面自动提取城市街区的表述特征。数据收集基于对 OpenStreetMap 的矢量地图数据进行拓扑结构的分析，并通过地块面积、建筑密度等基础几何计算，提取出 6000 余有效街区。对每个街区分析，并自动提取特征之后，相应的结果存入数据库中，形成建成环境与既有方案的记忆库。
- 形态分析：从数据库生成每个街区的组团形态与肌理，得到 500x500 像素的图像数据集，使用预训练模型提取高维特征向量。
- 活力分析：从城市街区包围矩形内GPS轨迹数据分析该区域的活动情况，用采样点简单表示街区活力值。
- 功能分析：将POI数据标签预定义为13种主要功能类别，统计其 5 分钟步行圈（400m）范围内功能点的分布情况。

{% include figure.html path="/assets/img/2021-06-20-22-45-55.png"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图2. 分别使用 Inception V3, Capsule Net, Autoencoder 三种模型进行特征提取，以相似街区检索任务测试适合网络应用的神经网络模型。
</div>
{% include figure.html path="/assets/img/1998-20m.png"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图3. 城市街区包围矩形范围内GPS轨迹数据，间隔20m采样
</div>

{% include figure.html path="/assets/img/6067.png"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图4. 13 种主要功能类别与 5 分钟步行圈（400m）内功能点分布。
</div>


### 技术细节
ArchIndex是分布式的云端应用。用户交互部分以网页应用呈现，可在任意电脑上使用Chrome内核的浏览器操作。以node上的WebSocket服务分别连接Java端的数据服务与Python的神经网络运算。名为`ArchiJSON`的数据规范将这多个部分连接起来，进行参数、检索结果、几何图元的传递。


{% include figure.html path="/assets/img/2021-06-20-22-44-14.png"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图5. 应用程序框架（简化）
</div>

### 结果与讨论
基于数据的方法对抽象
城市中
基于数据的方法，
图6、图7分别展示了两个 ArchIndex 在传统形态学任务上的结果。

本研究使用开源数据提供了一个研发框架，该方法可对任意城市快速构建云端应用程序。通过与机构合作，更好的数据亦能助益于生成更好的检索结果。在后面的研究中，ArchIndex 为数值抽象概念的表示提供了研究平台，可进一步提取形态的空间关系与基于轨迹图结构的活力分析等。
{% include figure.html path="/assets/img/result1.jpg"   class="img-fluid rounded" zoomable=true%}
{% include figure.html path="/assets/img/result2.jpg"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图6. 通过调整参数系数，可以比较街区形态表述中手工特征提取与自动特征提取的方法。
</div>
{% include figure.html path="/assets/img/result3.jpg"   class="img-fluid rounded" zoomable=true%}
{% include figure.html path="/assets/img/result4.jpg"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图7. 使用 ArchIndex 探索式地发现城市原型
</div>

### 应用展示
{% include figure.html path="/assets/img/interactive.gif"   class="img-fluid rounded" zoomable=true%}
<div class="caption">
    图8. ArchIndex 在网页端提供简单的场地地块编辑、功能区划分、调整检索系数等功能。
</div>
{% include figure.html path="/assets/img/archindex900.gif"   class="img-fluid rounded z-depth-1" zoomable=true %}
<div class="caption">
    图9. ArchIndex 可快速返回检索结果，并定位其在城市中的位置。
</div>




应用：https://index.archialgo.com

源码：https://github.com/Inst-AAA/archindex


