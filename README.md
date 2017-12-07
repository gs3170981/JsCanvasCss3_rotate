# 简易制作贝塞尔曲线动画（JS+css3+canvas）

-------------------
#### **一些废话（直接看代码的可跳过）**
贝塞尔曲线：什么是贝塞尔曲线？用过PS的就知道，那破钢笔工具就是，什么，没用过？自行百度用法。
![贝塞尔曲线](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1512059897300&di=6c24015d5594cae493f19cf26e290c02&imgtype=0&src=http://img.kuqin.com/upimg/allimg/151116/1933541963-5.png)

-------------------
#### 需要的工具

- **ctrl+c、ctrl+v**

-------------------

## 直接上代码

``` js
<!DOCTYPE html>
<html>
  <head>
    <meta charset="UTF-8">
    <title></title>
    <style>
      body {
        position: fixed;
        background: black;
      }
      #flower {
        border: 1px solid red;
        width: 500px;
        height: 500px;
        position: relative;
      }
    </style>
  </head>
  <body>
    <div id="flower"></div>
  </body>
  <script>
    (function (data) {
      var _methods = {
        // 初始化
        init: function () {
          this.data = data.data
          this.e = data.e
          this.count = data.number
          this.color = data.color
          // number = 度数° --- this.count = 循环次数
          this.number = 360 / this.count
          for (var i = 0, j = .1; i < this.count; i++, j += .1) {
            var canvas = document.createElement('canvas')
            canvas.id = 'flower' + i
            canvas.height = data.height
            canvas.width = data.width
            canvas.style.position = 'absolute'
            canvas.style.transition = j + 's ease'
            this.$(this.e).appendChild(canvas)
            // 绘图
            this.painting(this.$('flower' + i))
          }
          // 动画
          this.handle()
        },
        // 绘图
        painting: function (e) {
          var ctx = e.getContext("2d")
          ctx.beginPath()
          // ----起始坐标（x， y）
          ctx.moveTo(this.data.start.x, this.data.start.y)
          // ----曲线坐标（x, y）--结束坐标（x, y）
          ctx.quadraticCurveTo(this.data.curve.x, this.data.curve.y, this.data.end.x, this.data.end.y)
          /* 轴对称 --- 只针对曲线左到右 */
          ctx.moveTo(this.data.start.x, this.data.start.y)
          ctx.quadraticCurveTo(this.data.start.x + this.data.curve.x, this.data.curve.y, this.data.end.x, this.data.end.y)
          ctx.fillStyle = this.color
          ctx.strokeStyle = this.color
          ctx.globalAlpha = 0.1
          ctx.fill()
          ctx.stroke()
        },
        // 动画
        handle: function () {
          var self = this
          var i = 0
          var timer = setInterval(function () {
            if (i > self.count) {
              clearInterval(timer)
              return
            }
            self.$('flower' + i).style.transform = 'rotate(' + i * self.number + 'deg)'
            i++
          })
        },
        $: function (dom) {
          return document.getElementById(dom)
        }
      }
      _methods.init()
    })({
      /* 配置项 */
      e: "flower",
      height: 500,
      width: 500,
      color: '#cc00ff',
      data: {
        // ----起始坐标（x， y）
        start: {
          x: 250,
          y: 0
        },
        // ----曲线坐标（x, y）
        curve: {
          x: 125,
          y: 125
        },
        // ----结束坐标（x， y）
        end: {
          x: 250,
          y: 250
        }
      },
      // 数量
      number: 15
    })
  </script>
</html>
```
![过渡中的动画](http://img.blog.csdn.net/20171130221311994?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTWNreV9Mb3Zl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

>过渡中的动画

-------------------
![结束的动画](http://img.blog.csdn.net/20171130221421925?watermark/2/text/aHR0cDovL2Jsb2cuY3Nkbi5uZXQvTWNreV9Mb3Zl/font/5a6L5L2T/fontsize/400/fill/I0JBQkFCMA==/dissolve/70/gravity/SouthEast)

这里推荐一个网站可以免费手画贝塞尔曲线，可以复制里面的值：
http://www.j--d.com/bezier

-------------------
## 简单解释


#### 1）坐标解释
``` js
// ----起始坐标（x， y）
start: {
  x: 250,
  y: 0
},
// ----曲线坐标（x, y）
curve: {
  x: 125,
  y: 125
},
// ----结束坐标（x， y）
end: {
  x: 250,
  y: 250
}

```
这里真的是坑走多了得出的结论，稍微要解释的就是中间的曲线坐标，他就是两点之间的曲线角度
#### 2）Y轴的坑

>所有的Y轴为顶点到底部的距离值非左下角原点

#### 3）过程解释
>init（初始化遍历生成Dom结构并执行painting绘图Canvas）
>handle（生成动画）

-------------------

## 一个完整的DEMO

github：https://github.com/gs3170981/JsCanvasCss3_rotate（好用的话记得加星哦！）


-------------------

## 关于

make：o︻そ╆OVE▅▅▅▆▇◤（清一色天空）

blog：http://blog.csdn.net/mcky_love

-------------------

## 结束语
什么，你说有啥用？这难道不像你的菊花吗？^_^哈哈......开玩笑
你试着修改下曲线任意的值看看效果，不够？那你修改下源代码，增加下颜色的随机性试试？