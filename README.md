# 前言
此为记录贴，尽量一天一记。
[toc]
# 2020年8月
## 2020-8-16
- easy:
```javascript
 //阅读以下代码，问foo.count的值为？
        function foo() {
            this.count++;
        }
        foo.count = 0;
        for (var i = 0; i < 10; i++) {
            if (i % 5) {
                foo()
            }
        }
```
> 首先，答案是0。这里的this指向以及for循环判断都是拿来迷惑的，foo.count的值一直没变过。最初对于函数上设置属性很迷惑，查询资料得知，js中有句话叫“万物皆对象”，所以既然是对象就可设置属性，这里就牵扯出了静态属性暂且不提。若要for循环对foo上的count操作，可将其循环内设为foo.call(foo);
- normal:
什么是XSS攻击？
>（触及知识点盲区，浅显回答，以后再做修整）  
 xss攻击的中文名为“跨站脚本攻击”。指的是，恶意攻击者利用开发人员开发时留下的漏洞，利用其不对用户提交的数据进行处理，在提交时插入恶意代码，从而达到攻击用户的目的。具体表现在盗取cookie信息、流量劫持、利用iframe或XMLHttpRequest，以被攻击着（用户）的身份执行一些管理动作，如发微博，加好友等、利用js或css破坏页面正常结构等行为上。
   流程通常为：寻找漏洞->构建攻击代码->攻击用户->寻找输出点->攻击完成
   防御措施：为用户提交的数据进行编码处理。

- 小demo:
```javascript
//倒计时功能（简陋）
    <div id="app"></div>
    <button onclick="stop()">停止</button>
    <script>
        var dom = document.getElementById('app');
        var timer = null;
        function createDealTime() {
            dom.innerHTML = '';
            var now = new Date(); //格式为 Sun Aug 16 2020 21:46:59 GMT+0800 (中国标准时间)
            var to = new Date(2020, 7, 17, 20, 56)
            var num = to - now;
            var str = '';
            // 计算出差值的时分秒
            var h = Math.floor(num / (60 * 1000 * 60));
            var m = Math.floor(num / (1000 * 60) % 60);
            var s = Math.floor(num / 1000 % 60);
            var ms = Math.floor(num % 1000 / 10);
            var str = (h < 10 ? '0' + h : h) + ':' + (m < 10 ? '0' + m : m) + ':' + (s < 10 ? '0' + s : s) + ':' + (ms <
                10 ? '0' + ms : ms);
            dom.innerHTML = str;
            timer = requestAnimationFrame(createDealTime)
        }
        createDealTime();
        function stop() {
            cancelAnimationFrame(timer)
        }
    </script>
```
## 2020-8-17
- easy:
```javascript
    //  下面代码执行的结果是什么？
    function Car(){
        this.name='Lamborghini';
        return {
            name:'Maserati'
        }
    }
    var myCar = new Car();
    console.log(myCar.name)
```
> 答案是 Maserati。构造函数经过New调用会发生一下几个步骤：1，创建一个新对象obj；2，obj的proto指向构造函数的prototype；3，将构造函数的作用域赋值给新对象，因此this指向新对象；4,返回新对象obj。正常情况下在构造函数中不推荐写返回值，如果return值类型，则无影响，如果return引用类型，那么实例化对象就会返回该引用类型。没人这么写，算是一个注意点，让我们平时不要轻易在构造函数中使用 return xxx;

- normal:
什么是DDoS攻击？
> 首先先了解什么是Dos攻击，DoS(拒绝服务，Denial of Service)就是利用合理的服务请求来占用过多的服务资源，从而使合法用户无法得到服务的响应。就类型一个恶霸去饭店找茬，多次招呼服务员做这做那，使其没办法再去招呼其他客人。DDoS（分布式拒绝服务）攻击，是指攻击者利用大量“肉鸡”对攻击目标发动大量的正常或非正常请求，耗尽目标主机资源或网络资源，从而使被攻击的主机不能为正常用户提供服务。类似于一大群恶霸去饭店找茬。一个无赖去胡闹，就是 DoS 攻击，而一群无赖去胡闹（分布式），就是 DDoS 攻击。

- 小demo:

```javascript
//css暗黑模式适配
//html部分
    <div>12345</div>
    <a href="#">百度</a>
    <img src="./lyf.jpg" alt="">
// css部分
    <style>
        html {
            background-color: #fff;
            transition: color .2s,background-color .2s ;
        }
        img{
            width:200px;
            height:400px;
        }
        a {
            text-decoration: none;
        }
        @media(prefers-color-scheme:dark) {
            html {
                filter: invert(1) hue-rotate(180deg);
            }
            html img {
                filter: invert(1) hue-rotate(180deg);
            }
        }
    </style>
```
> filter滤镜，invert()为翻转输入图像，值为1或100%时，黑变白，白变黑；hue-rotate()给图像应用色相旋转,超过360deg相当于绕了一圈

## 2020-8-18
- easy:
```javascript
        //下面代码会输出什么结果？
        var a=1;
        var obj = {
            a:2,
            method:{
                a:3,
                A:function(){
                    return this.a;
                }
            }
        };
        var m=obj.method.A;
        console.log(m());
```
> 答案是 1。this指向问题，全局及函数预编译中，this指向window.

- normal:
什么是CSRF攻击?
> CSRF(跨站请求伪造)，利用cookie的便利，利用受信任用户的请求来利用受信用的网站。利用用户在A网站登录没有退出，在B网站点击了一个其实是A网站的一个链接，这个链接是支付链接且带好了传参值，然后由于浏览器有A网站的cookie，所以就在用户不知情的情况下进行了误支付。 所以它利用的是 cookie的自动携带特性以及跨站攻击，可以从这角度得出防范措施：1、检查Referer字段，HTTP头中的这个字段用于表明请求来源于哪个地址，由此可拒绝一切非本站发出的请求；2、Token验证，由于 CSRF 是利用了浏览器自动传递 cookie 的特性，另外一个防御思路就是校验信息不
通过 cookie 传递，在其他参数中增加随机加密串进行校验。

- 小demo:
```javascript
    // 分页插件
    //css部分
        .active {
            width: 40px;
            height: 40px;
            line-height: 40px;
            border: 1px solid red;
            display: inline-block;
            text-align: center;
            cursor: pointer;
        }
    //js部分
        function showPage(nowPage, pageSize, total) { //pageSize:每页数据条数,total:总数据条数
            var arr = [];
            arr.push({
                text: '首页',
                page: 1
            });
            if (nowPage > 3) {
                arr.push({
                    text: '···',
                    page: nowPage - 3
                })
            }
            if (nowPage > 2) {
                arr.push({
                    text: nowPage - 2,
                    page: nowPage - 2
                })
            }
            if (nowPage > 1) {
                arr.push({
                    text: nowPage - 1,
                    page: nowPage - 1
                })
            }
            arr.push({
                text: nowPage,
                page: nowPage
            });
            if (nowPage + 1 < Math.floor((total + pageSize - 1) / pageSize)) {
                arr.push({
                    text: nowPage + 1,
                    page: nowPage + 1
                })
            }
            if (nowPage + 2 < Math.floor((total + pageSize - 1) / pageSize)) {
                arr.push({
                    text: nowPage + 2,
                    page: nowPage + 2
                })
            }
            if (nowPage + 2 < Math.floor((total + pageSize - 1) / pageSize)) {
                arr.push({
                    text: '···',
                    page: nowPage + 3
                })
            }
            arr.push({
                text: '尾页',
                page: Math.floor((total + pageSize - 1) / pageSize)
            })
            return arr;
        }
        //创建一个页码
        function createEle(obj) {
            var div = document.createElement('div');
            div.className = 'active';
            div.innerText = obj["text"];
            //给每个页码绑定一个事件用作切换页码，并可在以后添加ajax请求获取每页的数据。
            div.onclick = function () {
                document.body.innerHTML = '';
                let nowArr = showPage(obj.page, 5, 1400)
                nowArr.forEach(item => {
                    createEle(item);
                })
            }
            document.body.appendChild(div)
        }
        //生成完整页码
        var myArr = showPage(8, 5, 1400) //当前页为8，每页5条数据，总共1400条数据
        myArr.forEach(item => {
            createEle(item);
        })
```
> 原本想在vue中写这个插件的，结果一时想不开就写了原生的。就写了个基本框架，目前搜索没做以及省略功能辣眼睛，就练练手也不想搞太多，就先这样吧，不能重复造轮子！

## 2020-8-19
- easy:
```javascript
    // 下面代码输出什么
        var arr =[1,2,3,4];
        var str='123asd'
        arr.length=1;
        str.length=1;
        console.log(arr,str)
```
> 答案是 [1] '123asd'。length会对arr起作用，但不会对原始值起作用，原始值没有属性与方法，但我们能调用他们是因为包装类，例如 var str='asd',str.length=1;
这会在内部执行 new String(asd).length = 1,然后删除，最后得到的str依旧是'asd'没有改变。

- normal:
setTimeout与requestAnimationFrame的区别？
> 1，由于Js的事件执行机制的问题，利用setTimeout可能会造成刷新时间不一致的情况，而requestAnimationFrame则会根据显示屏的频率让页面重绘的频率与
这个刷新频率保持同步；2，setTimeout属于JS引擎，requestAnimationFrame属于GUI引擎；3性能层面：当页面被隐藏或最小化时，定时器 setTimeout 仍在后台执行动画任务。而对于requestAnimationFrame来说，当页面处于未激活的状态下，该页面的屏幕刷新任务会被系统暂停，requestAnimationFrame 也会停止；

- 小demo：
```javascript
    //今天有点忙，事情还一堆没做完，所以就先投个懒，就写个mysql查询语句。下午因为这条语句有问题，为了解决这个Bug,足足花了四个多小时！！！
    //mysql模糊查询在node中的书写方式,属于dao层。
    function querySome(search,success){
        var selectSql = "select * from blog where title like concat('%',?,'%') or content like concat('%',?,'%');"
        var params = [search,search];
        // 下面就是mysql的创建连接，.connect(), .query(), .end();
    ···
    }
```

## 2020-8-20
- easy：
```javascript
   //下面代码执行的结果是什么
    function getInfo(per,age){
        per.name='c1';
        age=17;
    
    }
    var newPer={name:'c2'};
    var newAge=18;
    getInfo(newPer,newAge);
    console.log(newPer,newAge)
```
> 答案是 {name:'c1'} 18。利用原始值与引用值传值的区别，引用值传递的是地址，原始值传递的时候会覆盖形参，但函数里面的age则是函数作用域上的参数，与外面的newAge没关系。

-normal：
在地址栏中输入一个url按下回车后发生了什么？
>(先写个简单的，过两天周末时间多再来展开) 简单流程就是 先进行DNS解析->TCP三次握手->http传输->服务器接收数据并返回->四次握手关闭传输->浏览器端解析渲染render树（回流、重构）。
DNS解析是将域名解析成IP地址，首先会在浏览器缓存里找，如果没有则向本机host找，再没有则向上一级DNS服务器找，直到被找到，找到后悔进行缓存。接下来就是请求数据前的验证，TCP三次握手，先是客户端告诉服务器我要请求数据，发送一个标志码(syn(发起一个新连接))，服务器收到标志码并返回一个标志码(syn + ack)，说好的我收到请求了，你准备好了发过来吧，第三次握手则是客户端收到服务器的标志码并返回(ack(确认序号有效))，告诉服务器我知道你准备好了，接下来就发送数据了。（剩下的下次补。。。）

- 小demo：
```javascript
    //mock的基本使用（生成随机数，拦截ajax请求）
    //npm install mockjs
    var Mock = require('mock')
    //Mock.mock({id|+1:1}) Mock.random.date()
   Mock.mock('./data.json'，{
        "status"："success"，
        "msg":"ok",
        "data|10":[{
            "id|+1":1,
            "name":"@cname",
            "sex|1":"['男','女']"，
            "birth":"@date",
            "sNo|+1":11000，
            "phone":"@natural(13000000000,19900000000)",
            "address":"@country(true)",
            "ctime":"@date(T)"
        }]
    })
```

## 2020-8-21
-easy:
```javascript
    // 下面代码的执行结果是什么？
    var list =[1+2,1*2,1/2]
    console.log(list)
```
> 答案是[3,2,0.5]

- normal:
axios如何实现并发？（笔试竟然在这栽了，气晕，记下来）
```javascript
    axios.all([axios.get('url1'),axios.get('url2')])
          .then(axios.spread((resp1,resp2) => {
              console.log(resp1,resp2)
          }))
```

- 小demo：
```javascript
   // 性能优化之防抖？
    // 防抖就是为了防止在一段时间内重复一段代码（函数），目的是控制次数。思路：给函数设一个时间，让它在n秒后再执行，在这n秒内再次被触发则会重置时间。
    /*
    *   fn 为执行的函数,delay为延时，flag为状态标志，用作标记是否第一次要立即执行
    */
    function debounce(fn,delay,flag){
        let timer;
        return function(...args){
            timer && clearTimeout(timer);
            let that = this;
            if(flag){
                flag = false;
                fn.apply(that,args)  //这里可以直接调用fn(args)，这里这么写是为了提示下面的fn使用如果没用箭头函数则要绑定this。
            }
            timer = setTimeout(()=>{
                fn(args)
            },delay)
        }
    }
    var inp = document.getElementById('inp');
      
    var handler=debounce(show,500,true);

    inp.addEventListener('input',function(){
        let val = this.value;
        // console.log('ok')
       handler(val)
    })
    function show(val){
        console.log('a')
    }

```

## 2020-8-22
- easy:
```javascript
  // 下面代码的执行结果是什么？
        var one = (false || {} || null); 
        var two = (null || false || ""); 
        var three = ([] || 0 || true); 
        console.log(one,two,three)
```
> 答案是 {}，"",[]。逻辑运算符中，false、null、undefined、""、0、NaN这六种boolean判断为false，其他全为true。而对于（表达式1 || 表达式2）这种来说，若符号为&&，若表达式1为假，则返回表达式1的结果，且不执行表达式2，反之，返回表达式2的结果。若符号为||，若表达式1为真，则返回表达式1的结果，不执行表达式2，反之返回表达式2的结果

- normal:
什么是reflow回流？什么是repaint重绘？
> 回流指的是根据 render tree 布局，元素的尺寸、位置、内容、结构发生了变化，需要重新计算样式属性及渲染树。 重绘指的是元素的改变只影响了节点的一点样式（背景色、边框色、字体颜色），只需要应用新样式绘制这个元素就可以了。
引起回流的操作： 1. 页面第一次渲染。2.DOM树变化，如增删节点。3.Render树改变，如padding改变。4.浏览器窗口resize。5.获取元素的某些属性，如 offsetWidth,scrollTop、调用getComputed()、或者IE的currentStyle等。
引起重绘的操作：1.回流必定引起重绘。2.背景色、颜色，字体改变（大小改变时会发生回流）。
优化回流、重绘次数：1.避免逐个修改样式，最好一次性修改。2.可以将需要多次修改的DOM元素设置为display:none，操作完再显示（因为隐藏元素不在render树内）。3.避免多次读取某些属性
延伸：经常说操作DOM很耗资源，其实操作DOM的具体成本，说到底是造成浏览器回流以及重绘，从而消耗GPU资源。

- 小demo：
```javascript
  // 性能优化之节流？（这里纠结了三四个小时，因为查阅资料得知有不同的写法，总想需求一个完美的解决函数，
  //但最后才发现并没有完美函数，只能挑选更适合的函数去使用。算是给自己提个醒吧，想得出完美的办法再去实施是不可行的！！！！）
        function throttle(fn, delay, immediate) {
            if (immediate) { //是否第一次时立即执行一次
                let t; //t为上一次执行的时间
                return function () {
                    if (!t || Date.now() - t > delay) {
                        fn();
                        t = Date.now()
                    }
                }
            } else {
                let timer;
                return function () {
                    if (timer) {
                        return
                    }
                    timer = setTimeout(() => {
                        fn()
                        timer = null; //计时器到时见执行后得把timer值修改，否则会一直进入if判断里
                    }, delay)
                }
            }

        }
        function show() {
            console.log('a')
        }
        var handle = throttle(show, 1000,false)
        window.onresize = function () {
            handle()
        }

        //下面为另一种方法，对于input来说会显示最后一次输入的值，即便没达到间隔时间。但由于有clearTimeout操作，所以若是在结束时还没达到delay，则第一次的显示时间会变长。
        

        <input type = "text"id = "input" />
            let ipt = document.getElementById('input');

        let handler = throttle(handleSendPhone, 1000);

        ipt.addEventListener('input', function () {
            let val = this.value;
            handler(val);
        });

        // 调用的函数
        function handleSendPhone(val) {
            console.log(val)
        }

        /**
         * @fn : 要执行的函数
         * @delay : 每次执行函数的时间间隔
         */
        function throttle(fn, delay) {
            let timer;
            let prevTime; // 记录上一次执行的时间
            return function (...args) {
                let currTime = Date.now(); // 获取当前时间时间戳
                let context = this;
                if (!prevTime) prevTime = currTime; // 第一次执行时prevTime赋值为当前时间

                clearTimeout(timer); // 每次都清除定时器，保证定时器只是在最后一次执行

                if (currTime - prevTime > delay) { // 如果为true ，则表示两次执行函数的时间间隔为delay.
                    prevTime = currTime;
                    fn.apply(context, args);
                    clearTimeout(timer); // 清除定时器。用来处理假如函数停止调用时刚好函数也停止执行，不需要获取后续的值。 详见下面定时器的介绍。
                    return;
                }
                // 当上面执行currTime - prevTime > delay 为false时，执行定时器。
                // 用来处理：假如下次函数执行时间未到，函数不继续调用了，会造成最后一次函数执行 到 最后一次函数调用之间的值获取不到。
                timer = setTimeout(function () {
                    prevTime = Date.now();
                    timer = null;
                    fn.apply(context, args);
                }, delay);
            }
        }
```

## 2020-8-23
- easy:
通过类名获取元素的api？
> document.getElementByClassName('xx')、document.querySelect('.xx')。

- normal:
浏览器的渲染过程?
> 1.解析HTML，构建DOM树（此时遇见外链，会发起请求）。2.解析CSS，生成CSS规则树。3.合并CSS树与HTML树，生成Render树。4.布局render树（layout、reflow）。5.绘制render树(paint)。6.浏览器会将各层的信息发送给GPU，GPU将各层合成，显示在屏幕上。

- 小demo:
```javascript
//导出一个深克隆函数
export default function clone(obj){
    if(obj instanceof Array){
        return arrayClone(obj)
    }
    else if(obj instanceof Object){
        return objClone(obj)
    }else{
        return obj
    }
}

function arrayClone(obj){
    var arr = [];
    for(var i=0;i< obj.length;i++){
        arr[i] = clone(obj[i])
    }
    return arr;
}

function objClone(obj){
    var newObj={};
    for(var prop in obj){
        newObj[prop] = clone(obj[prop])
    }
    return newObj;
}
```
## 2020-8-24
- easy:
```javascript
    //下面代码输出的结果是什么?
    console.log(123..toString())
```
> 答案是 '123'，字符串类型的。这里面有两个点不常见，由于浏览器的机制，这种直接数字调用不清楚第一个点是小数点还是对象属性调用这种，所以需要两个点，它会把123..toString()当做123.0.toString(),也就是说前面的是数字123.0。

- normal:
什么是继承？
> 继承就是copy和复用，让一个对象拥有另一个对象的属性和方法即可。   实现：1.for·in循环对象，依次赋值。2.利用prototype属性，构建原型链。3.Object.assign()方法。

- 小demo:
```javascript
    //聊天框分别位居左右两边布局,这里记一笔主要是因为这个布局启动浮动后会产生左右隔行布局，除了给上一级清浮动外，给上上级flex-column也可以
       /* <div class="container">
            <div class="wrap" v-for="(item,index) in list" :key="index">
                <div class="right box" v-if="index % 2 ==0">
                    {{item}}
                </div>
                <div class="left box "v-else>
                    {{item}}
                </div>
            </div>
        </div>

         list: ['话1', '话2', '话3', '话4', '话5', '话6', '话7']
            .container {
        width: 100%;
        display: flex;
        flex-direction: column;
    }
    .box {
        max-width: 80%;
        height: 30px;
        line-height: 30px;
        text-align: left
    }
    .right {
        float: right;
        background-color: aqua
    }
  .left {
        float: left;
        background-color: aquamarine
    }
    */
   //或者不用flex布局，用clear
        <div class="containerBox">
            <!-- 显示聊天内容 -->
            // vue
            <div v-for="(item,index) in allContentList" :key="index" class="clear">
                <div v-if="index % 2 == 1" class="textBox fl">{{item.value}}</div>
                <div v-else class="textBox fr">{{item.value}}</div>
            </div>
            // 浮动clear
            <div class='clear'>
                <div class="textBox fl">1</div>
            </div>
            <div class='clear'>
                <div class="textBox fr">567</div>
            </div>
        </div>
```
## 2020-8-25
- easy:
```javascript
    //下面代码输出的结果是什么？
    var num =3;
    console.log(num.toString(2))
    console.log(num.toFixed(2))
```
> 答案是 11,3.00。原始值进行属性操作时会有包装类操作，这里没多大影响，主要是toString()方法没传数字时，会将其装换成字符串，传了数字后会对其进行n进制转换，所以这里是二进制的11；toFixed()是保留几位小数。

- normal:
对CSS盒模型了解多少？
> 盒模型分为标准盒模型及怪异盒模型，代码为box-sizing:content-box|border-box，标准盒模型的元素宽度=padding*2+border*2+width,但我们设置的width只是他的内容区content的宽度，所以与实际情况不太匹配。怪异盒模型的width=padding*2+border*2+content，符合我们实际用途，现在绝大多数都用这个。然后谈谈margin塌陷和合并，margin塌陷的解决方法就是出发bfc，具体方式为在父元素上设置：1. position:absolute|fixed、2.float:left|right，3.overflow:hidden，4.display:inline-block.
margin合并我们通常不触发bfc，只设置上面元素的margin-bottom就行。

- 小demo:
```javascript
// 科里化函数:一个函数中部分参数固定，只需要传入一部分剩下的参数即可。
    function cur(func,...args){
        return function(...subs){
            let total = [...args,...subs]
            if(total.length >= func.length ){
            return func(...total)
            }else{
                return cur(func,...total)
            }
        }
    }

    function add(a,s,d,f,g){
        return a+s+d+f+g
    }

    var f1 = cur(add,2,3)
    var sum = f1(11)
    var sum1=sum(10,2,3)
    console.log(sum1)
```

## 2020-8-26
- easy:
```javascript
//下面的代码会输出什么结果？
    class Person{
        constructor(name){
            this.name=name;
            //this.sayHello1=()=>{...}
        }
        sayHello1 =()=>{
            console.log(this.name)
        };
        sayHello2(){
            console.log(this.name)
        }
    }

    const p = new Person('cl');
    p.sayHello1();  //cl
    Person.prototype.sayHello1(); //报错
    Person.prototype.sayHello1.call(p); //报错
    p.sayHello2(); //cl
    Person.prototype.sayHello2(); //undefined
    Person.prototype.sayHello2.call(p); //cl
```
> 答案是注释中写的那些。考虑到类的书写，hello1的写法就相当于this.hello1，即写在注释那位置。hello2即写在原型上，但原型上的this没有name，综合这些即可得出答案。

- normal:
常用的CSS选择器？
> 除了常用的元素选择器（通配符（少用，耗资源）、标签、class、id、伪类、属性），主要讲一下其中的伪类选择器:first-child及:first-of-type，前面一种少用，相比之下用后面一种比较好，更符合现实语境。有这些:first-of-type, :last-of-type, :only-of-type, :nth-of-type(), :nth-last-of-type()。权重为！important>行间样式>id>class>标签>*；

- 小demo:
```javascript
    // 调用云数据库
    // app.vue的created函数中初始化
    wx.cloud.init({
        traceUser:true,
        env:'test-41qbp'
    })
    // 各vue中调用
    const db = wx.cloud.database({
        env:'test-41qbp'
    })
    db.collection('数据库名').get().then().catch()

    //调用云函数
    // project.config.json中配置
    // "cloudfunctionRoot":"static/functions"，同时也在wx/dist目录下创建相同文件名。内容仅为这个。static下创建文件夹
    // app.vue的created函数中初始化,
    wx.cloud.init({
        traceUser:true,
        env:'test-41qbp'
    })
    // 修改，在云函数的index.js下添加修改为
    const cloud = require('wx-server-sdk')
    cloud.init()
    const db = cloud.database()
    exports.main = async (event, context) => {
    
        return await db.collection("shujuku3").update({
        data:{
            nameData: event.nameData
        }
        }).then(res=>{
        console.log(res)
        }).catch(console.error)
    }
    // 调用
    wx.cloud.callFunction({name:'xxx',data:{nameData:'xx'}}).then().catch()
```

## 2020-8-27
- easy:
```javascript
    // 下面的代码运行后，变量set中保存了什么
    var set = new Set();
    var a= [];
    set.add(a);
    a.push(1);
    set.add(a);
    a=[1];
    set.add(a);
    a.pop();
    set.add(a);
    set.add([]);
    set.add([1])

    console.log(set)
```
> 答案是{[ 1 ], [], [], [ 1 ]}。new Set()得到的值为不重复的集合，但得注意数组存的是地址，所以a=1上面的语句只会得到{[1]},后面的安装规律就行。

- normal:
v-for为什么不推荐用Index作key值?
> 因为v-for循环的时候，如果这时候再对循环列表进行增删操作，会导致元素根据key值进行渲染，如果用Index作key值，会产生多个渲染，这与虚拟dom的缓存及数据响应不相符。例如[1,2,3,4,5],如果要插入一个值变为[1,2,8,3,4,5],若是用Index当中key值，会导致8及以后的选项都重新渲染，若单独给他们一个id，则只会渲染加载8.

- 小demo:
```javascript
    // Typescript初体验
    //创建一副牌并打印出来（无大小王）
    type Color="♥"|"♠"|"♦"|"♣"
    type Mark = number

    interface Cards {
        color: Color
        mark: Mark
    }
    function card(): Cards[] {
        let result: Cards[] = [];
        for (let i = 1; i <= 13; i++) {
            result.push({
                color:"♥",
                mark:i
            })
            result.push({
                color:"♠",
                mark:i
            })
            result.push({
                color:"♣",
                mark:i
            })
            result.push({
                color:"♦",
                mark:i
            })
        }
        return result
    }
    function Print(arr:Cards[]){
        let str:string='';
        arr.forEach((item,i) =>{
            if(i%6 ===0){
                str +='\n';
            }
            str += item.color + item.mark + '\t';
        })
        console.log(str)
    }
    let data = card();
    Print(data)
```

## 2020-8-28
- easy:
```javascript
    // 声明一个变量a的值为1，变量b的值为2。如果不借助其他变量，完成a与b的交换
    var a = 1;
    var b=2;
    //some code

    console.log(a,b) //2,1
```
> [a,b] = [b,a]。我这里用的是es6里的解构。

- normal:
聊聊箭头函数？
> 箭头函数的写法简便，绑定了this，this指向箭头函数所处的环境。所以需要得到动态的this时，或者构造函数这种this有其指向对象实例是最好不用它，同事它也没有arguments、call、apply、bind这些。除此之外，大多都可以尽情使用箭头函数。

- 小demo:
> 今天就记一下，把上面的扑克牌函数优化了一下，用了ts的模块化导入导出，但由于在配置的时候```module:commonjs```这里配置成了es2016所以一直报错，记一笔，因为是运行在Node环境中，所以用commonjs，还有就是print函数名的问题，发现会有冲突，不知道昨天运行时为什么没报错，今天要把print名修改下。

## 2020-8-29

- easy:
```javascript 
//下面的代码会输出什么结果？
    function Print(x,y){
        this.x=x;
        this.y=y;
        this.moveTo=function(x,y){
            this.x=x;
            this.y=y;
            console.log(this.x +','+this.y);
        }
    }
    var p1=new Print(0,0);
    var p2 = {x:0,y:0};
    p1.moveTo(1,1);
    p1.moveTo.apply(p2,[10,10])
```
> 答案是 1,1 10,10。this.moveTo函数中的this指向moveTo这个函数。apply把this指向了p2.

- normal:
set与map?
> 这两个都是es6新加的集合，目的是因为原有存数据的数组及对象不能够满足要求，所以新出了两个存储数据的集合类型。set是去重。使用方式为：```var s= new Set([1,3,5,4,5])```会得到{1,3,5,4},再对其进行数组展开就达到数据去重的目的了。遍历可以用for...of，其身上也还有add()|clear()|has()|value()|keys()等方法。  map方法则是存储一个键值对，使用方式为：```var map = new Map();map.set('name','value');map.get('name')```，主要是set、get方法，然后其自身也有add|has|value等方法，循环的话也用for...of循环。

- 小demo:
```javascript
    // 图片瀑布流插件
function waterfall(options) {
    // 默认配置
    let defaultOption = {
        minGap: 10, //间隔
        imgWidth: 220,  // 图片宽度
        container: document.body, //父级
        imgList:[] // 图片地址
        number: 5
    }
    // 混合配置
    options = {
        ...defaultOption,
        ...options
    }
    let imgs = []; //存储所有的img元素
    conPosition()
    createImg();
    window.onresize = function () {
        setPosition()
    }
    //创建img元素
    function createImg() {
        options.imgList.forEach((t, i) => {
            var img = document.createElement('img');
            img.style.width = options.imgWidth + 'px';
            img.style.position = 'absolute';
            img.style.transition = ".5s"

            img.src = t;
            imgs.push(img)
            options.container.appendChild(img);
            img.onload = function () {
                setPosition()
            };
        })
    }

    // 获取图片水平方向上的信息：容器宽度、图片数量、间隔
    function getHorizontal() {
        var obj = {};
        obj.clientWidth = options.container.clientWidth;
        // 获取每行列数
        obj.number = Math.floor((obj.clientWidth + options.minGap) / (options.imgWidth + options.minGap));
        // 动态计算出每列间隔
        obj.gap = (obj.clientWidth - obj.number * options.imgWidth) / (obj.number - 1)
        return obj;
    }
    // 设置每一张图片的位置
    function setPosition() {
        let info = getHorizontal();
        let arr = new Array(info.number);
        arr.fill(0);
        imgs.forEach((item, index) => {
            let min = Math.min.apply(null, arr)
            let i = arr.indexOf(min);
            arr[i] = min + item.clientHeight + info.gap;
            item.style.top = min + 'px';
            item.style.left = i * (info.gap + options.imgWidth) + 'px';
          
        })
        // 高度自适应
        let max = Math.max.apply(null,arr);
        options.container.style.height = max + 'px'
    }
    // 判断父元素是否有定位
    function conPosition() {
        let attr = window.getComputedStyle(options.container);
        if (attr.position === 'static') {
            options.container.style.position = 'relative'
        }
    }

}

```

## 2020-8-30

- easy:
```javascript
    // 下面代码输出什么？
let globalVar ='xyz';
(function (outerArg){
    let outerVal='a';
    (function(innerAgr){
        var innerVal = 'b';
        console.log(
            `"outerAgr = " + outerArg + ","+
            "innerAgr = " + innerAgr + ","+
            "outerVal = " + outerVal + ","+
            "innerVal = " + innerVal + ","+
            "globalVar = " + globalVar`
        )
    })(456)
}(123))

```
> 输出为123,456,a,b,xyz(没写格式)。

- normal:
谈谈es6中的类？
> 由于最初js中没有面向对象编程的概念，所以随着应用范围的扩大，对其需求的增加，我们在编程的时候就需要一些面向对象的思想，最初是进行原型上的改造，即创建构造函数，并在其原型上增加方法，令其与面向独享相似，而class类就是构造函数的语法糖。它使得构造函数在形式上变得简便，而且也没有了变量提升。使用方式为：```class animai{constrouctor(){},protoFun(){}}```constructor中是私有属性，会直接进行调用，其他原型上方法则会根据情况进行调用，添加方法既可以在prototype上直接添加，也可以利用Object.assign(per.prototype,obj)进行添加。

- 小demo:
```javascript
//对昨天的瀑布流进行优化，增加防抖效果
    //创建img元素
    function createImg() {
        options.imgList.forEach((t, i) => {
            var img = document.createElement('img');
            img.style.width = options.imgWidth + 'px';
            img.style.position = 'absolute';
            img.style.transition = ".5s"
            img.src = t;
            imgs.push(img)
            options.container.appendChild(img);
            img.onload =  debounce1
        })
    }
    //注意顺序， debounce1要在createImg前创建。 
       conPosition()
    var debounce1 = debounce(setPosition,100,false)
    createImg();
    // 改变窗口大小
    window.onresize = debounce1

```
## 2020-8-31
- easy:
```javascript 
// 下面代码输出什么？
console.log(1 + undefined); //NaN
console.log(1 + null); //1
console.log(undefined + null); //NaN
console.log(true + false); //1
console.log(3 +'6');   // '36'
console.log(2 + []);    //'2'
console.log(2+[2,3])  //'22,3'
console.log(2 +{a:12});    //'2[object,Object]'
console.log([]+{});  // '[object,Object]'
console.log({}+[]);   // '[object,Object]'
console.log(3 + {});   //'3[object,Object]'
console.log({}+3);   //'[object,Object]3'
```
> 答案如上，要注意的是隐式类型转换时，{}、[]这两个的结果。

- normal：
常见的状态码？
> 1xx,表示接收到请求并且继续处理。 2xx，表示响应成功，200：请求被成功响应，请求的数据发送回客户端。3xx，表示重定向，301：永久重定向；302：临时重定向；303：临时重定向，只能用get请求；304：上次使用后，请求的网页未修改，使用缓存。4xx:客户端错误，400：请求语法错误，403：禁止访问，服务器收到请求，但拒绝访问。404：请求资源不存在。5xx:服务端错误，500：服务器出错，503：服务器负载过大，暂时没办法处理请求。

- 小demo:
```javascript
// 扑克牌优化：采用枚举。
export enum Color {
    heart = "♥",
    spade = "♠",
    club = "♣",
    diamond = "♦"
}
export enum Mark {
    A = "A",
    two = "2",
    three = "3",
    four = "4",
    five = "5",
    six = "6",
    seven = "7",
    eight = "8",
    nine = "9",
    ten = "10",
    eleven = "J",
    twelve = "Q",
    king = "K"
}
export type Cards = {
    color: Color
    mark: Mark
}
export type Deck = Cards[];

    export function myPrint(arr:Deck): void {
    let str: string = '';
    arr.forEach((item, i) => {
       str += item.color+item.mark +'\t'
       if((i+1)%6 ===0){
           str +='\n'
       }
    })
    console.log(str)
}
export function card(): Deck {
    let result: Deck = [];
    let mark =Object.values(Mark)
    let color = Object.values(Color)
    for (const m of mark) {
        for (const c of color) {
            result.push({
                color:c,
                mark:m
            })           
        }
    }
    return result
}
```
# 2020年9月
## 2020-9-1
- easy:
```javascript 
// 下面代码输出什么？
var a = 1;
function fn1(a){
console.log(a)
a = 2;
}
fn1(); //undefined
console.log(a); //1
```
> 答案在上面，考察的是预编译。

- normal:
事件委托？
> 在实际应用中，又是要给每个子元素添加事件会显得很麻烦，所以我们会利用事件委托直接在它们的父节点上触发函数，这得益于浏览器的事件冒泡机制。事件委托用到了js的两个经常被忽略的特性：事件冒泡和目标事件，
```javascript
function getEventTarget(e){
    e = e||window.event;
    return e.target||e.srcElement
}
```

- 小demo:
```javascript
// 二叉树的深搜
function deepSearch(root,target){
    if(root.value == null){
        return false;
    }
    if(root.value == target) return true;
    let left = deepSearch(root.left,target);
    let right = deepSearch(root.right,target);
    return left || right;
}
```

## 2020-9-2
- easy:
```javascript
// 下面代码输出什么？
// 如何改正可以输出0 1 2 3 4？
for(var i = 0; i<5;i++){
    setTimeout(function(){
        console.log(i)
    },i*1000)
}
```
> 答案是5 5 5 5 5。修改方法是将var改为let，或者将定时器用立即执行函数包裹起来。

- normal:
```javascript
    // 记一下今天的面试题
    var a =function b(){
        // console.log(a,b) //function b(){} function b(){} 这行自己添加的
        console.log(a ==b) 
    }
    a();// true
    b(); //报错

    function test(){
        var num =888;
        var fun=function(){ console.log(num)}
        num++;
        return fun;
    }
    var t1 =test();
    t1() // 889，闭包了
```

- 小demo:
```javascript
    // 二叉树的广搜
    //结构为 a.left=b;a.right=c; a.value='a'，注意这里传入一个数组
    function plus(arr,target){
        if(arr == null || arr.length == 0) return false;
        let childList=[];
        for (let i = 0; i < arr.child.length; i++) {
            const element = array[i];
            if (element !=null && element.value ==target) {
                return true;
            }else{
            childList.push(element.left);
            childList.push(element.right);
            }
        }
        return plus(childList,target)
    }
```

## 2020-9-3
- easy:
```javascript
    // 下面代码输出什么
    var a = {},
    b={key:'b'},
    c={key:'c'}
    a[b] =123;
    a[c]=456;
    console.log(a[b])
```
> 答案是 456。 因为a[b]这种写法里面的b是变量，但b又是一个对象，根据我们平时的对象写法，对象的属性是一个字符串，所以这里会调用toString()方法，则对象会变成'[object,Object]'，所以这里相当于两次给a['[object,Object]']赋值，结果为456.

- normal:
解决跨域问题？
> 跨域存在于浏览器端，处于浏览器把数据接收到后发现有跨域情况才会报错，协议、域名、端口号只要有一样不同就会引起跨域。解决跨域的方法有，1.使用JSONP请求，2.使用iframe标签。3.后端响应头设置CORS，即'access-control-allow-origin:*'。4.后端进行代理，例如用node穿件一个接口，这个接口利用axios请求跨域的url数据，前端则axios请求node的这个接口。

- 小demo:
```javascript
// 二叉树部分算法  a={value:'a',right:b,left:c} 
/***
 *      A
 *    C   B
 *  F  G D  E
 * 
 */

// 前序遍历 ACFGBDE
function front(root){
    if(root == null) return;
    console.log(root.value);
    front(root.left);
    front(root.right);
}
// 中序遍历 FCGADBE
function f1(root){
    if(root == null) return ;
    f1(root.left);
    console.log(root.value);
    f1(root.right);
}
// 后序遍历 FGCDEBA
function last(root){
    if(root == null) return;
    last(root.left);
    last(root.right);
    console.log(root.value);
}
```

## 2020-9-4
- easy:
```javascript
    // 下面代码输出什么？
    console.log(false == '0');
    console.log(false === '0');
```
> 答案是 true，false。考察比较运算符，相对等于和绝对等于

- normal:
预编译口号及四部曲？
> 口号：1.函数声明整体提升，变量声明提升。2.四部曲（AO）：a.创建AO对象。 b.找形参与变量声明，值为undefined。 c.形参、实参相统一。 d.在函数体内找到函数声明，值赋予函数体。

- 小demo:
```javascript
     // 冒泡排序  排序的本质就是比较和交换
    function sort(arr){
        for(var i = 0;i<arr.length;i++){
            for (var j = 0; j < arr.length-i-1; j++) {
                if(isBig(arr[j],arr[j+1])){
                    exchange(j,j+1,arr)
                }
            }
        }
    }
    // 交换
    function exchange(a,b,arr){
        var temp=arr[a];
        arr[a]=arr[b];
        arr[b] = temp;
    }
    // 比较
    function isBig(a,b){
        return a-b> 0? 1:0
    }
```

## 2020-9-5
- easy:
```javascript
    // 下面代码输出什么？
    console.log("0||1=" + (0||1)); //1
    console.log("1||2=" + (1||2)); //1
    console.log("0&&1=" + (0&&1)); //0
    console.log("1&&2=" + (1&&2)); //2
```
> 答案将注释部分拼接就行，考察逻辑运算符。

- normal:
谈谈less?
> less是一种css预编译器，安装过后自带一个lessc工具，可以单独编译less文件。常见作用除了嵌套，还有变量@red:#f40，将.classname(){color:#222}当做一个代码段混入，函数调用，@import('xxx')导入等操作;

- 小demo:
```javascript
    // vue ssr渲染  （极其简陋版）
    const Express = require('express');
    const app =new Express();
    const Vue = require('vue');
    const serverRender=require('vue-server-renderer');
    const fs = require('fs')
    const template = fs.readFileSync('./template.html') //此html中带有 `<!--vue-ssr-outlet-->`
    const vm = new Vue({
        template:`<div> hello world </div>`
    })
    const render = serverRender.createRenderer({
        template
    })
    app.get('/',(req,res)=>{
        let value = render.renderToString(vm); //将vue转换成字符串渲染
        res.end(value)
    })
    app.listen(3000,_ =>{ console.log('server is running at 3000')})
```

## 2020-9-6
- easy:
```javascript
    // 下面代码的输出结果是什么？
    var length = 10;

    function fn() {
        console.log('m',this)
    }
    var obj = {
        length: 5,
        method: function (fn) {
            fn();
            arguments[0]();
        }
    };
    obj.method(fn, 1)
```
> 答案是10 2。考察this指向。第一个10很好得出，这里arguments[0]()相当于```arguments.0()```，0就是fn，this指向arguments,参考arguments['fn'] == arguments.fn()。注意arguments是一个对象非数组，这样一想容易理解了。

- normal:
绑定事件的方式？
```javascript
    // 时间类型可选，这里案例用click
    // 1.句柄方式 只能绑定一个
    dom.onclick=function(){}
    dom.onclick = null

    // w3c 可绑定多个
    dom.addEventListener('click',fn,false) //第三个参数为是否捕获
    dom.removeEventLister('click',fn,false) //要注意，只有命名函数才可以取消

    // ie  这个方法是IE独有的方法，一个事件同样可以绑定多个处理函数。
    // 当同一个函数绑定多次的时候，addEventListener是只执行一次，但是attachEvent会绑定几次执行几次。
    dom.attachEvent('onclick',fn);
    dom.detachEvent('onclick',fn) //也是同一个函数才可以解除绑定，匿名函数无法解除绑定
```

- 小demo:
```javascript
    // 原型链圣杯模式
    let inherit = (function(){
        let F = function(){};
        return function(target,origin){
            F.prototype = origin.prototype;
            target.prototype = new F();
			target.prototype.constructor = target;
			target.prototype.uber = origin.prototype; //补充说明我是由谁来的，即origin.prototype
        }
    })()
```

## 2020-9-7
- easy:
```javascript
    // 实现函数内部参数对调
    function swap(a,b){
        [a,b] = [b,a]
    }
```

- normal:
精灵图（css sprite）有什么优缺点？
> 假如要请求十张图片，不用精灵图的话会导致发送十次http请求，很消耗性能。用了精灵图后会值请求一次大大优化了性能，但缺点是不容易定位具体需要的icon，以及要对精灵图增加、删除icon时有些麻烦。

- 小demo:
```javascript
// 封装一个兼容性的事件绑定函数
function handle(ele, type, fn) {
    if (ele.addEventLister) {
        ele.addEventLister(type, fn, null)
    } else if (ele.attachEvent) { //ie上的this指向window
        ele['temp' + on + type] = fn;
        ele['on' + type] = function () {
            ele['temp' + on + type].call(ele)
        }
        ele.attachEvent('on' + type, ele['on' + type])
    } else {
        ele[('on' + type)] = fn;
    }
}
// 接触绑定事件
function removeHandle(ele, type, fn) {
    if (ele.removeEventListener) {
        ele.removeEventListener(type, fn, false)
    } else if (ele.detachEvent) {
        ele.detachEvent('on' + type, fn)
    } else {
        ele['on' + type] = null
    }
}
```
## 2020-9-8
- easy:
```javascript
    //  下面代码输出什么？
    var arr=[];
    arr[0]='a';
    arr[1]='b';
    arr.foo = 'c';
    console.log(arr.length); //2
    arr.length += arr.foo.length;
    console.log(arr.length)  //3
```
> 答案是2 3.arr.foo这个属性不算length里，下一行代码2+1 =3；

- normal:
(坐在电脑前不知道应该写什么，最近好烦啊)
三栏布局集中实现方式？
> 方法1：flex布局，方法2：利用absolute和中间元素的margin-left\margin-right实现。方法3：浮动实现。

- 小demo:
```javascript
    // 简单快速排序
function sort(arr){
    if(arr == null || arr.length == 0) return [];
    var left=[];
    var right = [];
    var num = arr[0];
    for (let i = 1; i < arr.length; i++) {
        if(arr[i]> num){
            right.push(arr[i])
        }else{
            left.push(arr[i])
        }
    }
    right = sort(right);
    left = sort(left);
    left.push(num);
    return left.concat(right);
}
```

## 2020-9-9
- easy:
```javascript
//  0的除法运算
var num = false;
var result = num / 0 ;
if(result){
    console.log(result * 2 + '2' + 4)
}else{
    console.log(!result * 2 + '2' + 4)
}
```
> 答案是'224'。 0/0=>NaN, ！NaN => true。

- normal:
```javascript
    //数据类型转换之Number
    console.log(Number('2')) //2
    console.log(Number('2b')) //NaN
    console.log(Number('')) //0
    console.log(Number([0])) //0
    console.log(Number([])) //0
    console.log(Number([8,8])) //NaN
    console.log(Number([8,'b'])) //NaN
    console.log(Number({})) //NaN
```
> 	Number()
    1、undefined	NaN
    2、null			0
    3、布尔值		true为1，false为0
    4、字符串
        1、空字符串，空格字符串转为0
        2、非空字符串，并且内容为纯数字（包含进制与科学表示法）转成对应的数字
        3、其余都是NaN
    5、数字		原来的数字
    6、对象
        1、对象、函数转成NaN
        2、空数组转为0，数组里只有一个数据并且这个数据能转成数字，则转成对应的数字，其它都转成NaN 

- 小demo:
```javascript
// 记录美团项目中axios的二次封装
    axios.defaults.baseURL = 'http://www.baidu.com';
    axios.interceptors.request.use(config => {
        // Do something before request is sent
        config = {
            ...config,
                params:{
                    appkey:"asdaklsdajsdn123"
                }
        }
        return config;
    }, error => {
        // Do something with request error
        return Promise.reject(error);
    });

    export var api = {
        getData(){
            return axios.get('url')
        },
        getInfo(params){
            return axios.get('url',params)
        }
    }
    // 调用
    import { api } from 'xx'

    methods:{
        api.getData().then(res => { console.log(res)})
    }
```

## 2020-9-10
- easy:
```html
<!-- 下面代码输出什么 -->
<div class="content"></div>
<div class="content"></div>
<div class="content"></div>

<script>
    var divs = document.getElementsByClassName('content');
    divs[1].className='change';
    console.log(divs.length)
</script>
```
- normal:
> （记录一下今天面试题）
> css中@supports是什么意思？ 
> 它是检查浏览器是否支持某个CSS属性。若支持则使用里面的css代码 ```@supports (display: grid) {div {display: grid;}}``` ,有点类似于media。
> em、rem、rpx、px各自是什么？
>em是相对长度单位，它是相对当前对象内的文本字体尺寸的，不进行设置的话，默认1em=16px。rem是css3的一个新属性，是相对根元素字体尺寸的。rpx是微信小程序的屏幕宽度尺寸，规定750rpx即为屏幕总宽。px是像素单位。

- 小demo：
```javascript
//  css tree shaking （借鉴了这里https://www.cnblogs.com/geyouneihan/p/9575674.html）
const PurifyCss = require('purifycss-webpack');
const glob = require('glob-all');

// 为了方便最后检查打包后的 css 文件，配置中还使用了 extract-text-webpack-plugin 这个插件。
const ExtractTextPlugin = require("extract-text-webpack-plugin");

let purifyCSS = new PurifyCSS({
    paths: glob.sync([
      // 要做CSS Tree Shaking的路径文件
      path.resolve(__dirname, "./*.html"), // 请注意，我们同样需要对 html 文件进行 tree shaking
      path.resolve(__dirname, "./src/*.js")
    ])
  });

module.exports={
    module:{
        rules:[
            {
                text:/\.css$/,
                use: ExtractTextPlugin.extract({
                    fallback: {
                      loader: "style-loader",
                      options: {
                        singleton: true
                      }
                    },
                    use: {
                      loader: "css-loader",
                      options: {
                        minimize: true
                      }
                    }
                  })
            }
        ]
    },
    plugins:[extractTextPlugin, purifyCSS]
}
```

## 2020-9-11
- easy:
```javascript
    //下面代码输出什么？
     var obj={
   name:'cl',
   age:18,
   method(){
     (function(){
       console.log(this.name)
     })()
   }
 }
 obj.method()
```
> 答案是 undefined。这道题是昨天的面试题，刚刚突然想起来了，一校验发现昨天还是写错了，这个时候的this指向window我明白，但是window是对象，给要获取对象上不存在的属性时只会返回undefined,而不会报错，记一笔。

- normal:
typescript的基本约束有哪些？
> 有number string boolean undefined null object array void never 联合类型 字面量类型 元祖类型 类型别名type 枚举enum 接口interface 类class

- 小demo:
```javascript
    //vue-class-component插件的用法
    import Vue from 'vue'
    import Component from 'vue-class-component'
    @Component({
        components:{
            'test':test
        },
        beforeEnter(){
            //todo
        }
    })
    export default class MyComponent extends Vue {
        a = 'z'
        data(){
            return {
                b:'x'
            }
        }
        methodFun1(){
            //todo
        }
        get msg(){
            return a+3;
        }
    }
```

## 2020-9-12
- easy:
```javascript
    //下面代码输出什么？
 var func = function course(){
   console.log(typeof course);
 }
 func(); 
 console.log(typeof course)
```
> 答案是 function undefined。

- normal:
typescript的一些词语
> 类的继承：如果A和B都是类，且可以描述为A是B，则A和B形成继承关系，A是子类，B是父类。若A继承自B，则A中自动拥有B中所有成员。例如玩家坦克是坦克。这里需注意this指向是动态的。继承还有单根性及传递性。

- 小demo:
```javascript
    //抽象类语法熟悉
    abstract class User {
        static users:User[] = []
        constructor(public loginName: string, public LoginPwd: string) { 
            User.users.push(this)
        }
        abstract move(x: string): boolean
    static login(loginName:string,loginPwd:string): void {
            console.log(loginName, loginPwd)
        }
    }
    class u1 extends User {
        move(x: string) {
            console.log(x + 'is ok')
            return true
        }
    }
    let p = new u1('cl','18')
    let p2 = new u1('c2','18')
    console.log(User.users)
```

## 2020-9-13
- easy:
```javascript
//agguments的数据类型是什么？
```
> 答案是对象。因为输出结果是数组类似，也被我们称为类数组。

- normal:
装饰符？
> 装饰符是用来解决关注点分离的问题。分为类装饰符和成员装饰符、参数修饰符。类装饰符接收一个参数，表示类本身。成员装饰符分属性和方法，属性有两个参数，如修饰静态属性，第一个则为类本身，否则为该类的原型。第二个参数为修饰的属性名，固定为字符串。方法修饰符有三个参数，前两个与属性的一样，第三个为属性描述对象。

- 小demo:
```javascript
//类型别名
type User={
    name:string
    age:number
  }
//接口
  interface User1{
      name:string
      age:number
  }
```

## 2020-9-14
- easy:
```javascript
    //如何获取形参的个数？
    function(arg1,arg2){
        //上面的arg1,arg2
    }
```
> arguments。

- normal:
> javascript中进行$emit全局注册时，记得在路由destroy时把它取消掉，即$off('xx')，这样是防止其一遍遍调用。

- 小demo:
```javascript
    //layui在ts+vue中的使用
    window["layui"].use('table',function(){
        const table = window["layui"].table;
        table.render({
            elem:"#demo",
            height:312,
            page:true,
            url:'./url',
            data:[],
            cols:[{
                field:"id",title:'ID',width:80,sort:true,fixed:true
            },{
                field:"name",title:'name',width:80,sort:true
            }]
        });
    })
```

## 2020-9-15
```javascript
//下面代码输出什么？
function fun(n,o){
    console.log(o);
    return {
        fun:function(m){
            return fun(m,n)
        }
    }
}
var a= fun(0);
a.fun(1)
a.fun(2);
a.fun(3);
var b = fun(0).fun(1).fun(2).fun(3);
var c= fun(0).fun(1);
c.fun(2);
c.fun(3)
```
> 答案是undefined 0 0 0 undefined 0 1 2 undefined 0 1 1。注意a.fun(1)时，return时fun执行，此时为实参n=0，清楚了这个思路后就可以一次计算。

- normal:
```javascript
    //这两天新入职，接触的项目跟自己平时敲得比较起来有点庞大，在恶补相关代码的功能实现，突然有点不想更新太多自己的每日git了
    //就记几个组件名称吧：表单form、表格table、遮罩层dialog、穿梭框transfer
```

## 2020-9-16
- easy:
```javascript
    var user="cl";
    function first(){
        console.log(user);
    }
    function second(){
        var user = 'cj';
        first();
    }
    second()
```
> 答案是 cl。 

> 今天就写个easy部分。

## 2020-9-17
- easy:
```javascript
    //如何阻止点击a链接时的跳转
           a.onclick=function(e){
           // 方法一： e.preventDefault()
           //方法二： return false
       }
```
> 方法一具有通用性，方法二为onclick的方法。

## 2020-9-18
- easy:
```javascript
    //引用值类型与原始值类型的区别？
```
> 原始值类型有boolean string number undefined null，引用值类型有array object function。它们之间的区别在于存储的方式不同，原始值采用的是数据段存储，值和地址一样；引用值类型采用的是地址存储，访问的是一个指针而不是值。所以直接进行复制的时候原始值是各自分离的，但引用值是共用一个地址，一个对象的改变会引起另一个指向该地址的值得改变。