

源码使用webpack打包



三个文件引用的都是`lib`文件下的文件，执行下面一步提示的命令`npm insall`后就可以得到`lib`文件夹，它里面的文件和`src`文件夹中的文件主要内容是相同的，不同之处在于：前者文件是通过类似CMD的模式打包的，后者文件是通过webpack进行打包的。



```
npm install  //安装所有依赖包
webpack      //打包
webpack -p   //打成压缩包（.min.js）
```



## 两个重要的概念components和charts

charts是指各种类型的图表，例如line，bar，pie等，在配置项中指的是series对应的配置；components组件是在配置项中除了serie的其余项，例如title，legend，toobox等。



源码的重要目录及说明如下(注：dist为编译后生成的文件夹)

- extension （扩展中使用）
- lib （源码中没有，执行webpack编译后才存在）
- map （世界地图，中国地图及中国各个省份地图的js和json两种格式的文件）
- src （核心源码）
- test （示例demo）
- theme （主题）



# 2 渲染情况

最外层是id为main的div，是我们自己写的用来渲染echarts图表的。
echarts渲染了两个div，一个div用来渲染主要的图表的，div里面嵌套一个canvas标签，
第二个div是为了显示hover层信息的。



# 3.入口`echarts.js`



这里介绍下源码里面是怎样管理配置项的。主要源码在echarts/model/Model（以下简称Model），echarts/model/Global（以下简称GlobalModel，继承Model），echarts/model/OptionManager（以下简称OptionManager）



- Model模块是一些基本的方法，主要的方法就是`get`,`getModel`通过options的对象名获取对象值。还混合了lineStyle，areaStyle，textStyle,itemStyle方法用来管理与线，文本，项目有关的options属性。
- GlobalModel继承Model，暴露Model的方法，再封装一些自己独有的方法。
- OptionManager是用来管理options配置项的，有重要的`setOption`方法，`mergeOption`方法(私有方法，合并options），`parseRawOption`方法（私有方法，解析options）



对外API

```
ExtensionAPI
```

6.事件



我们知道canvas API没有提供监听每个元素的机制，这就需要一些处理。处理的思路是：监听事件的作用坐标（如点击时候的坐标），判断在哪个绘制元素的范围中，如果在某个元素中，这个元素就监听该事件。



## links

 ECharts 3.0源码简要分析1-总体架构 [https://zrysmt.github.io/2017/03/09/ECharts%203.0%E6%BA%90%E7%A0%81%E7%AE%80%E8%A6%81%E5%88%86%E6%9E%901-%E6%80%BB%E4%BD%93%E6%9E%B6%E6%9E%84/](https://zrysmt.github.io/2017/03/09/ECharts 3.0源码简要分析1-总体架构/



echarts源码解读《三》：echarts源码之Component分析 http://qiuruolin.cn/2019/05/22/echarts-3/