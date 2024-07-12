---
title: "FixIt主题优化"
date: 2024-07-02T13:56:13+08:00
lastmod: 2024-07-02T13:56:13+08:00
draft: false
tags: ["建站"]
categories: ["建站"]
collections: ["Hugo建站日志"]
featuredImage: ""
featuredImagePreview: ""
---

写在前面，因为每个人审美不同，我们完全可以对主题的细节进行修改，但是，不建议直接在主题上进行修改。因为Hugo允许在站点根目录下对应的文件夹下的样式去覆盖主题默认的样式，实际就是生成了两份代码，然后在网页中主题中的那一份样式优先级会降低，而我们定义的样式优先级更高，所以会优先使用自定义的样式。

比如，我们新增或者要修改一些主题中scss文件中的样式：可以在站点目录下`assert/css` 下面创建`_custom.scss`文件，里面定义我们自定义的css样式，如果需要覆盖主题的样式，最好在样式的属性值后面加上`!important`表示优先级更高。

同理：

- 新增的js放在站点根目录下`assert/js`下。
- 自定义菜单的html文章内容显示的模块放在`layouts\your_menu\single.html`。
- 自定义的字体放在`static\fonts`，引用时组要在\_custom.scss中加入下面的代码，最好引入woff或woff2类型的字体，体积小很多：
  ```纯文本
  @font-face {
    font-family: "Intel One Mono"; /* 名称自定义 */
    src: url('/fonts/IntelOneMono-Regular.woff2') format('woff2');
  }
  ```
- 图片放在`static\images`，在引用图片时直接用`/images/xxxxxx.png`即可。

## 细节修改

1. 修改常用的FontAwesome图标

   有的地方需要fa图标的unicode编码，可以参考这个网站[FontAwesome图标大全](https://tool56.com/info-fontawesome/ "FontAwesome图标大全")。
2. 修改代码块背景统一

   需要将全局包括黑白主题如下属性：
   ```css
   background-color: darken($code-background-color, x%);
   ```
   统一修改为：
   ```css
   background-color: $code-background-color;
   ```
3. 代码块加边框
   ```css
   .highlight {
     > .chroma {
       @include border-radius($global-border-radius);
       border: 0.1em solid $code-border-color;
       
       [data-theme='dark'] & {
         border: 0.1em solid $code-border-color-dark;
       }
     }
   }
   ```
4. 修改全局英文字体

   将导入的字体放在前面，调整字体显示顺序。
   ```css
   $global-font-family: Rubik, system-ui, -apple-system, BlinkMacSystemFont, PingFang SC, Microsoft YaHei UI, Segoe UI, Roboto, Oxygen, Ubuntu, Cantarell, Fira Sans, Droid Sans, Helvetica Neue, Helvetica, Arial, sans-serif;
   ```
   全局搜索所有使用 `$global-font-family` 的css进行替换。
5. 修改代码中的中英文字体
   ```css
   $code-font-family: Intel One Mono, Source Code Pro, Menlo, Consolas, Monaco, $global-font-family;
   
   ```
   同上，全局搜索所有使用 `$code-font-family` 的css进行替换。
6. 修改全局字体大小和行高
   ```css
   $global-font-size: 18px;
   $global-line-height: 1.8rem;
   ```
7. 修改主页文章分割线的粗细
   ```css
   .summary {
     border-bottom: 2px dashed $global-border-color !important;
   }
   ```

## 加入小鱼动画

在站点目录下创建文件 `assets/js/flyfish.js`，内容如下：

```css
/**
 * 源码来自互联网，作者不详
 * @modified by Lruihao 2024-05-21 移除依赖 jQuery
 * @description 一个鱼游动的动画效果
 */
const RENDERER = {
    POINT_INTERVAL: 5,
    FISH_COUNT: 3,
    MAX_INTERVAL_COUNT: 50,
    INIT_HEIGHT_RATE: 0.5,
    THRESHOLD: 50,
  
    init: function () {
      this.setParameters();
      this.setStyle();
      this.reconstructMethods();
      this.setup();
      this.bindEvent();
      this.render();
    },
    setParameters: function () {
      this.window = window;
      this.container = document.createElement("div");
      this.container.id = "flyfish";
      this.canvas = document.createElement("canvas");
      this.context = this.canvas.getContext("2d");
      this.points = [];
      this.fishes = [];
      this.watchIds = [];
      document.querySelector('.footer').appendChild(this.container);
    },
    setStyle: function () {
      const style = document.createElement("style");
      style.innerHTML = `
      .footer {
        position: relative;
      }
      #flyfish {
        position: absolute;
        width: 100%;
        height: 250px;
        overflow: hidden;
        left: 0;
        bottom: 0;
        z-index: -1;
        pointer-events: none;
      }`;
      document.querySelector("head").appendChild(style);
    },
    createSurfacePoints: function () {
      const count = Math.round(this.width / this.POINT_INTERVAL);
      this.pointInterval = this.width / (count - 1);
      this.points.push(new SURFACE_POINT(this, 0));
  
      for (let i = 1; i < count; i++) {
        const point = new SURFACE_POINT(this, i * this.pointInterval),
          previous = this.points[i - 1];
  
        point.setPreviousPoint(previous);
        previous.setNextPoint(point);
        this.points.push(point);
      }
    },
    reconstructMethods: function () {
      this.watchWindowSize = this.watchWindowSize.bind(this);
      this.jdugeToStopResize = this.jdugeToStopResize.bind(this);
      this.startEpicenter = this.startEpicenter.bind(this);
      this.moveEpicenter = this.moveEpicenter.bind(this);
      this.render = this.render.bind(this);
    },
    setup: function () {
      this.points.length = 0;
      this.fishes.length = 0;
      this.watchIds.length = 0;
      this.intervalCount = this.MAX_INTERVAL_COUNT;
  
      this.containerWidth = this.container.offsetWidth;
      this.containerHeight = this.container.offsetHeight;
      this.width = this.containerWidth;
      this.height = this.containerHeight;
      this.fishCount =
        (((this.FISH_COUNT * this.width) / 500) * this.height) / 500;
      this.canvas.width = this.width;
      this.canvas.height = this.height;
      this.reverse = false;
  
      this.container.appendChild(this.canvas);
      this.fishes.push(new FISH(this));
      this.createSurfacePoints();
    },
    watchWindowSize: function () {
      this.clearTimer();
      this.tmpWidth = this.window.innerWidth;
      this.tmpHeight = this.window.innerHeight;
      this.watchIds.push(setTimeout(this.jdugeToStopResize, this.WATCH_INTERVAL));
    },
    clearTimer: function () {
      while (this.watchIds.length > 0) {
        clearTimeout(this.watchIds.pop());
      }
    },
    jdugeToStopResize: function () {
      const width = this.window.innerWidth,
        height = this.window.innerHeight,
        stopped = width == this.tmpWidth && height == this.tmpHeight;
  
      this.tmpWidth = width;
      this.tmpHeight = height;
  
      if (stopped) {
        this.setup();
      }
    },
    bindEvent: function () {
      const self = this;
      this.window.addEventListener("resize", function() {
        self.watchWindowSize();
      });
      this.container.addEventListener("mouseenter", function(event) {
        self.startEpicenter(event);
      });
      this.container.addEventListener("mousemove", function(event) {
        self.moveEpicenter(event);
      });
    },
    getAxis: function (event) {
      const offset = this.container.getBoundingClientRect();
  
      return {
        x: event.clientX - offset.left + this.window.scrollX,
        y: event.clientY - offset.top + this.window.scrollY,
      };
    },
    startEpicenter: function (event) {
      this.axis = this.getAxis(event);
    },
    moveEpicenter: function (event) {
      const axis = this.getAxis(event);
  
      if (!this.axis) {
        this.axis = axis;
      }
      this.generateEpicenter(axis.x, axis.y, axis.y - this.axis.y);
      this.axis = axis;
    },
    generateEpicenter: function (x, y, velocity) {
      if (
        y < this.height / 2 - this.THRESHOLD ||
        y > this.height / 2 + this.THRESHOLD
      ) {
        return;
      }
      const index = Math.round(x / this.pointInterval);
  
      if (index < 0 || index >= this.points.length) {
        return;
      }
      this.points[index].interfere(y, velocity);
    },
    controlStatus: function () {
      for (let i = 0, count = this.points.length; i < count; i++) {
        this.points[i].updateSelf();
      }
      for (let i = 0, count = this.points.length; i < count; i++) {
        this.points[i].updateNeighbors();
      }
      if (this.fishes.length < this.fishCount) {
        if (--this.intervalCount == 0) {
          this.intervalCount = this.MAX_INTERVAL_COUNT;
          this.fishes.push(new FISH(this));
        }
      }
    },
    render: function () {
      const self = this;
      function renderFrame() {
        self.controlStatus();
        self.context.clearRect(0, 0, self.width, self.height);
        // 这里利用 FixIt 主题的 isDark 属性来判断是否是暗色主题
        if (fixit.isDark) {
          self.context.fillStyle = "rgb(255 255 255 / 10%)";
        } else {
          self.context.fillStyle = "#e6e5f8";
        }
  
        for (let i = 0, count = self.fishes.length; i < count; i++) {
          self.fishes[i].render(self.context);
        }
        self.context.save();
        self.context.globalCompositeOperation = "xor";
        self.context.beginPath();
        self.context.moveTo(0, self.reverse ? 0 : self.height);
  
        for (let i = 0, count = self.points.length; i < count; i++) {
          self.points[i].render(self.context);
        }
        self.context.lineTo(self.width, self.reverse ? 0 : self.height);
        self.context.closePath();
        self.context.fill();
        self.context.restore();
  
        requestAnimationFrame(renderFrame);
      }
      renderFrame();
    },
  };
  
  // SURFACE_POINT class
  function SURFACE_POINT(renderer, x) {
    this.renderer = renderer;
    this.x = x;
    this.init();
  }
  SURFACE_POINT.prototype = {
    SPRING_CONSTANT: 0.03,
    SPRING_FRICTION: 0.9,
    WAVE_SPREAD: 0.3,
    ACCELARATION_RATE: 0.01,
  
    init: function () {
      this.initHeight = this.renderer.height * this.renderer.INIT_HEIGHT_RATE;
      this.height = this.initHeight;
      this.fy = 0;
      this.force = { previous: 0, next: 0 };
    },
    setPreviousPoint: function (previous) {
      this.previous = previous;
    },
    setNextPoint: function (next) {
      this.next = next;
    },
    interfere: function (y, velocity) {
      this.fy =
        this.renderer.height *
        this.ACCELARATION_RATE *
        (this.renderer.height - this.height - y >= 0 ? -1 : 1) *
        Math.abs(velocity);
    },
    updateSelf: function () {
      this.fy += this.SPRING_CONSTANT * (this.initHeight - this.height);
      this.fy *= this.SPRING_FRICTION;
      this.height += this.fy;
    },
    updateNeighbors: function () {
      if (this.previous) {
        this.force.previous =
          this.WAVE_SPREAD * (this.height - this.previous.height);
      }
      if (this.next) {
        this.force.next = this.WAVE_SPREAD * (this.height - this.next.height);
      }
    },
    render: function (context) {
      if (this.previous) {
        this.previous.height += this.force.previous;
        this.previous.fy += this.force.previous;
      }
      if (this.next) {
        this.next.height += this.force.next;
        this.next.fy += this.force.next;
      }
      context.lineTo(this.x, this.renderer.height - this.height);
    },
  };
  
  // FISH class
  function FISH(renderer) {
    this.renderer = renderer;
    this.init();
  }
  FISH.prototype = {
    GRAVITY: 0.4,
  
    init: function () {
      this.direction = Math.random() < 0.5;
      this.x = this.direction
        ? this.renderer.width + this.renderer.THRESHOLD
        : -this.renderer.THRESHOLD;
      this.previousY = this.y;
      this.vx = this.getRandomValue(4, 10) * (this.direction ? -1 : 1);
  
      if (this.renderer.reverse) {
        this.y = this.getRandomValue(
          (this.renderer.height * 1) / 10,
          (this.renderer.height * 4) / 10
        );
        this.vy = this.getRandomValue(2, 5);
        this.ay = this.getRandomValue(0.05, 0.2);
      } else {
        this.y = this.getRandomValue(
          (this.renderer.height * 6) / 10,
          (this.renderer.height * 9) / 10
        );
        this.vy = this.getRandomValue(-5, -2);
        this.ay = this.getRandomValue(-0.2, -0.05);
      }
      this.isOut = false;
      this.theta = 0;
      this.phi = 0;
    },
    getRandomValue: function (min, max) {
      return min + (max - min) * Math.random();
    },
    controlStatus: function (context) {
      this.previousY = this.y;
      this.x += this.vx;
      this.y += this.vy;
      this.vy += this.ay;
  
      if (this.renderer.reverse) {
        if (this.y > this.renderer.height * this.renderer.INIT_HEIGHT_RATE) {
          this.vy -= this.GRAVITY;
          this.isOut = true;
        } else {
          if (this.isOut) {
            this.ay = this.getRandomValue(0.05, 0.2);
          }
          this.isOut = false;
        }
      } else {
        if (this.y < this.renderer.height * this.renderer.INIT_HEIGHT_RATE) {
          this.vy += this.GRAVITY;
          this.isOut = true;
        } else {
          if (this.isOut) {
            this.ay = this.getRandomValue(-0.2, -0.05);
          }
          this.isOut = false;
        }
      }
      if (!this.isOut) {
        this.theta += Math.PI / 20;
        this.theta %= Math.PI * 2;
        this.phi += Math.PI / 30;
        this.phi %= Math.PI * 2;
      }
      this.renderer.generateEpicenter(
        this.x + (this.direction ? -1 : 1) * this.renderer.THRESHOLD,
        this.y,
        this.y - this.previousY
      );
  
      if (
        (this.vx > 0 && this.x > this.renderer.width + this.renderer.THRESHOLD) ||
        (this.vx < 0 && this.x < -this.renderer.THRESHOLD)
      ) {
        this.init();
      }
    },
    render: function (context) {
      context.save();
      context.translate(this.x, this.y);
      context.rotate(Math.PI + Math.atan2(this.vy, this.vx));
      context.scale(1, this.direction ? 1 : -1);
      context.beginPath();
      context.moveTo(-30, 0);
      context.bezierCurveTo(-20, 15, 15, 10, 40, 0);
      context.bezierCurveTo(15, -10, -20, -15, -30, 0);
      context.fill();
  
      context.save();
      context.translate(40, 0);
      context.scale(0.9 + 0.2 * Math.sin(this.theta), 1);
      context.beginPath();
      context.moveTo(0, 0);
      context.quadraticCurveTo(5, 10, 20, 8);
      context.quadraticCurveTo(12, 5, 10, 0);
      context.quadraticCurveTo(12, -5, 20, -8);
      context.quadraticCurveTo(5, -10, 0, 0);
      context.fill();
      context.restore();
  
      context.save();
      context.translate(-3, 0);
      context.rotate(
        (Math.PI / 3 + (Math.PI / 10) * Math.sin(this.phi)) *
          (this.renderer.reverse ? -1 : 1)
      );
  
      context.beginPath();
  
      if (this.renderer.reverse) {
        context.moveTo(5, 0);
        context.bezierCurveTo(10, 10, 10, 30, 0, 40);
        context.bezierCurveTo(-12, 25, -8, 10, 0, 0);
      } else {
        context.moveTo(-5, 0);
        context.bezierCurveTo(-10, -10, -10, -30, 0, -40);
        context.bezierCurveTo(12, -25, 8, -10, 0, 0);
      }
      context.closePath();
      context.fill();
      context.restore();
      context.restore();
      this.controlStatus(context);
    },
  };
  
  window.onload = function () {
    RENDERER.init();
  };
```

注意：小鱼游泳的颜色和高度可以改。

在 hugo.toml 主题参数配置中引入 `flyfish.js` 文件：

```css
[params]
  # ...
  [params.page.library]
    [params.page.library.js]
      flyfish = "/js/flyfish.js"
```

## 引入自定义自定义的字体

在主题根目录下`static\fonts` 中加入自己的字体，没有没有文件夹需要自己创建，字体太大，最好是用压缩的woff2字体。

然后在主题根目录下 `asserts/css/_custom.scss` 中引入：

```css
// 添加自定义字体,防止安卓和ios用其他字体显示
@font-face {
    font-family: "Rubik"; /* 名称自定义 */
    src: url('/fonts/Rubik-Regular.ttf') format('truetype');
  }
```

注意，不要放在主题目录下对应的的 fonts 和 \_custom.scss 中，方便以后主题的更新，参考：[https://github.com/Lruihao/hugo-blog/blob/main/assets/css/\_custom.scss](https://github.com/Lruihao/hugo-blog/blob/main/assets/css/_custom.scss "https://github.com/Lruihao/hugo-blog/blob/main/assets/css/_custom.scss")。

## 增加奶油效果

首先将资源放在

在主题的根目录下，`assert\css\_shaky.scss` 中加入

```css
.shaky {
    display: inline-block;
    padding: 1px;
    font-size: 1em;
    -webkit-transform-origin: center center;
    -ms-transform-origin: center center;
    transform-origin: center center;
    -webkit-animation-name: shaky-slow;
    -ms-animation-name: shaky-slow;
    animation-name: shaky-slow;
    -webkit-animation-duration: 4s;
    -ms-animation-duration: 4s;
    animation-duration: 4s;
    -webkit-animation-iteration-count: infinite;
    -ms-animation-iteration-count: infinite;
    animation-iteration-count: infinite;
    -webkit-animation-timing-function: ease-in-out;
    -ms-animation-timing-function: ease-in-out;
    animation-timing-function: ease-in-out;
    -webkit-animation-delay: 0s;
    -ms-animation-delay: 0s;
    animation-delay: 0s;
    -webkit-animation-play-state: running;
    -ms-animation-play-state: running;
    animation-play-state: running;
  }
  @-webkit-keyframes shaky-slow {
    0% {
      -webkit-transform: translate(0px, 0px) rotate(0deg)
    }
    2% {
      -webkit-transform: translate(-1px, 1.5px) rotate(1.5deg)
    }
    4% {
      -webkit-transform: translate(1.3px, 0px) rotate(-0.5deg)
    }
    6% {
      -webkit-transform: translate(1.4px, 1.4px) rotate(-2deg)
    }
    8% {
      -webkit-transform: translate(-1.3px, -1px) rotate(-1.5deg)
    }
    10% {
      -webkit-transform: translate(1.4px, 0px) rotate(-2deg)
    }
    12% {
      -webkit-transform: translate(-1.3px, -1px) rotate(-2deg)
    }
    14% {
      -webkit-transform: translate(1.5px, 1.3px) rotate(1.5deg)
    }
    16% {
      -webkit-transform: translate(1.5px, -1.5px) rotate(-1.5deg)
    }
    18% {
      -webkit-transform: translate(1.3px, -1.3px) rotate(-2deg)
    }
    20% {
      -webkit-transform: translate(1px, 1px) rotate(-0.5deg)
    }
    22% {
      -webkit-transform: translate(1.3px, 1.5px) rotate(-2deg)
    }
    24% {
      -webkit-transform: translate(-1.4px, -1px) rotate(2deg)
    }
    26% {
      -webkit-transform: translate(1.3px, -1.3px) rotate(0.5deg)
    }
    28% {
      -webkit-transform: translate(1.6px, -1.6px) rotate(-2deg)
    }
    30% {
      -webkit-transform: translate(-1.3px, -1.3px) rotate(-1.5deg)
    }
    32% {
      -webkit-transform: translate(-1px, 0px) rotate(2deg)
    }
    34% {
      -webkit-transform: translate(1.3px, 1.3px) rotate(-0.5deg)
    }
    36% {
      -webkit-transform: translate(1.3px, 1.6px) rotate(1.5deg)
    }
    38% {
      -webkit-transform: translate(1.3px, -1.6px) rotate(1.5deg)
    }
    40% {
      -webkit-transform: translate(-1.4px, -1px) rotate(-0.5deg)
    }
    42% {
      -webkit-transform: translate(-1.4px, 1.3px) rotate(-0.5deg)
    }
    44% {
      -webkit-transform: translate(-1.6px, 1.4px) rotate(0.5deg)
    }
    46% {
      -webkit-transform: translate(-2.1px, -1.3px) rotate(-0.5deg)
    }
    48% {
      -webkit-transform: translate(1px, 1.6px) rotate(1.5deg)
    }
    50% {
      -webkit-transform: translate(1.6px, 1.6px) rotate(1.5deg)
    }
    52% {
      -webkit-transform: translate(-1.4px, 1.6px) rotate(0.5deg)
    }
    54% {
      -webkit-transform: translate(1.6px, -1px) rotate(-2deg)
    }
    56% {
      -webkit-transform: translate(1.3px, -1.6px) rotate(-2deg)
    }
    58% {
      -webkit-transform: translate(-1.3px, -1.6px) rotate(0.5deg)
    }
    60% {
      -webkit-transform: translate(1.3px, 1.6px) rotate(-0.5deg)
    }
    62% {
      -webkit-transform: translate(0px, 0px) rotate(-1.5deg)
    }
    64% {
      -webkit-transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    66% {
      -webkit-transform: translate(1.6px, -1.6px) rotate(0.5deg)
    }
    68% {
      -webkit-transform: translate(0px, -1.6px) rotate(-2deg)
    }
    70% {
      -webkit-transform: translate(-1.6px, 1px) rotate(1.5deg)
    }
    72% {
      -webkit-transform: translate(-1.6px, 1.6px) rotate(2deg)
    }
    74% {
      -webkit-transform: translate(1.3px, -1.6px) rotate(-0.5deg)
    }
    76% {
      -webkit-transform: translate(1.4px, 1px) rotate(-0.5deg)
    }
    78% {
      -webkit-transform: translate(-1px, 1.4px) rotate(2deg)
    }
    80% {
      -webkit-transform: translate(1.4px, 1.6px) rotate(2deg)
    }
    82% {
      -webkit-transform: translate(-1.6px, -1.6px) rotate(-0.5deg)
    }
    84% {
      -webkit-transform: translate(-1.4px, 1.4px) rotate(-2deg)
    }
    86% {
      -webkit-transform: translate(1px, 1.4px) rotate(-2deg)
    }
    88% {
      -webkit-transform: translate(-1.4px, 1.4px) rotate(-1.5deg)
    }
    90% {
      -webkit-transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    92% {
      -webkit-transform: translate(-1.6px, 1.6px) rotate(2deg)
    }
    94% {
      -webkit-transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    96% {
      -webkit-transform: translate(-1.4px, 1.3px) rotate(-2deg)
    }
    98% {
      -webkit-transform: translate(1.3px, 1px) rotate(-0.5deg)
    }
  }
  @keyframes shaky-slow {
    0% {
      transform: translate(0px, 0px) rotate(0deg)
    }
    2% {
      transform: translate(-1px, 1.5px) rotate(1.5deg)
    }
    4% {
      transform: translate(1.3px, 0px) rotate(-0.5deg)
    }
    6% {
      transform: translate(1.4px, 1.4px) rotate(-2deg)
    }
    8% {
      transform: translate(-1.3px, -1px) rotate(-1.5deg)
    }
    10% {
      transform: translate(1.4px, 0px) rotate(-2deg)
    }
    12% {
      transform: translate(-1.3px, -1px) rotate(-2deg)
    }
    14% {
      transform: translate(1.5px, 1.3px) rotate(1.5deg)
    }
    16% {
      transform: translate(1.5px, -1.5px) rotate(-1.5deg)
    }
    18% {
      transform: translate(1.3px, -1.3px) rotate(-2deg)
    }
    20% {
      transform: translate(1px, 1px) rotate(-0.5deg)
    }
    22% {
      transform: translate(1.3px, 1.5px) rotate(-2deg)
    }
    24% {
      transform: translate(-1.4px, -1px) rotate(2deg)
    }
    26% {
      transform: translate(1.3px, -1.3px) rotate(0.5deg)
    }
    28% {
      transform: translate(1.6px, -1.6px) rotate(-1.5deg)
    }
    30% {
      transform: translate(-1.3px, -1.3px) rotate(-1.5deg)
    }
    32% {
      transform: translate(-1px, 0px) rotate(2deg)
    }
    34% {
      transform: translate(1.3px, 1.3px) rotate(-0.5deg)
    }
    36% {
      transform: translate(1.3px, 1.6px) rotate(1.5deg)
    }
    38% {
      transform: translate(1.3px, -1.6px) rotate(1.5deg)
    }
    40% {
      transform: translate(-1.4px, -1px) rotate(-0.5deg)
    }
    42% {
      transform: translate(-1.4px, 1.3px) rotate(-0.5deg)
    }
    44% {
      transform: translate(-1.6px, 1.4px) rotate(0.5deg)
    }
    46% {
      transform: translate(-2.1px, -1.3px) rotate(-0.5deg)
    }
    48% {
      transform: translate(1px, 1.6px) rotate(1.5deg)
    }
    50% {
      transform: translate(1.6px, 1.6px) rotate(1.5deg)
    }
    52% {
      transform: translate(-1.4px, 1.6px) rotate(0.5deg)
    }
    54% {
      transform: translate(1.6px, -1px) rotate(-2deg)
    }
    56% {
      transform: translate(1.3px, -1.6px) rotate(-2deg)
    }
    58% {
      transform: translate(-1.3px, -1.6px) rotate(0.5deg)
    }
    60% {
      transform: translate(1.3px, 1.6px) rotate(-0.5deg)
    }
    62% {
      transform: translate(0px, 0px) rotate(-1.5deg)
    }
    64% {
      transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    66% {
      transform: translate(1.6px, -1.6px) rotate(0.5deg)
    }
    68% {
      transform: translate(0px, -1.6px) rotate(-2deg)
    }
    70% {
      transform: translate(-1.6px, 1px) rotate(1.5deg)
    }
    72% {
      transform: translate(-1.6px, 1.6px) rotate(2deg)
    }
    74% {
      transform: translate(1.3px, -1.6px) rotate(-0.5deg)
    }
    76% {
      transform: translate(1.4px, 1px) rotate(-0.5deg)
    }
    78% {
      transform: translate(-1px, 1.4px) rotate(2deg)
    }
    80% {
      transform: translate(1.4px, 1.6px) rotate(2deg)
    }
    82% {
      transform: translate(-1.6px, -1.6px) rotate(-0.5deg)
    }
    84% {
      transform: translate(-1.4px, 1.4px) rotate(-2deg)
    }
    86% {
      transform: translate(1px, 1.4px) rotate(-2deg)
    }
    88% {
      transform: translate(-1.4px, 1.4px) rotate(-1.5deg)
    }
    90% {
      transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    92% {
      transform: translate(-1.4px, 1.6px) rotate(2deg)
    }
    94% {
      transform: translate(-1.6px, -1.6px) rotate(-2deg)
    }
    96% {
      transform: translate(-1.4px, 1.3px) rotate(-2deg)
    }
    98% {
      transform: translate(1.3px, 1px) rotate(-0.5deg)
    }
  }
```

在主题的根目录下，`assert\css\_custom.scss` 中加入

```css
@import '_shaky.scss';

$blog-base-color: #e6e5f8 !default;

#header-desktop {
    body:not([data-theme='dark']) & {
        background-image: linear-gradient(180deg, lighten($blog-base-color, 5%), $blog-base-color);
    }

    &::after {
        content: '';
        position: absolute;
        top: 3.5rem;
        left: 0;
        width: MAX(360px, 10%);
        height: 110px;
        pointer-events: none;
        background-image: url(/images/drop.min.svg);
        // 背景图水平翻转
        transform: scaleX(-1);
        background-size: contain;
        background-position: top;
        background-repeat: no-repeat;

        [data-theme='dark'] & {
        background-image: url(/images/drop-dark.min.svg);
        }
    }
}

.github-corner svg {
  body:not([data-theme='dark']) & {
    fill: darken($blog-base-color, 25%);
  }
}

```

## 菜单增加自定义新的项

如果想在博客栏中新增一个“笔记”项，需要在博客根目录下hugo.toml中增加一项

```
  [[menu.main]]
    identifier = "notes"
    parent = ""
    pre = ""
    post = ""
    name = "笔记"
    url = "/notes/"
    title = ""
    weight = 5
    [menu.main.params]
      icon = "fa-solid fa-book"
```

然后需要在站点`content`下新建`notes`文件夹，并且在notes文件夹下创建`_index.md`，内容如下：

```
---
title: "笔记"
---
```

如果想在“笔记”下创建子项，比如“编程”：

```
  [[menu.main]]
    identifier = "notes-bc"
    parent = "notes"
    pre = ""
    post = ""
    name = "编程"
    url = "/notes/编程"
    title = ""
    weight = 6
    [menu.main.params]
      icon = "fa-solid fa-book"
```

同时，同上创建对应的子目录和\_index.md文件即可。