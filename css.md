# css题目
### 浏览器重构与重绘
> 浏览器渲染过程
1. http请求结束，浏览器拿到了html,生成了DOM树，解析Css生成Css树
2. html树与Css树结合生成render树（渲染树）
3. layout（回流）根据生成的渲染树，进行回流，得到每个节点的几何信息
4. painting(重绘) 根据渲染树以及回流得到的几何信息，得到节点的绝对像素
5. 将像素发给gpu，展示在页面上。

### 盒子模型
首先两种盒子模型
- W3C标准盒子模型，这是一种你设置了宽度只是content的盒子模型，border+padding会占据文档
- IE盒子模型，width就是width，border+padding只会压缩content并不会超过你预计的大小
设置两种：box-sizing: border-box/content-box

### 实现居中
无数的公司会问这个吧，我就写一些简单吧，
~~~css
/** 第一种 **/
.parent{
    display: flex;
    justify-content: center;
    align-items: center;
}
/** 第二种 **/
.parent{
    position: relative;
}
.child{
    position: absolute;
    top:0;
    bottom:0;
    left:0;
    right:0;
    margin:auto;
}
~~~
给个链接看吧[七种方式实现垂直居中](https://jscode.me/t/topic/1936)
关于为什么CSS上下居中这么麻烦（CSS回溯机制）
### BFC是什么？？？
BFC(block ormatting content)块级格式化上下文
- 举个例子
~~~ html
     <style>
        div{
            width: 300px;
            height: 300px;
            border: 1px solid red;
            margin: 100px;
            
        }
    </style>
    <div></div>
    <div></div>
~~~
两个margin会重叠，这就是bfc;想要去除给他们包裹起来，父级元素加上overflow:hidden;
### flex常用属性
#### 父级属性
- flex-direction 主轴方向
- flex-wrap 一行排序或者分行排序直接改变内部元素大小
- flex-flow 后面参数 *flex-direction* *flex-wrap*
- justify-content 主轴对其方式(居中/两端对齐/间隔相等)
- align-item 交叉轴上如何对齐
- align-centent 多根轴线（多行）的对齐方式。如果项目只有一根轴线
#### 项目属性
- order 排列顺序，默认都是0
- flex-grow 放大比例 等分区域
- flex-shrink 缩小比例
- flex-basis 分配空间之前占主轴的空间
- flex *flex-grow* *flex-shrink* *flex-basis*  可能这个会被问
### 写一个随着页面缩放的正方形
- width: 30%; height: 30vw;
### CSS选择器
- 越具体越好
- 写在后面的盖住前面的
- ！ important少写
### 清除浮动
- overflow:hidden
- ~~~css
  clearfix::after{
      content: '';
      display: block;
      clear:both;
  }
  ~~~