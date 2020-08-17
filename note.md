# 前言
此为记录贴，尽量一天一记。
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