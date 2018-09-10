# AJAX快速上手 #

![](imgs/2018-09-08_102654.png)

![](imgs/2018-09-08_104140.png)

## http协议 ##

![](imgs/2018-09-08_105035.png)

**请求头的格式:**

![](imgs/2018-09-08_105224.png)

**对xx进行编程,意思就是让代码去操作xxx**

## 计算一个js函数执行的时候花了多长时间 ##

console.time('name');      // name是计数器的名字,可以随便写一个
for (let i=0;i<arr.lenght;i++){};
console.timeEnd('name');

## 统计同步和异步请求所花费的时间 ##

![](imgs/2018-09-08_191214.png)

格式化json数据:   var jsonData=JSON.parse(data);

## 模板引擎 ##

![](imgs/2018-09-09_111359.png)

## ajvax的基本封装 ##

![](imgs/2018-09-09_114208.png)

## jquery 对ajax的封装 ##

![](imgs/2018-09-09_165835.png)

jquery封装的参数中,datatype是用于设置后台给前台返回数据中响应体的内容(也就是content-type的格式,默认是content-type:application/html),如果content-type:application/json,则返回的数据格式为json格式的.

![](imgs/2018-09-09_170719.png)

**jquery中ajax的一些回调函数说明**

![](imgs/2018-09-09_171540.png)

## 如何制作loading中的样式?? ##

![](imgs/2018-09-09_182503.png)

![](imgs/2018-09-09_183012.png)

**特别注意一下这个load方法(),类似于$.get(),但是load(),一般只用一个地址.**

![](imgs/2018-09-09_183742.png)

上面的图片加载的是load()中URL页面中的地址,

![](imgs/2018-09-09_184327.png)

## 如何使用nprogress进度? ##

先引进css和JS文件

![](imgs/2018-09-09_185459.png)

然后,直接使用

![](imgs/2018-09-09_185721.png)

# 有光ajax的跨域请求 #

## ===>实质就借助于script标签可以发送不同源的请求的这么一个特性来完成的 ##

后台代码以一个js的形式返回,

![](imgs/2018-09-10_224707.png)

前台接收到了之后,用js的形式解析他

![](imgs/2018-09-10_224601.png)

## jsonp 相关技术封装 ##

![](imgs/2018-09-10_230639.png)

**完整版本的jsonp封装函数**

![](imgs/2018-09-10_232632.png)

## 借助ajax发送jsonp请求 ##

![](imgs/2018-09-10_232358.png)




















