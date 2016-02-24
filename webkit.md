title: Webkit 内核最佳实践
speaker: 孙新杰
date: 2016-02-23

[slide]

# Webkit内核最佳实践
## よろしく
<small>演讲者：孙新杰 <br> http://freeyiyi.com/</small>


[slide]
# 浏览器


前端工程师们的KDE(桌面环境)、IDE(集成开发环境)

<!-- 当然这是不慎严谨的说法 -->

<!-- 浏览器是我们使用最多的软件，大家应该都有这样的体验：一台新电脑到手，首先得安装一个顺手的浏览器 -->

<!-- 学习浏览器的内部工作原理将有助于我们理解最佳开发实践的缘由，能让我们更迅速地作出更明智的决策 -->

<!-- 浏览器由哪些部分组成呢 -->

[slide]
![browser](/assets/img/webkit/brower.png)

<!-- * 用户界面 - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。 -->

<!-- * 浏览器引擎 - 在用户界面和渲染引擎之间传送指令。 -->

<!-- * 渲染引擎 - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。 -->

<!-- * 网络 - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。 -->

<!-- * 用户界面后端 - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。 -->

<!-- * JavaScript 解释器。用于解析和执行 JavaScript 代码。 -->

<!-- * 数据存储。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。 -->

<!-- 这里我们只看 渲染引擎 部分的webkit渲染引擎 -->

<!-- 在此之前 先明确几个概念 -->

[slide]

## 理解 Webkit

![](/assets/img/webkit/webkitBlink.png)

<!-- 渲染引擎(排版引擎)、开源项目的区别 -->

<!-- WebKit 前身是 KDE 小组的 KHTML，WebKit 所包含的 WebCore 排版引擎和 JSCore 引擎来自于 KDE 的 KHTML 和 KJS，Apple公司将其拿出来作为 Safari 的渲染引擎，并作为一个开源项目维护，他为各平台的的移植提供 Web 接口，这些接口提供操作和显示网页的能力，也就是WebView或者类似WebView。 -->

<!-- WebKit2相对于WebKit，不是简单的第二个版本，是一个新的API层，主要贡献者是Apple。 -->

<!-- Chromium是一个建立在WebKit之上的浏览器开源项目（webkit的分支），由Google发起的。 -->

<!-- 该项目被创建以来发展迅速，很多先进的技术被采用，如跨进程模型，沙箱模型等等。同时，很多新的规范被支持，例如WebGL，Canvas2D，CSS3以及其他很多的HTML5特性，基本上每天你都可以看到它的变化，它的版本升级很快。在性能方面，其也备受称赞，包括快速启动，网页加载迅速等。 -->

<!-- Blink fork 自 Webkit，Google 剔除了其中与chromium无关的部分，将代码结构重新整理，就目前而言，Blink的渲染和WebKit是一样，但是，以后两者将各自走不同的路。这有点类似于之前WebKit从KHTML中复制出来一样，历史总是惊人的相似。 -->

<!-- 一个由多个巨头组成的联合体，无法满足Chrome的要求，有必要另立门户 自己制定标准了。 -->

<!-- Chrome是Google公司的浏览器产品，它基于chromium开源项目，他还有一个先行版canary 。 -->

<!-- 如果你了解gitflow 你可以简单的把chrome理解成master分支 canary是测试环境上的master分支 chromium是develop分支 -->

<!-- 除了帮助大家明确概念，有一点要强调的是：我们所使用的排版引擎变动十分频繁，几小时更新一个版本，比如：有些特性在今天比别的特性慢，但过不了多久可能就会反过来。我们今天讨论的内容， 没法保证对于未来还会适用。它们随着时间的变化而不同，关键在于如何解读你看到的数据，并以此来调整优化的手段。所以对一些特性我们不能死机硬背，理解很重要 -->

<!-- 了解了webkit是什么 我们来详细看一下他的渲染 流程 -->

[slide]

![webkitflow](/assets/img/webkit/webkitflow.png)

<!-- 大家对这个流程应该都已经很熟悉了，这里在 chrome devtools 的 timeline 来看这个过程是怎么映射上去的，有哪些渲染细节是没有暴露给开发者的 -->

[render flow on timeline](/assets/img/webkit/render.html)

[slide]
![render flow on timeline](/assets/img/webkit/renderflow.png)

<!-- 为了方便查看，我提前截图标注了

第一阶段：
1. parse HTML -> DOM Tree.

第二阶段：
2. parse CSS -> STYLE Object. 加上 DOM, 生成 Render Tree
    1. layout 根据 css 中位置等属性进行 layout.
    1. update layer tree 根据布局信息，更新layer tree.

第三阶段：
    1. paint 根据renderObject保存的绘制信息绘制.
    1. composite layers
    1. 生成最终的 Render Tree. Render Object 包含尺寸和位置.

这是一个渐进的过程。为达到更好的用户体验，呈现引擎会力求尽快将内容显示在屏幕上。它不必等到整个 HTML 文档解析完毕之后，就会开始构建呈现树和设置布局。在不断接收和处理来自网络的其余内容的同时，呈现引擎会将部分内容解析并显示出来。
解析的过程可以分成两个子过程：词法分析和语法分析。至于解析的详细过程和知识点 这个我们就不必了解了。

解析结束后,开始解析那些处于“deferred”模式的脚本，也就是那些应在文档解析完成后才执行的脚本。然后触发 DOMContentLoaded事件，等图片等外部资源加载后，load 事件将随之触发。

预解析

Webkit 和 Firefox 都进行了这项优化。在执行脚本时，其他线程会解析文档的其余部分，找出并加载需要通过网络加载的其他资源。通过这种方式，资源可以在并行连接上加载，从而提高总体速度。请注意，预解析器不会修改DOM树，而是将这项工作交由主解析器处理；

预解析器只会解析外部资源（例如外部脚本、样式表和图片）的引用。 -->

<!-- timeline的layer tree 和 composite layers 是什么？ 其实中间还有这几个步骤 -->

[slide]

## 我们不知道的渲染细节

![DOM Tree VS Render Tree VS RenderLayer Tree VS GraphicLayer Tree](/assets/img/webkit/GraphicsLayer.png)


[slide]

DOM Tree VS RenderTree

![](/assets/img/webkit/presentdomtree.png)

<!-- RenderObject对象在DOM树创建的同时也会被创建，当然，如果DOM中有动态加入元素时，也可能会相应地创建RenderObject对象。 -->

<!-- Render Object是一个基类 Render Viewport Render Scroll Render Block RenderText是他的子类 他们共同构成Render Tree -->

<!-- head、 meta等不可见标签，display: none 的元素 不会出现在 render tree 里， 而 visibility 属性值为 hidden 会在 render tree 里 -->


[slide]

<!-- Render Tree 对 DOM tree 不只是一对一或者一对多 还可能一对零，因为根据 CSS 规范，inline 元素只能包含 block 元素或 inline 元素中的一种。如果出现了混合内容，则应创建匿名的 block 呈现器，以包裹 inline 元素。 -->

## Anonymous RenderObjects

![](/assets/img/webkit/AnonymousRenderObjects.png)

[slide]

## Render tree 的判定

* DOM树的document节点；
* DOM树中的可视化节点，例如HTML，BODY，DIV等，非视可化节点不会建立Render树节点，例如HEAD，META，SCRIPT等；
* 某些情况下需要建立匿名的Render节点，该节点不对应于DOM树中的任何节点；


<!-- Render Tree 是排版引擎(webkit)的输入. 包含渲染信息的矩形，通常对应于相关节点的 CSS 框 -->

<!-- 渲染引擎不直接对 Render Tree 处理, 而是再生成 Layer 树(根据限定条件), 节点为 Render Layer. -->

[slide]

DOM Tree VS Render Tree VS RenderLayer Tree

![dom tree vs render tree vs render layer tree](/assets/img/webkit/renderlayer.png)

<!-- 渲染引擎处理 Layer. ( Layer 树决定层次顺序, Render Object 决定 Layer 内容) -->


[slide]

## RenderLayer 判定

<!--
It's the root object for the page
It has explicit CSS position properties (relative, absolute or a transform)
It is transparent
Has overflow, an alpha mask or reflection
Has a CSS filter
Corresponds to <canvas> element that has a 3D (WebGL) context or an accelerated 2D context
Corresponds to a <video> element -->


* 根元素
* 有明确定位属性(position 不为static或者transform)
* 有透明效果
* 有 overflow, alpha mask 或者reflection效果
* 有 CSS filter
* Canvas 2D和3D (WebGL)
* Video节点


<!-- 大家可这么理解 RenderObject tree是零件 RenderLayer是聚合体 透明部分需要和其他部分区分开 那就建立一个新层吧 -->

<!-- RenderLayer 会决定z-index的层次关系 -->

<!-- Render Layer 根据需要创建对应的 GrapficsLayer. -->


[slide]

## DOM Tree VS Render Tree VS RenderLayer Tree VS GraphicLayer Tree

![DOM Tree VS Render Tree VS RenderLayer Tree VS GraphicLayer Tree](/assets/img/webkit/GraphicsLayer.png)

<!-- 渲染的四个树到齐了 -->

[slide]

## GraphicsLayer 判定

<!--
* Layer has 3D or perspective transform CSS properties
* Layer is used by <video> element using accelerated video decoding
* Layer is used by a <canvas> element with a 3D context or accelerated 2D context
* Layer is used for a composited plugin
* Layer uses a CSS animation for its opacity or uses an animated webkit transform
* Layer uses accelerated CSS filters
* Layer has a descendant that is a compositing layer
* Layer has a sibling with a lower z-index which has a compositing layer (in other words the layer overlaps a composited layer and should be rendered on top of it)
 -->
* 有3D属性(perspective、transform)的层
* video标签并使用加速视频解码的层
* canvas元素并启用3D的层
* 插件，比如flash
* CSS动画(animation、opacity、transform)
* 有 CSS filter
* 有一个后代元素是独立的layer(clip、reflection)
* 兄弟元素的z-index值比较小并且是一个层，那么该元素必须作为复合层来渲染

<!-- 渲染引擎将每个GraphicsLayer栅格化，并独立的绘制进位图中，并将这些位图作为纹理上传至GPU，复合多个层来生成最终的屏幕图像，即只有GranphicsLayer才是用走GPU渲染路径的 -->

<!-- 而 GraphicsLayer 会存在 cache, 当层 Render Object 不变时, 不用重新绘制, 只需要 GPU 重新 composite 即可.
并且当 Render Object 发生变化时, 只需要更新它所属的单个 GraphicsLayer 即可. -->

<!-- GraphicsLayer走GPU渲染路径的好处就是虽然元素依然在文档流中，但是对他进行transform变形、位移、缩放不会引起大面积重排，但是如果层的内容发生改变，那就需要重绘了。-->

<!-- 纹理：可以把它想象成一个从主存储器(例如 RAM)移动到图像存储器(例如 GPU 中的 VRAM)的位图图像(bitmap image)。一旦它被移动到 GPU 中，你可以将它匹配成一个网格几何体(mesh geometry) —— 在视频游戏或 CAD 程序中，该技术通常是为骨骼的 3D 模型(skeletal 3D models)赋予“皮肤(skin)”。Chrome 使用纹理来从 GPU 上获得大块的页面内容。通过将纹理应用到一个非常简单的矩形网格就能很容易匹配不同的位置(position)和变形(transformation)。这也就是 3D CSS 的工作原理，它对于快速滚动也十分有效。 -->

[slide]

## 总结

渲染过程中的四个平行的树结构：

* DOM tree
* RenderObject tree
* RenderLayer tree
* GraphicsLayer tree

<!--
The DOM tree, which is our fundamental retained model
The RenderObject tree, which has a 1:1 mapping to the DOM tree’s visible nodes. RenderObjects know how to paint their corresponding DOM nodes.
The RenderLayer tree, made up of RenderLayers that map to a RenderObject on the RenderObject tree. The mapping is many-to-one, as each RenderObject is either associated with its own RenderLayer or the RenderLayer of its first ancestor that has one. The RenderLayer tree preserves z-ordering amongst layers.
The GraphicsLayer tree, mapping GraphicsLayers one-to-many RenderLayers -->

<!-- ![](/assets/img/webkit/GraphicsLayer.png) -->

<!-- 说了RenderLayer 会决定z-index的层次关系，那么我们深入了解下层叠上下文 -->


[slide]

![z-index-order](/assets/img/z-index/zindex-order.png)

<!-- 这张大家应该都不陌生，图说明了 `z-index 不仅是在比大小 还的看上下文` -->

<!-- 那么在层叠上下文中，各种布局(float inline block positioned)又是如何排序的呢？ -->

[slide]

在层叠上下文中(注意：非格式化上下文)，浏览器会根据以下规则来渲染绘制每个在同一个层叠上下文中的盒模型（从先绘制到后绘制）：

* 背景和边框：建立层叠上下文元素的背景和边框。层叠中的最低级
* 负 z-index：z-index 为负的后代元素建立的层叠上下文
* 块级盒：文档流内非行内级非定位后代元素
* 浮动盒：非定位浮动元素
* 行内盒：文档流内行内级非定位后代元素
* z-index: 0：定位元素。这些元素建立了新层叠上下文
* 正 z-index：（z-index 为正的）定位元素。层叠的最高等级

[slide]

[层叠上下文的顺序](/assets/img/z-index/stack-context.html)

[slide]

## 层叠上下文的判定
* 根元素（HTML）
* (position 不为 static && z-index 值不为 auto) || position: fixed
* 一个伸缩项目 Flex Item && z-index 值不为 auto，即父元素 display: flex|inline-flex
* opacity < 1
* transform 非 none
* mix-blend-mode 不为 normal
* filter 不为 normal
* isolation: isolate
* will-change 中指定了上述任意属性，即便你没有直接定义这些属性
* -webkit-overflow-scrolling: touch

[slide]

[stacking-order-test](/assets/img/z-index/stack-context2.html)

<!--
原理：

* `同级情况下，按源代码中的顺序，后来居上`
* 新属性创建的堆叠上下文，相当于`z-index: 0 && positioned`(文档只提到[opacity](https://www.w3.org/TR/css3-color/#transparency))
 -->

[slide]

## 包含块

<!-- 视觉格式化模型中决定元素位置的重要概念,包含块的概念很重要，因为可视化格式模型中很多的理论性知识都跟这个概念有关系，比如，宽度高度自动值的计算，浮动元素的定位，绝对定位元素的定位等等。
 -->


[slide]

<!-- 包含块的判定 -->

![CB](/assets/img/z-index/CB.png)

<!-- 太抽象？来个具体的 -->

[slide]

![](/assets/img/z-index/008.png)


[slide]

<!-- 应用 -->

## transform 创建一个层叠上下文和包含块

[transform-contain-block-test](/assets/img/z-index/contain-block.html)

[slide]
<!-- 再看之前的demo -->

![oveflow:hidden失效](/assets/img/webkit/layer.html)

[slide]

谢谢大家