title: Code For Future
speaker: 孙新杰
transition: vertical3d

[slide]

# Code For Future 会后分享
## どうぞよろしくお願いします
<small>演讲者：孙新杰 <br> http://freeyiyi.com/</small>


[slide]
## 我的角色和目的

<!-- 学习目的: 汇报自己的吸收、其次作为，信息的传递着者、尽可能把讲师的精髓再现，实在不行，吸引你们去看第一手资料也行，如果吸引不成[作为一个过渡吧，想要继续深入了解的可以看原版文字、slide、视频、跟作者直接交流
暂时没时间和精力去了解的，也希望能通过我的介绍有个初步印象，未来用到的时候能想到 或者有些想法
]，也许未来有一天能用得到 想起这些东西 给大家一些启发
 -->

[slide]
## 分享内容
* web组件化的权衡-民工精髓
* react生态圈-郭达峰

<!-- 由三个同学每个同学选择2个主题，尽量把精彩的内容呈现给大家 -->

[slide]
## 如何分享
* 讲师介绍
* 演讲风格分析
* 讲师内容精髓再现
* 总结


[slide]
## web组件化的权衡

[slide]
## 讲师介绍

[@民工精髓V](http://weibo.com/u/1858846672?topnav=1&wvr=6&topsug=1#_rnd1445822892608)

* 标签：
    - 有货
    - 高产
    - 神棍

* [前端读书资助计划](http://weibo.com/p/1001603702250437536879)

[slide]
## 演讲风格
* 演绎式(有货)[ vs 大漠]
* 真诚、谦和
* <span style="color:red; font-size: 150%">萌</span>的不要不要的
* 准备十分充分[文字版]


[slide]
## 内容提炼


[slide]

## 组件化

* 狭义 vs 广义

<!-- 基于数据逻辑层的业务，对不同层级进行不同能力的封装 -->

* 组件化优势-生产力
    * 原始的组件，可单独开发测试
    * 组件逐级拼装成更复杂的组件，直到整个应用
    * 每一级都是易装配，可追踪，可管控的

* 组件化目标
    * 复用 vs 分治

* 做到什么程度
    * 资产 vs 耗材
    * 全组件 vs 半组件化

[slide]
## 组件化框架

* Angular，Vue，Aurelia
* React
* Polymer

[slide]
## 现状
如果要跟其他异构系统作集成，基本上都不可能优雅

[slide]
## 未来

大家将来到底能不能在一起

业务数据层会是一个基本没有框架差异的东西

可利用的东西逐渐向标准聚拢

独特 新颖 融合 趋同 未来是美好的


[slide]
## 组件化的实践-技术选型
* 组件粒度 vs 组件的层级(3~5)
* 组件之间通讯(props mixin dispather)
    - 数据模型
    - 组件间

[slide]
## 业务架构
* 框架选择、技术不是重点
* 数据流转关系决定业务架构，数据通讯层的设计是个大课题
* 理解透彻 才能搞出些有意思的新东西

[slide]

## Meteor Relay GraphQL


[slide]
## 其他思考
* 可视化继承: 继承 vs 组合
* [插件化平台](http://codepen.io/xufei/pen/zvPNzN?editors=100)


[slide]
## 总结
* 农业社会到工业社会的飞跃
* 各类客户端开发
* [文字版](https://github.com/xufei/blog/issues/22) | [幻灯片](http://xufei.github.io/slides/2015/components%20and%20templates.html#0) | [视频](http://v.youku.com/v_show/id_XMTM2NjY0NjQwNA==.html)

[slide]
## react生态圈

[slide]
## 讲师介绍
@[郭达峰](http://weibo.com/u/1798620665?topnav=1&wvr=6&topsug=1)

* Strikingly <span style="color:red; fontsize:150%;">[Niubility]</span>CTO
* 14年老coder
* Qcon讲师

[slide]
## 演讲风格
* 中英混杂
* 高大上 \*听不懂\*

[slide]
## 内容提炼


[slide]
## react为什么那么火？

[slide]
![](/assets/img/all.png)

<!-- 他到底好在哪里？为什么前端新事物层出不穷？前端圈很能折腾嘛？ -->

[slide]
## 前端面临前所未有的工程化挑战！
<!-- 赞探索一种最适合的方式去解决我们遇到的各种痛点 -->

[slide]
## 一分钟讲懂React
* 多目标平台：一次学，到处写
    - 各种终端、\*\*canvas、\*\*art、\*\*three
* React(V 状态机)：更新DOM、响应事件
    - 记录用户在页面上积累的状态-上帝模式
* Virtual DOM：
    - diff，按需更新
    - 最小化重绘
    - 同构渲染
* 组件化：语义标签

<!-- w3c多年的努力 react一下子就糅合在一起了 -->

[slide]
## 数据处理
* Flux
* GraphQL + Relay
* Falcor + JSONGraph
* Om Next
* Meteor

[slide]
## 其他
* ES6
* Immutable.js
* Flow
* CSS in JavaScript

[slide]
## 总结
* 粒度不同 选择问题 没有完美的方案
* 混乱状态
* [PDF](http://7sbxk2.com2.z0.glb.qiniucdn.com/pdf-react-ecosystem.pdf#rd) | [视频](http://v.youku.com/v_show/id_XMTM2NjM0Nzk2MA==.html)

<!-- 描述想要的数据 后端按需返回，后端的支持：得事先定义好所有可能的数据 -->

<!--
## REST API 的问题
* 多请求
    * /api/v1/site/{id}
    * /api/v1/site/{id}/analytics
* 过度交付
* 客户端版本

 -->
<!--
## CSS七宗罪
CSS in JavaScript
1. Global namespace - solved
2. Dependencies - solved
3. Dead code elimination - solved
4. Minification - solved
5. Sharing constants - solved
6. Non-deterministic resolution - solved
7. Isolation - solved
 -->
<!-- 粒度 -->
<!-- 对数据有更⼩粒度控制的需求 -->

[slide]
## 10年内，我们未必不如他们

与君共勉