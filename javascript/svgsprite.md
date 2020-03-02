svg制作小图标
============
什么是svg
svg(scalable vector graphics)可缩略矢量图形。视网膜屏下分辨率解决方案之一。比如在iphone设备下devicePixelRatio大于1的情况下，防止图片模糊就可以使用svg的图片。svg是基于xml的标记语言。编辑起来类似于html构建页面一样。学习曲线平滑。实际应用于背景图片，小图标的场景较多。

如果获取svg
既然svg常作为图标使用，那么如何复用现有的轮子。还是在https://www.iconfont.cn上下载的时候选择“svg下载即可”。
如何使用svg
直接当作图片使用
和使用png，jpg图片相同的方式

<img src="paiqiu.svg">
<div style="background: url(paiqiu.svg); width: 100px;height: 100px;"></div>
如果我们发现下载的svg图片过大或者过小怎么办？ 和修改图片没什么两样，直接编辑svg图片
<?xml version="1.0" standalone="no" ?>
<!DOCTYPE svg PUBLIC "-//W3C//DTD SVG 1.1//EN" "http://www.w3.org/Graphics/SVG/1.1/DTD/svg11.dtd">
<svg t="1550061994320" class="icon" style="" viewBox="0 0 1024 1024" version="1.1"
xmlns="http://www.w3.org/2000/svg" p-id="1292" xmlns:xlink="http://www.w3.org/1999/xlink"
width="100" height="100">
    <defs>
        <style type="text/css">
        </style>
    </defs>
    <path d="M836.077037 115.579259a513.327407 513.327407 0 0 0-94.814815-61.345185 512 512 0 0 1-632.414815 773.404445 512 512 0 1 0 727.22963-712.05926z"
    fill="#BDDDFA" p-id="1293"></path>
    <path d="M512 75.851852A436.148148 436.148148 0 1 1 75.851852 512 436.622222 436.622222 0 0 1 512 75.851852m0-75.851852a512 512 0 1 0 512 512A512 512 0 0 0 512 0z"
    fill="#0788FF" p-id="1294"></path>
    <path d="M273.825185 420.598519l-20.48-19.816297c181.854815-187.828148 425.528889-205.368889 724.290371-51.579259l-12.98963 25.315556c-286.340741-147.437037-518.731852-131.792593-690.820741 46.08z"
    fill="#0788FF" p-id="1295"></path>
    <path d="M264.722963 426.192593c-106.761481-54.139259-134.637037-144.782222-81.540741-269.368889l26.168889 11.188148c-46.838519 109.89037-24.557037 186.026667 68.171852 232.865185z"
    fill="#0788FF" p-id="1296"></path>
    <path d="M228.124444 890.69037l-18.962963-21.428148c116.717037-101.451852 133.499259-249.552593 51.2-452.93037l26.737778-10.24c86.186667 212.48 66.37037 375.751111-58.974815 484.598518zM567.277037 992.616296l-23.04-16.687407c169.623704-234.097778 179.579259-470.471111 29.487407-702.577778l23.893334-15.454815c157.108148 243.01037 146.868148 490.097778-30.340741 734.72z"
    fill="#0788FF" p-id="1297"></path>
    <path d="M318.388148 672.995556C111.976296 630.044444 22.281481 526.601481 51.863704 365.605926l28.444444 5.12c-26.642963 144.971852 52.906667 234.477037 243.863704 274.394074zM879.597037 849.825185l-27.306667-8.059259c67.602963-228.219259 53.94963-407.703704-40.675555-533.238519l22.755555-17.066666c100.408889 133.12 115.579259 321.042963 45.226667 558.364444zM192.284444 318.767407l-24.841481-13.179259C250.69037 146.868148 413.961481 66.37037 653.463704 66.37037v28.444445c-228.124444 0-383.241481 75.377778-461.17926 223.952592zM273.066667 834.74963c-69.214815-11.946667-125.914074-37.925926-168.675556-76.894815l18.962963-21.048889c38.684444 35.271111 90.642963 58.785185 154.358519 69.878518z"
    fill="#0788FF" p-id="1298"></path>
</svg>
内容有点多，但是很眼熟。不要被吓到，直接看着width height操作起来。其余参数后续会介绍。但是初步使用，改改宽高就够了。

svg sprite
-----------
直接使用感觉并没有多高级，甚至一个高像素图片能解决的事情，非要用svg感觉不爽。 那么svg sprite是你需要的。类似于css spirit 将图标整合到一起，实际呈现特定图标即可。
symbol
从刚刚的例子中我们看到，svg标签类似于html标签，是一个大舞台，想画什么在里面自成一派的绘制即可。symbo（翻译为元件），symbo就是一个个组装好的原价，一个元件就是一个图标。 如下就是一个舞台上的三个剧目
<svg>
    <symbol>
        <!-- 第1个图标路径形状之类代码 -->
    </symbol>
    <symbol>
        <!-- 第2个图标路径形状之类代码 -->
    </symbol>
    <symbol>
        <!-- 第3个图标路径形状之类代码 -->
    </symbol>
</svg>
use
好的剧目如果不播出来看看是不知道效果的。单独把上面的内容放到html中，你只能得到三大段空白。是骡子是马，要用用才知道（use），这时需要svg 中的<use>元素
可重复调用
<svg>
    <symbol id="icon-pencil" viewBox="0 0 32 32">
        <title>pencil</title>
        <path d="M27 0c2.761 0 5 2.239 5 5 0 1.126-0.372 2.164-1 3l-2 2-7-7 2-2c0.836-0.628 1.874-1 3-1zM2 23l-2 9 9-2 18.5-18.5-7-7-18.5 18.5zM22.362 11.362l-14 14-1.724-1.724 14-14 1.724 1.724z">
        </path>
    </symbol>
</svg>
<ul>
    <li>
        <svg>
            <use xlink:href="#icon-pencil" />
        </svg>
    </li>
    <li>
        <svg>
            <use xlink:href="#icon-pencil" />
        </svg>
    </li>
</ul>
 
一个元件我可以use多次
跨SVG调用 SVG中的use元素可以调用其他SVG文件的元素，只要在一个文档中。
总结
======
symbol+use就可以轻松使用svg spirite。
自定义svg图片
=============
之前的介绍足以满足一个通用svg图标的制作。那么如果是一些搜不到的图标，我们该怎么造轮子呢？自己绣。

手工绘制svg
==========
网格
svg图片的绘制说白了就是坐标描点，（和十字绣很像）。默认以像素为单位。左上角为(0,0)
基本形状
<svg width="200" height="250" version="1.1" xmlns="http://www.w3.org/2000/svg">
    <rect x="10" y="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>
    <rect x="60" y="10" rx="10" ry="10" width="30" height="30" stroke="black" fill="transparent" stroke-width="5"/>
 
    <circle cx="25" cy="75" r="20" stroke="red" fill="transparent" stroke-width="5"/>
    <ellipse cx="75" cy="75" rx="20" ry="5" stroke="red" fill="transparent" stroke-width="5"/>
 
    <line x1="10" x2="50" y1="110" y2="150" stroke="orange" fill="transparent" stroke-width="5"/>
    <polyline points="60 110 65 120 70 115 75 130 80 125 85 140 90 135 95 150 100 145" stroke="orange" fill="transparent" stroke-width="5"/>
 
    <polygon points="50 160 55 180 70 180 60 190 65 205 50 195 35 205 40 190 30 180 45 180"
      stroke="green" fill="transparent" stroke-width="5"/>
 
    <path d="M20,230 Q40,205 50,230 T90,230" fill="none" stroke="blue" stroke-width="5"/>
</svg>
矩形：<rect x="左上角点横坐标" y="左上角点纵坐标" rx="圆角x轴半径" ry="圆角y周半径" width="矩形宽" height="矩形高"/>
圆形：<circle cx="圆心的x位置" cy="圆心的y位置" r="圆的半径"/>
椭圆：<ellipse cx="椭圆的x半径" cy="椭圆的y半径" rx="椭圆中心的x位置" ry="椭圆中心的y位置"/>
线条：<line x1="起点的x位置" x2="终点的x位置" y1="起点的y位置" y2="终点的y位置"/>
折线：<polyline points="60 110, 65 120, 70 115, 75 130, 80 125, 85 140, 90 135, 95 150, 100 145"/>点的集合
路径：<path d="M 20 230 Q 40 205, 50 230 T 90230"/>path可能是SVG中最常见的形状。你可以用path元素绘制矩形（直角矩形或者圆角矩形）、圆形、椭圆、折线形、多边形，以及一些其他的形状，例如贝塞尔曲线、2次曲线等曲线
path
path是svg中最强大的形状。也是我们自定义形状的关键。无论多特殊的形状都是直线曲线构成的。

直线命令
<svg width="100px" height="100px" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <path d="M10 10 H 90 V 90 H 10 L 10 10"/>
</svg>
M:move to缩写，带有两个参数MX Y。常用作起始点，指明起始点x，y坐标
H：绘制水平线
V：绘制垂直线
L：Line to缩写，LX Y。和之前点之间绘制连线。
 我们也常用闭合路径Z简化path。Z表示从当前点，到起始点连线
<path d="M10 10 H 90 V 90 H 10 Z" fill="transparent" stroke="black"/>
弧线
<svg width="320px" height="320px">
    <path d="M0 0
    A 45 45, 0, 0, 0, 90 0
    L 90 0 Z" fill="blue" />
</svg>
A 椭圆的x方向宽 椭圆的y方向宽 x轴，旋转角度， 弧线大于还是小于180度（1or0）， 0从始至终画弧线 ，结束点x y坐标

曲线命令
---------
<svg width="190px" height="160px" version="1.1" xmlns="http://www.w3.org/2000/svg">
  <path d="M10 10 C 20 20, 40 20, 50 10" stroke="black" fill="transparent"/>
</svg>
曲线命令常用绘制特殊的曲线，比如凡尔赛曲线等。具体定义可参考（developer.mozilla.org/zh-CN/docs/…

填充和上色
----------
fill属性设置对象内部的颜色，stroke属性设置绘制对象的线条的颜色。

<rect x="10" y="10" width="100" height="100" stroke="blue" fill="purple" fill-opacity="0.5" stroke-opacity="0.8"/>
复制代码
一些特殊的描边方式可能也会使用
<svg width="160" height="140" xmlns="http://www.w3.org/2000/svg" version="1.1">
  <line x1="40" x2="120" y1="20" y2="20" stroke="black" stroke-width="20" stroke-linecap="butt"/>
  <line x1="40" x2="120" y1="60" y2="60" stroke="black" stroke-width="20" stroke-linecap="square"/>
  <line x1="40" x2="120" y1="100" y2="100" stroke="black" stroke-width="20" stroke-linecap="round"/>
</svg>
总结
掌握了以上一些自定义的svg图片的绘制基本解决了。

自定义svg sprite
解决了spirit绘制，那如何把他做成元件，安排到spirit这个大舞台上呢。

自定义svg整合
以上图绘制的弧形元件为例，只需要symbol标签包装一下，一个元件就诞生了

<svg>
    <symbol id="halfCircle">
        <title>halfCircle</title>
        <path d="M0 0
        A 45 45, 0, 0, 0, 90 0
        L 90 0 Z" fill="blue" />
      </symbol>
</svg>
<svg class="webicon">
    <use xlink:href="#halfCircle" />
</svg>
下载svg整合
可以使用在线工具https://icomoon.io/app/#/select 左上角导入现有svg图片，或者从页面选择需要的svg图片

 点击左下Generator 
 进入新页面对所选svg进行整合下载。 
 得到想要的svg sprite
结语
以上是制作小图标的方法。具体更多svg好文见参考文献。