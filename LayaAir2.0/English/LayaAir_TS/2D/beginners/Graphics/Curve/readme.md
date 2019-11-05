#draw a curve



It is more difficult to understand the relationship between the drawing of curves and coordinates than that of straight lines. Because LayaAir engine draws Bessel curve, this paper first explains the basis of Bessel curve, and then explains it with API of engine.



### **I. Basis of Bessel Curve**

Bessel Curve is called Bezier Curve in Hong Kong, Macao and Taiwan, and Bezier Curve in Singapore and Malaysia. General vector graphics software draws curves accurately through it. Bessel curve is composed of line segments and nodes. Nodes are pivots that can be dragged. Lines are like stretchable tendons. The pen tool we see on the drawing tool is used to make such vector curves.

Bessel curve is a mathematical curve applied to two-dimensional graphics applications. The definition of a curve has four points: the starting point, the ending point (also known as anchor point) and two separated intermediate points. The shape of Bessel curve will change when two intermediate points are slipped.

Based on the difference of linear, quadratic, cubic and other formulas, Bessel curve is also called primary, quadratic... The quintic Bessel curve is also called the first and second order in some articles. It's one thing. Now let's have an intuitive understanding through the motion picture.

#### **1.1 Primary Bessel Curve**



​        ![1.gif](gif/1.gif)<br/>
(Fig. 1)

Explanation: The figure above is a continuous point from P0 to P1, describing a linear Bessel curve. T in the linear Bessel curve function passes through the curve described by B (t) from P0 to P1. For example, when t = 0.25, B (t) is a quarter of the path from point P0 to P1. Like a continuous t from 0 to 1, B (t) describes a line from P0 to P1.

#### **1.2 Quadratic Bessel Curve**

​![2.gif](gif/2.gif)<br/>
(Figure 2)

​![blob.png](img/1.png)<br/>
(Figure 3)

Explanation: To construct a quadratic Bessel curve, the continuous point Q0 from P0 to P1 in the figure above describes a linear Bessel curve. The continuous point Q1 from P1 to P2 describes a linear Bessel curve. The continuous point B (t) from Q0 to Q1 describes a quadratic Bessel curve.

#### **1.3 Cubic Bessel Curve**

​![3.gif](gif/3.gif)<br/>
(Figure 4)

​![blob.png](img/2.png)<br/>
(Fig. 5)

Explanation: For cubic curve, the intermediate points Q0, Q1, Q2 described by linear Bessel curve and points R0 and R1 described by quadratic curve can be constructed.

#### **1.4 higher order Bessel curve**

Since higher-order Bessel curves are not common, this article will not elaborate on them. If you want to know more about the principle of Bessel curves, you can see other related articles.

​![4.gif](gif/4.gif)<br/>
(Fig. 6) Fourth Bessel Curve

​![5.gif](gif/5.gif)<br/>
(fig. 7) quintic Bessel curve





### **2. Drawing Quadratic Bessel Curve with LayaAir Engine API**

LayaAir engine uses quadratic Bessel curve to draw curve. Developers can search the laya.display.Graphics class in API document to see the drawing Curves ();"curve drawing method. A detailed description of the method is shown in the following figure:

​![blob.png](img/3.png)<br/>

(Figure 8)

Next, we use the LayaAir engine to draw vector curves. The sample code is as follows:


```typescript

module laya {
    import Sprite = Laya.Sprite;
    import Stage = Laya.Stage;
    import WebGL = Laya.WebGL;
  
    export class Sprite_DrawShapes {
        private sp: Sprite;
  
        constructor()
        {
            Laya.init(500, 300, WebGL);
            this.drawSomething();
        }
  
        private drawSomething(): void {
            this.sp = new Sprite();
            Laya.stage.addChild(this.sp);
            //画折线
            this.sp.graphics.drawCurves(10, 58, [0, 0, 19, -100, 39, 0], "#ff0000", 3);
      }
    }
}
new laya.Sprite_DrawShapes();
```


After publication, as shown in the figure below, we succeeded in drawing a simple curve.

​![blob.png](img/4.png)<br/>
(Figure 9)

By adding the parameters of the third point set of drawCurves, we can make the curve more complex. The modified example code is as follows:


```typescript

//增加58, 100与78, 0坐标让曲线更复杂一些
  
this.sp.graphics.drawCurves(10, 58, [0, 0, 19, -100, 39, 0, 58, 100, 78, 0], "#ff0000", 3) ;
```


After publication, as shown in the following figure,

​![blob.png](img/5.png)<br/>
(Fig. 10)
If you want to draw more complex curves, you can adjust the parameters in drawCurves by yourself, and then understand them with the principle of quadratic Bessel curve.

Finally, it is reminded that, like drawing polygons, all coordinates in the third parameter are relative coordinates, which are affected by the "10,58" of the first and second parameters. Once the "10,58" changes, the overall curve will be affected.





###3. Drawing quadratic Bessel curve with LayaAirIDE dragging control

​**Step one**Open our LayaAirIDE, click Design Patterns, and create a new View page

​![6](img/6.png)<br/>
(Fig. 11)

**Step two**Drag the curve component from the component onto the View page and automatically generate the default curve

​![7](img/7.png)<br/>
(Fig. 12)

**Step three**Modify (add/reduce) the point value in component attributes to change the position or curvature of the curve

​![8](img/8.png)<br/>
(Fig. 13)

​![9](img/9.png)<br/>
(Fig. 14)

So far, we have finished drawing curves through the components of LayaAirIDE.