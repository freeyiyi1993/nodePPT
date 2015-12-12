title: Webkit Render
speaker: 孙新杰
transition: vertical3d

[slide]

# Webkit内核渲染机制
## よろしく
<small>演讲者：孙新杰 <br> http://freeyiyi.com/</small>

[slide]
## 交互渲染bug

1. [ripple demo](http://127.0.0.1:3000/1.html)
2. [no ani demo](http://127.0.0.1:3000/3.html)

<!-- stackoverflow chromium bug -->

[slide]
## 现象分析
* <del>动画</del>
* `overflow:hidden;` 失效
* **webkit/blink** 内核的浏览器


[slide]
## webkit 届的 haslayout

** -webkit-transform: translateZ(0); **

* [Chrome absolute绝对定位display/visibility渲染bug](http://www.zhangxinxu.com/wordpress/2015/01/chrome-absolute-display-visibility-render-bug/)

* [利用重绘解决IE下JS交互产生的定位重叠等棘手bug](http://www.zhangxinxu.com/wordpress/2013/01/js-paint-ie6-relative-ie8-inline-block-bug-fix/)

[slide]

    ** 遇到一些交互渲染bug时候，不妨触发浏览器的重绘，说不定问题即解决 **

[slide]

## 质疑思考
* **-webkit-transform: translateZ(0);** 触发重绘?
* 和硬件加速有什么关系？
* 交互渲染bug的本质是什么？
* 类似的bug有哪些？怎么避免？
* **z-index: 0;**
* **transform**

<!-- 三种解决方案 z-index 层 transform -->
<!-- z-index -->
[slide]
## 浏览器渲染过程


![](http://images.cnitblog.com/blog/595796/201408/211743255813505.png)

[slide]
## 重绘？

* [重排、重绘、重布局、回流、重渲染](http://freeyiyi.com/css-repaint-relayout-reflow/)

* [demo](127.0.0.1:3000/1.html)

* 结论1

<!--
## 重绘

![](http://cdn2.infoqstatic.com/statics_s1_20151020-0055-2u1/resource/articles/javascript-high-performance-animation-and-page-rendering/zh/resources/0409011.jpg)
 -->

[slide]
## 硬件加速

[slide]
### CPU vs GPU

![](http://www.frontopen.com/wp-content/uploads/2013/10/20130912164559628-590x427.png)

[slide]
## 三种渲染方式
* 软件
* 硬件GPU
* 混合


[slide]
## 软件：CPU
- Pros: 缓存机制减小重绘开销，不需要GPU并行性
- Cons: 对于高级绘图能力不足、性能不好

[slide]
## 硬件：GPU
- Pros: 兼容、CSS3D、HTML5多媒体、WebGL [性能对比参考文](http://www.frontopen.com/1463.html)
- Cons: GPU内存资源相对紧张，网页分层使GPU内存使用相对较多

[slide]
## 主要区别：
* 更新区域
[demo](http://127.0.0.1:3000/1.html)
只绘制变动的部分 vs 更新的层

<!-- 软件渲染没有为每一层提供后端存储
他需要将和这个区域所有重叠部分的所有层次的相关区域从后向前重绘一遍
有交集的RenderObject
只绘制变动的部分

软件过程更新区域 是不连续的区域

软件渲染存储方式 各平台不一样 基本上都是位图 -->
[slide]
## 指导意义
网页结构、渲染策略


[slide]
## [GPU Accelerated Compositing in Chrome](http://www.chromium.org/developers/design-documents/gpu-accelerated-compositing-in-chrome)
**translate3d hack**

[slide]

![](http://images2015.cnblogs.com/blog/383794/201511/383794-20151104183340336-879945381.png)

[slide]

![](http://images2015.cnblogs.com/blog/383794/201511/383794-20151104183415024-259428031.png)


[slide]
## 交互渲染bug的本质是什么？
* 渲染路径不同

![](http://images2015.cnblogs.com/blog/383794/201511/383794-20151104184529727-351034581.png)



[slide]
## 类似的bug
[bug68196](https://bugs.webkit.org/show_bug.cgi?id=68196)


[slide]
## 滥用的灾难

[z-index demo](http://fouber.github.io/test/layer/)

![](http://images2015.cnblogs.com/blog/383794/201511/383794-20151102184844289-1187063133.png)



[slide]
## 如何避免

...

[slide]
## z-index
* [文档](http://devdocs.io/css/z-index)
    For a positioned box, the z-index property specifies:

    - Whether the box establishes a **local stacking context**.
    - The **stack level** of the box in the current stacking context.

* value: auto | integer

[slide]
## stacking context

* HTML
* position: fixed
* positioned !static with a z-index !auto
* flex with a z-index !auto
* opacity < 1
* transform !none,
* mix-blend-mode !normal
* filter !none
* isolation: isolate
* -webkit-overflow-scrolling: touch;
* ...
* [文档](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Positioning/Understanding_z_index/The_stacking_context)

[slide]
## z-index 相对于当前 stack context 才有意义

![](http://images2015.cnblogs.com/blog/383794/201511/383794-20151102182749274-1957961770.png)

[slide]

谢谢大家