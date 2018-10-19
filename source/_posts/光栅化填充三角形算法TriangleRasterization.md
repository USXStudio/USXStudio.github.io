---
title: 光栅化填充三角形算法TriangleRasterization
date: 2018-10-16 14:08:57
tags: 计算机图形图像
---
 ### 简介 triangle 光栅化算法
 本文讨论三角形光栅化算法，也就是三角形的填充算法，在平时的开发中其实该算法很少会遇到，目前的光栅化其实都是已经固化到了显卡芯片里，
 但是里面的很多思路我们还是可以借鉴的，比如扫描线算法在gis，计算几何的应用中很广泛。

 ### 标准三角形光栅化算法
标准算法主要主要考虑了底部平行x轴和顶部平行x轴这两种情况，事实上大家都知道这两种情况很好绘制。
如下图所示

<div>
        <img src = 'flatbottomtriangle.png' alt="flatbottomtriangle" style = "float:left"><img src = 'flattoptriangle.png' alt="flattoptriangle" style = "float:left">
        <br style = 'clear:both'>
</div>
如左图所示我们从点v1开始平行于底部边v2v3开始绘制一条一条的平行线。他们的交点分别为p1 ，p2。
显而易见，p1.y = p2.y，他们的y值是一样的。


同时v1.y1 - p1.y = v1.y - p2.y。 这个算法的主要流程就是从点v1 开始，沿着边v2v3一步一步的绘制直线填充这个三角形。


从点v1开始每一次一条线都是向下扫描前进的单位是dy = 1。两条腿的斜率为 slope =  dy/dx ,dx = dy/slope;


这个的算法思想如下:
1. 计算边 v1v2,v1v3斜率的倒数invslope1 , invslope2
2. 然后从v1.y 开始不断的 vec2 p1 = v1 + vec2(invslope1,1) vec2 p2 = v1 + vec2(invslope2,1)(这里偷懒用向量表示下)一步一步的前进构建直线



```
// 采用笛卡尔坐标系，下为正y，右为正x
fillBottomFlatTriangle(Vertice v1, Vertice v2, Vertice v3)
{
  float invslope1 = (v2.x - v1.x) / (v2.y - v1.y);
  float invslope2 = (v3.x - v1.x) / (v3.y - v1.y);
 
  
  float curx1 = v1.x;
  float curx2 = v1.x;

  for (int scanlineY = v1.y; scanlineY <= v2.y; scanlineY++)
  {
    drawLine((int)curx1, scanlineY, (int)curx2, scanlineY);
    // 勘误 假设左边的点x为curx1，那么curx1 -= invslope1 原文为 +=
    // 可能坐标系不一样
    curx1 += invslope1;
    curx2 += invslope2;
  }
}
```

然后考虑后面一种情况

<div> 
    <img src="flattoptriangle.png" alt="flattoptriangle">
</div>

同理绘制下面这个三角形。只不过，开始的点不一样，
```
// 采用笛卡尔坐标系，下为正y，右为正x
fillTopFlatTriangle(Vertice v1, Vertice v2, Vertice v3)
{
  float invslope1 = (v3.x - v1.x) / (v3.y - v1.y);
  float invslope2 = (v3.x - v2.x) / (v3.y - v2.y);

  float curx1 = v3.x;
  float curx2 = v3.x;

  for (int scanlineY = v3.y; scanlineY > v1.y; scanlineY--)
  {
    drawLine((int)curx1, scanlineY, (int)curx2, scanlineY);
    // 勘误 假设左边的点x为curx1，那么curx1 -= invslope1 原文为 +=
    // 可能坐标系不一样
    curx1 -= invslope1;
    curx2 -= invslope2;
  }
}
```

有了前面的基础，现在我们考虑一下如下图所示的更加一般的情况,基本思路就是将该三角形分解为和上面类型一样的两个三角形
一个平顶的一个平底的。这样我们就能将一个新问题转化为已经解决了的问题了。现在问题的关键点就是我们如何使用切分这个一般的三角形，如下图所示。
<img src="generalTriangle.png" alt="generalTriangle">
现在我们穿过点v2 沿着x轴构建一条直线。假设这条直线与三角形的交点为v4，并且v4.y = v2.y。至于v4.x 我们可以使用截线定理(Intercept theorem)获得。
具体的推导过程上图的右边已经给出了，最终我们获得了两个三角形Δ1 = (V1, V2, V4) ， Δ2 = (V2, V4, V3)，分割成这两个三角形之后，我们就能使用上面的两种特殊情况来拼凑成一般情况下的三角形光栅化算法了，稳！！
一下为具体的三角形绘制算法
```
// 采用笛卡尔坐标系，下为正y，右为正x
drawTriangle()
{
   /* at first sort the three vertices by y-coordinate ascending so v1 is the topmost vertice */
   /* 第一步将三个点按照y值大小进行排序，确定顶部的第一个点 */
  sortVerticesAscendingByY();

  /* here we know that v1.y <= v2.y <= v3.y */
  /* 这里默认的是 v1.y <= v2.y <= v3.y */
  /* check for trivial case of bottom-flat triangle */
  /* 这种情况下是一个底部平行于x轴的三角形 */
  if (v2.y == v3.y)
  {
    fillBottomFlatTriangle(v1, v2, v3);
  }
  /* check for trivial case of top-flat triangle */
  /* 这种情况下是一个顶部平行于x轴的三角形 */
  else if (vt1.y == vt2.y)
  {
    fillTopFlatTriangle(g, vt1, vt2, vt3);
  } 
  else
  {
    /* general case - split the triangle in a topflat and bottom-flat one */
    /* 一般情况下算法 */
    /* 这里默认的是 v1.y <= v2.y <= v3.y */
    /* 所以 第二个点是v2 但是在实际的应用中我们这个需要先对整个点进行排序 */
    Vertice v4 = new Vertice( (int)(vt1.x + ((float)(vt2.y - vt1.y) / (float)(vt3.y - vt1.y)) * (vt3.x - vt1.x)), vt2.y);
    fillBottomFlatTriangle(g, vt1, vt2, v4);
    fillTopFlatTriangle(g, vt2, v4, vt3);
  }
}


```


 ### Bresenham 三角形光栅化算法
首先假定大家都知道Bresenham 直线算法，假如你不知道的话，请看另外一篇介绍Bresenham算法的文章。

上面介绍了三角形光栅化的核心标准算法，但是这里会遇到一个问题。光栅化的算法是将直线绘制到屏幕上，但是因为屏幕是一个一个离散独立的点，所以在光栅化的算法中点坐标各个分量的值都应该是整数。
但是上面的算法在考虑三角形边界的时候没有考虑这个问题。所以有了 Bresenham 三角形光栅化算法。
<img src="bresenhamIdea.png" alt="generalTriangle">

```
这一段因为涉及到bresenham细节问题，暂时先不翻译，后续理解bresenham算法后再翻译

So let's start: Suppose we just want to draw the line from v1 to v2 using the bresenham algorithm. Ignoring breshenham details like swapping variables depending on the slope etc, we also assume for our example without loss of generality that we go in y-direction and having the error value e = dx/dy. So we iterate in the y-direction and in each step as long as e >= 0 go into the x-direction. Till this point we completely comply with the bresenham algorithm - nothing special.

考虑到简化不必要的额外细节，这里先忽略 bresenham 算法的一些细节问题，比如根据坡度交换变量等等。
如下图所示，假如我们在绘制线v1v2的时候，使用bresenham algorithm，

```

我们



 ### Barycentric 三角形光栅化算法

本文主要翻译自   
http://www.sunshine2k.de/coding/java/TriangleRasterization/TriangleRasterization.html

---
原文作者: [Chaos](https://github.com/ChowBu)
---
原文链接: https://usxstudio.github.io/2018/10/16/%E5%85%89%E6%A0%85%E5%8C%96%E5%A1%AB%E5%85%85%E4%B8%89%E8%A7%92%E5%BD%A2%E7%AE%97%E6%B3%95TriangleRasterization/
---
许可协议: [知识共享署名-非商业性使用 4.0 国际许可协议](http://creativecommons.org/licenses/by-nc/4.0/)
---