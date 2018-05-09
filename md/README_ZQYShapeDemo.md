# ZQYShapeDemo

- **效果图**

![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/1.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/2.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/3.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/4.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/5.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/6.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/7.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/8.png)
![image](https://github.com/zhaoqingyue/ZQYShapeDemo/blob/master/img/9.png)


- **使用Drawable有什么好处**

```

1. 方便得到一个矩形、圆、椭圆、圆环，容易维护和修改

2. 方便实现圆角，渐变（线性渐变，径向渐变，扫描渐变）

3. 代替图片作为 View 的背景，减少 apk 的体积

4. 大图片耗内存，使用 Drawable 节省内存

```


- **shape标签定义的类型Drawable**

1. shape 可以定义四种类型的几何图形，由 android:shape 属性指定


```

- line --> 线

- rectangle --> 矩形（圆角矩形）

- oval --> 椭圆，圆

- ring --> 圆环

```


2. shape 可以定义边框属性


```

- 有边框

- 无边框

- 虚线边框

- 实线边框

```


3. shape 可以实现矩形圆角效果


```

可以指定其中一个角或者多个角设置圆角效果
指定圆角半径设置圆角的大小

```

4. shape 可以实现三种渐变，由子标签 gradient 实现


```

- linear --> 线性渐变（水平，垂直，对角线三个渐变）

- sweep --> 扫描渐变（只支持顺时针方向，其实颜色反过来跟逆时针一样）

- radial --> 径向渐变（由指定的中心点开始向外渐变，指定半径）

xml 实现只支持三个颜色，startColor，CenterColor，endColor

```


