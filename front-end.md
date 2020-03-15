### 前端面试准备
#### 大体
```
    HTML&CSS：  对Web标准的理解、浏览器内核差异、兼容性、hack、CSS基本功：布局、盒子模型、选择器优先级及使用、HTML5、CSS3、移动端适应。
 
    JavaScript：   数据类型、面向对象、继承、闭包、插件、作用域、跨域、原型链、模块化、自定义事件、内存泄漏、事件机制、异步装载回调、模板引擎、Nodejs、JSON、ajax等。
    
    其他：  HTTP、安全、正则、优化、重构、响应式、移动端、团队协作、可维护、SEO、UED、架构、职业生涯 
```
#### 细分
+ cookie的弊端
    + IE7和firefox最多只能存放50条cookie（Chrome和safari没有做硬性限制）
    + Opera会清理最近最少使用的cookie，firefox会随机清理4096字节（1M =1024 KB,1KB = 1024Byte-字节,1Byte = 8bit-位）
    + 优点
        + 通过编程可以控制保存在cookie中的session对象大小
        + 通过加密和安全传输技术（ssl），减少了cookie被破解的可能性
        + 只在cookie放不敏感数据被盗也不会有太大损失
        + 控制生命周期，偷盗者拿到的cookie可能是过期的
    + 缺点
        + `Cookie`数量和长度的限制。每个domain最多只能有20条cookie，每个cookie长度不能超过4KB，否则会被截掉。
        + 安全性问题。如果cookie被人拦截了，那人就可以取得所有的session信息。即使加密也与事无补，因为拦截者并不需要知道cookie的意义，他只要原样转发cookie就可以达到目的了。
        + cookie紧跟域名，同一域名下的请求都会带上cookie，大量调用cookie是对性能的极大浪费
+ css相关
    + display:none和visiblity:hidden的区别
        + display隐藏元素的同时不在分配空间
        + visiblity隐藏元素但是仍然保留空间
    + css中link和@import区别
        + link属于HTML标签，而@import是CSS提供的; 
        + 页面被加载的时，link会同时被加载，而@import引用的CSS会等到页面被加载完再加载;
        + import只在IE5以上才能识别，而link是HTML标签，无兼容问题; 
        + link方式的样式的权重 高于@import的权重
    + position中的absolute和fixed的共同点和不同点
        + 共同点：
            + 改变行内元素的呈现方式，display被置为block；
            + 让元素脱离普通流，不占据空间；
            + 默认会覆盖到非定位元素上
        + 不同点：
            + absolute的”根元素“是可以设置的，而fixed的”根元素“固定为浏览器窗口。当你滚动网页，fixed元素与浏览器窗口之间的距离是不变的。  
    + 盒子模型
        + 有两种， IE 盒子模型、标准 W3C 盒子模型；IE的content部分包含了 border 和 pading;
        + 盒模型： 内容(content)、填充(padding)、边界(margin)、 边框(border).
    + CSS 选择符有哪些？哪些属性可以继承？优先级算法如何计算？ CSS3新增伪类有那些？
        + 选择器：
            + id选择器（ # myid）
            + 类选择器（.myclassname）
            + 标签选择器（div, h1, p）
            + 相邻选择器（h1 + p）
            + 子选择器（ul > li）
            + 后代选择器（li a）
            + 通配符选择器（ * ）
            + 属性选择器（a[rel = "external"]）
            + 伪类选择器（a: hover, li:nth-child）
        + 可继承的样式： font-size font-family color, text-indent;
 
        + 不可继承的样式：border padding margin width height ;
 
        + 优先级就近原则，同权重情况下样式定义最近者为准;载入样式以最后载入的定位为准;
            + 优先级为:!important >  id > class > tag  
            + important 比 内联优先级高,但内联比 id 要高
        + CSS3新增伪类举例：
            ```
                p:first-of-type 选择属于其父元素的首个 <p> 元素的每个 <p> 元素。
                p:last-of-type  选择属于其父元素的最后 <p> 元素的每个 <p> 元素。
                p:only-of-type  选择属于其父元素唯一的 <p> 元素的每个 <p> 元素。
                p:only-child    选择属于其父元素的唯一子元素的每个 <p> 元素。
                p:nth-child(2)  选择属于其父元素的第二个子元素的每个 <p> 元素。
                :enabled  :disabled 控制表单控件的禁用状态。
                :checked        单选框或复选框被选中。
            ```
        + 列出display的值说明他们的作用，position的值，relative和absolute分别是相对于谁定位
            ```
                1.   
                block 像块类型元素一样显示。
                inline 缺省值。像行内元素类型一样显示。
                inline-block 像行内元素一样显示，但其内容象块类型元素一样显示。
                list-item 像块类型元素一样显示，并添加样式列表标记。
                
                2. 
                *absolute 
                        生成绝对定位的元素，相对于最近一个已经定位的元素进行定位，如果没有就算body
                
                *fixed （老IE不支持）
                        生成绝对定位的元素，相对于浏览器窗口进行定位。 
                
                *relative 
                        生成相对定位的元素，（根据偏移量）相对于其在普通流中的位置（即自身位置）进行定位。 
                
                * static  默认值。没有定位，元素出现在正常的流中
                *（忽略 top, bottom, left, right z-index 声明）。
                
                * inherit 规定从父元素继承 position 属性的值。
            ```
        + css3新特性
            + CSS3实现圆角（border-radius），阴影（box-shadow），
            + 对文字加特效（text-shadow、），线性渐变（gradient），旋转（transform）
            + transform:rotate(9deg) scale(0.85,0.90) translate(0px,-30px) skew(-9deg,0deg);//旋转,缩放,定位,倾斜
            + 增加了更多的CSS选择器  多背景 rgba 
            + 在CSS3中唯一引入的伪元素是::selection.
            + 媒体查询，多栏布局
            + border-image
        + 为什么要初始化样式
            ```
                因为浏览器的兼容问题，不同浏览器对有些标签的默认值是不同的，如果没对CSS初始化往往会出现浏览器之间的页面显示差异。
 
                当然，初始化样式会对SEO有一定的影响，但鱼和熊掌不可兼得，但力求影响最小的情况下初始化。
            
                *最简单的初始化方法就是： * {padding: 0; margin: 0;} （不建议）
            
                淘宝的样式初始化： 
                body, h1, h2, h3, h4, h5, h6, hr, p, blockquote, dl, dt, dd, ul, ol, li, pre, form, fieldset, legend, button, input, textarea, th, td { margin:0; padding:0; }
                body, button, input, select, textarea { font:12px/1.5tahoma, arial, \5b8b\4f53; }
                h1, h2, h3, h4, h5, h6{ font-size:100%; }
                address, cite, dfn, em, var { font-style:normal; }
                code, kbd, pre, samp { font-family:couriernew, courier, monospace; }
                small{ font-size:12px; }
                ul, ol { list-style:none; }
                a { text-decoration:none; }
                a:hover { text-decoration:underline; }
                sup { vertical-align:text-top; }
                sub{ vertical-align:text-bottom; }
                legend { color:#000; }
                fieldset, img { border:0; }
                button, input, select, textarea { font-size:100%; }
                table { border-collapse:collapse; border-spacing:0; } 
            ```
        + 对于BFC规范的理解
            ```
                BFC，块级格式化上下文，一个创建了新的BFC的盒子是独立布局的，盒子里面的子元素的样式不会影响到外面的元素。在同一个BFC中的两个毗邻的块级盒在垂直方向（和布局方向有关系）的margin会发生折叠。
                （W3C CSS 2.1 规范中的一个概念，它决定了元素如何对其内容进行布局，以及与其他元素的关系和相互作用。）
            ```
        + 解释css sprites，如果再网站中使用它
            ```
                CSS Sprites其实就是把网页中一些背景图片整合到一张图片文件中，再利用CSS的“background-image”，“background- repeat”，“background-position”的组合进行背景定位，background-position可以用数字能精确的定位出背景图片的位置。这样可以减少很多图片请求的开销，因为请求耗时比较长；请求虽然可以并发，但是也有限制，一般浏览器都是6个。对于未来而言，就不需要这样做了，因为有了`http2`。(啥是http2捏？)
            ```
+ html部分
    + 对于语义化的理解
        + 去掉或者丢失样式的时候能够让页面呈现出清晰的结构
        + 有利于SEO：和搜索引擎建立良好沟通，有助于爬虫抓取更多的有效信息：爬虫依赖于标签来确定上下文和各个关键字的权重；
        + 方便其他设备解析（如屏幕阅读器、盲人阅读器、移动设备）以意义的方式来渲染网页；
        + 便于团队开发和维护，语义化更具可读性，是下一步吧网页的重要动向，遵循W3C标准的团队都遵循这个标准，可以减少差异化。
    + Doctype作用，严格模式和混杂模式的区分，它们有何意义
        + <!DOCTYPE> 声明位于文档中的最前面，处于 <html> 标签之前。告知浏览器以何种模式来渲染文档。 
        + 严格模式的排版和 JS 运作模式是  以该浏览器支持的最高标准运行。
        + 在混杂模式中，页面以宽松的向后兼容的方式显示。模拟老式浏览器的行为以防止站点无法工作。
        + DOCTYPE不存在或格式不正确会导致文档以混杂模式呈现。
    + html和xhtml区别
        ```
            区别：
            1.所有的标记都必须要有一个相应的结束标记
            2.所有标签的元素和属性的名字都必须使用小写
            3.所有的XML标记都必须合理嵌套
            4.所有的属性必须用引号""括起来
            5.把所有<和&特殊符号用编码表示
            6.给所有属性赋一个值
            7.不要在注释内容中使“--”
            8.图片必须有说明文字
        ```   
    + 解释浮动的工作原理，清除浮动的技巧
        ```
            浮动元素脱离文档流，不占据空间。浮动元素碰到包含它的边框或者浮动元素的边框停留。

            1.使用空标签清除浮动。
            这种方法是在所有浮动标签后面添加一个空标签 定义css clear:both. 弊端就是增加了无意义标签。
            2.使用overflow。
            给包含浮动元素的父标签添加css属性 overflow:auto; zoom:1; zoom:1用于兼容IE6。
            3.使用after伪对象清除浮动。
            该方法只适用于非IE浏览器。具体写法可参照以下示例。使用中需注意以下几点。
            一、该方法中必须为需要清除浮动元素的伪对象中设置 height:0，否则该元素会比实际高出若干像素；
        ```
    + 浮动元素引起的问题和解决办法
        ```
            浮动元素引起的问题：
            （1）父元素的高度无法被撑开，影响与父元素同级的元素
            （2）与浮动元素同级的非浮动元素会跟随其后
            （3）若非第一个元素浮动，则该元素之前的元素也需要浮动，否则会影响页面显示的结构

            (2)(3)增加额外标签<div style="clear:both;"></div>
            （1）clearfix
        ```
    + html5有哪些新特性、移除了那些元素？如何处理HTML5新标签的浏览器兼容问题？如何区分 HTML 和 HTML5？
        ```
            * HTML5 现在已经不是 SGML 的子集，主要是关于图像，位置，存储，多任务等功能的增加。
 
            * 拖拽释放(Drag and drop) API 
            语义化更好的内容标签（header,nav,footer,aside,article,section）
            音频、视频API(audio,video)
            画布(Canvas) API
            地理(Geolocation) API
            本地离线存储 localStorage 长期存储数据，浏览器关闭后数据不丢失；
            sessionStorage 的数据在浏览器关闭后自动删除
            
            表单控件，calendar、date、time、email、url、search  
            新的技术webworker, websocket, Geolocation
            
            * 移除的元素
            
            纯表现的元素：basefont，big，center，font, s，strike，tt，u；
            
            对可用性产生负面影响的元素：frame，frameset，noframes；
            
            支持HTML5新标签：
            
            * IE8/IE7/IE6支持通过document.createElement方法产生的标签，
            可以利用这一特性让这些浏览器支持HTML5新标签，
            
            浏览器支持新标签后，还需要添加标签默认的样式：
            
            * 当然最好的方式是直接使用成熟的框架、使用最多的是html5shim框架
            <!--[if lt IE 9]> 
            <script> src="http://html5shim.googlecode.com/svn/trunk/html5.js"</script> 
            <![endif]--> 
            如何区分： DOCTYPE声明\新增的结构元素\功能元素
        ```
    + iframe优缺点
        + 优点：
            + 解决加载缓慢的第三方内容如图标和广告等的加载问题
            + 并行加载脚本
 
        + 缺点：
            *iframe会阻塞主页面的Onload事件；
            *即时内容为空，加载也需要时间
            *没有语意 
    + 对于网站资源的优化
        + 文件合并
        + 文件最小化/文件压缩
        + 使用 CDN 托管
        + 缓存的使用（多个域名来提供缓存）
    + 进程和线程的区别
        + 进程是执行中的一个程序，程序一旦被执行就是一个进程，单个进程中执行的任务就是线程，线程是进程执行运算的最小单位
        + 一个进程可以有多个线程，但是一个线程只属于一个进程
    + 减少页面加载时间的方式
        + 优化图片 
        + 图像格式的选择（GIF：提供的颜色较少，可用在一些对颜色要求不高的地方） 
        + 优化CSS（压缩合并css，如margin-top,margin-left...) 
        + 网址后加斜杠（如www.campr.com/目录，会判断这个“目录是什么文件类型，或者是目录。） 
        + 标明高度和宽度（如果浏览器没有找到这两个参数，它需要一边下载图片一边计算大小，如果图片很多，浏览器需要不断地调整页面。这不但影响速度，也影响浏览体验。 当浏览器知道了高度和宽度参数后，即使图片暂时无法显示，页面上也会腾出图片的空位，然后继续加载后面的内容。从而加载时间快了，浏览体验也更好了。） 
        + 减少http请求（合并文件，合并图片）。
    + json
        + JSON(JavaScript Object Notation) 是一种轻量级的数据交换格式。它是基于JavaScript的一个子集。数据格式简单, 易于读写, 占用带宽小
    + js延迟加载的方式
        + defer和async、动态创建DOM方式（创建script，插入到DOM中，加载完毕后callBack）、按需异步载入js
    + 状态码
        + 100-199 用于指定客户端应相应的某些动作。 
        + 200-299 用于表示请求成功。 
        + 300-399 用于已经移动的文件并且常被包含在定位头信息中指定新的地址信息。 
        + 400-499 用于指出客户端的错误。
            + 400语义有误，当前请求无法被服务器理解。
            + 401当前请求需要用户验证 
            + 403服务器已经理解请求，但是拒绝执行它。
        + 500-599 用于支持服务器错误。 503 – 服务不可用
    + 一个页面从输入url到显示完成经过了什么
        + 当发送一个URL请求时，不管这个URL是Web页面的URL还是Web页面上每个资源的URL，浏览器都会开启一个线程来处理这个请求，同时在远程DNS服务器上启动一个DNS查询。这能使浏览器获得请求对应的IP地址。
        + 浏览器与远程Web服务器通过TCP三次握手协商来建立一个TCP/IP连接。该握手包括一个同步报文，一个同步-应答报文和一个应答报文，这三个报文在 浏览器和服务器之间传递。该握手首先由客户端尝试建立起通信，而后服务器应答并接受客户端的请求，最后由客户端发出该请求已经被接受的报文。
        + 一旦TCP/IP连接建立，浏览器会通过该连接向远程服务器发送HTTP的GET请求。远程服务器找到资源并使用HTTP响应返回该资源，值为200的HTTP响应状态表示一个正确的响应。
        + 此时，Web服务器提供资源服务，客户端开始下载资源。
        +  请求返回后，便进入了我们关注的前端模块简单来说，浏览器会解析HTML生成DOM Tree，其次会根据CSS生成CSS Rule Tree，而javascript又可以根据DOM API操作DOM
    + http状态码
        ```
            100  Continue  继续，一般在发送post请求时，已发送了http header之后服务端将返回此信息，表示确认，之后发送具体参数信息
            200  OK   正常返回信息
            201  Created  请求成功并且服务器创建了新的资源
            202  Accepted  服务器已接受请求，但尚未处理
            301  Moved Permanently  请求的网页已永久移动到新位置。
            302 Found  临时性重定向。
            303 See Other  临时性重定向，且总是使用 GET 请求新的 URI。
            304  Not Modified  自从上次请求后，请求的网页未修改过。
            
            400 Bad Request  服务器无法理解请求的格式，客户端不应当尝试再次使用相同的内容发起请求。
            401 Unauthorized  请求未授权。
            403 Forbidden  禁止访问。
            404 Not Found  找不到如何与 URI 相匹配的资源。
            
            500 Internal Server Error  最常见的服务器端错误。
            503 Service Unavailable 服务器端暂时无法处理请求（可能是过载或维护）。
        ```
    + 剩下需要拓展的
        + ajax
        + 三大框架比较
        + js基础
    + 前端面试知识点
        + https://github.com/yisainan/web-interview