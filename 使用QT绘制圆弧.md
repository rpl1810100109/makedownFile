从HMI程序中列举的几个关键函数、类

```cpp
QPainterPath *painterPath
painterPath->moveTo
painterPath->arcTo(newRect,althp,fabs(360+delta));
p.drawPath(*painterPath);
```

## QPainterPath

可以记录画笔路径，画直线画圆弧









## arcTo函数

这是一个关键的函数

有两个重载：

```cpp
void QPainterPath::arcTo(qreal x, qreal y, qreal width, qreal height, qreal startAngle, qreal sweepLength)

void QPainterPath::arcTo(const QRectF &rectangle, qreal startAngle, qreal sweepLength)

```

两个其实差不多，都是接受一个矩形的坐标和宽、高，然后加上圆弧的开始角度和结束角度。

## moveTo函数

```cpp
path.moveTo(wid+30,30+het/2);
```

接受一个起始点，用这个函数可以消除连线,否则会有一条起点到圆弧起点的连线

## drawPath

使用画笔的drawPath可以画出这个圆弧