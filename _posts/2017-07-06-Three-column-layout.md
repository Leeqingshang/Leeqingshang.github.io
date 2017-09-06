---
layout: post
title: 圣杯布局/双飞翼布局
category: blog
description: 圣杯布局/双飞翼布局
---

这属于老生常谈的题目了。但还是要知道其所以然。

**名字的由来**

圣杯布局最早是国外传出来的，最早发布是2006年[见这里](https://alistapart.com/article/holygrail)。圣杯布局这个名字是直译过来的，但在西方，圣杯又被用来隐喻“渴求而不可得之物”。放到布局相关问题，是开发人员针对三列布局想有一种完美的布局方法，但完美本身就是一种不可得的东西，所以圣杯布局其实叫“最终布局”或"完美布局"更贴切。

双飞翼布局据闻是淘宝UED提出来的，国人创作大家都能理解，双飞翼这个名字也比较形象。

**作用**

这两种布局方式都是用来解决三列布局，其中两边定宽，中间自适应的布局问题的。当然，不光只是这一种，就作用来说，还有许多变种。

**实现**

先是三列

    <div class="container">
       <div class="main"> main </div>
       <div class="left"> left </div>
       <div class="right"> right </div>
    </div>
    
然后是CSS，先全部浮动，设置左右定宽，高度可自定，我们这里设置同高。主体部分100%自适应

    .main,.left,.right{float:left;height:800px}
    .left,.right{width:200px}
    .main{width:100%;}
    
为了区分，我们给三个部分加上背景色

    .main,.left,.right{float:left:height:800px}
    .left,.right{width:200px;background:blue;}
    .main{width:100%;background:red;}
    
然后这个时候因为主体部分100%，撑满，所以我们需要利用负边距把两个侧边拉上来

    .main,.left,.right{float:left:height:800px}
    .left,.right{width:200px;background:blue;}
    .left{margin-left:-100%;}
    .right{margin-left:-200px;}
    .main{width:100%;background:red;}
    
这个时候三列布局大概有个样子了，但中间主体部分的内容会被挡住，所以这个时候我们就需要给外部容器设置padding来解决这个问题

    .container{padding:0 200px;} 
    .main,.left,.right{float:left:height:800px}
    .left,.right{width:200px;background:blue;}
    .left{margin-left:-100%;}
    .right{margin-left:-200px;}
    .main{width:100%;background:red;}
    
还是挡住的，而且两边空白了，这个时候我们再通过t调整各自的相对定位来解决

    .container{padding:0 200px;} 
    .main,.left,.right{float:left:height:800px;position:relative;}
    .left,.right{width:200px;background:blue;}
    .left{margin-left:-100%;left:-200px;}
    .right{margin-left:-200px;right:-200px;}
    .main{width:100%;background:red;}
    
好了，这样基本的圣杯布局就算完成了，实际应用的话还可以给容器加上最小宽度什么的。

至于双飞燕布局，区别在于在*main*里面加上一个子div，然后通过设置这个div的外边距来解决两边遮挡问题，而不需要用定位。

[一个案例](http://lijiazhen888.com/2016ifepage/first-stage/task3/index.html)


