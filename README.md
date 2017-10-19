<p align="center"><a href="https://vuejs.org" target="_blank"><img width="100"src="https://vuejs.org/images/logo.png"></a></p>

<p align="center">
  <a href="https://github.com/songxtianx/vue1-drapload/"><img src="https://img.shields.io/badge/vue1_drapload-v1.0.0-09BF3E.svg" alt="Version"></a>
  <a href="https://v1.vuejs.org/api/"><img src="https://img.shields.io/badge/language-vue_v1.0.21-09BF3E.svg" alt="Language"></a>
  <a href="http://www.cnxusong.com/"><img src="https://img.shields.io/chrome-web-store/stars/nimelepbpejjlbmoobocpfnjhihnpked.svg" alt="Rating"></a>
  <a href="https://img.shields.io/badge/platform-android|ios-brightgreen.svg"><img src="https://img.shields.io/badge/platform-android|ios-brightgreen.svg" alt="platform"></a>
  <a href="https://www.facebook.com/songxtianx"><img src="https://img.shields.io/badge/facebook-songxtianx-brightgreen.svg" alt="Facebook"></a>  
  <br>
  <a href="https://saucelabs.com/u/vuejs"><img src="https://saucelabs.com/browser-matrix/vuejs.svg" alt="Sauce Test Status"></a>
</p>


# 上滑加载更多数据 Drap Load

数据源用的静态json,也可以使用rap作为接口。改进了加载完数据重复加载的问题。
<br>
<br>
<br>

## 实例 DEMO（在线网址和二维码）

### 在线网址 URL

<br>

&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp; [https://songxtianx.github.io/vue1-drapload/](https://songxtianx.github.io/vue1-drapload/)

<br>

### 二维码 QR

<br>

![img](https://songxtianx.github.io/vue1-drapload/temp/qr.png)

<br>

## 代码 CODE
### HTML
```html
    <div v-drapload drapload-key="ascroll" drapload-initialize="true" drapload-down="down()">

            <div class="item clearfix" v-for="data in a" v-cloak>
                <h1 class="title text-over">{{data.name}}</h1>
                ...
            </div>

    </div>
```
### JAVASCRIPT
通过前端循环数据，判定数据加载完成之后修改JS里的数据状态。


```JavaScript
    var counter = 0;
    var app = new Vue({
        el: 'body',
        data: function () {
            return { a: []}
        },
        ready: function () {
            var me = this;
            me.$options.vue = me
        },
        /**
         * 加载数据
         * @param fn
         */
        loadListData: function (fn) {
            // 每页展示个数
            var num = 4;
            var pageStart = 0,pageEnd = 0;
            var me = this.vue;
            $.ajax({
                url: '/vue1-drapload/projectManage.json',
                data: {},
                type: 'GET',
                success: function (data) {
                    // 初始化数据 先获取所有数据并初始化前num条
                    // Initialize the data,get 0-num data of the array.
                    counter++;
                    pageEnd = num * counter;
                    pageStart = pageEnd - num;
                    if(pageStart <= data.sites.length){
                        for(var i = pageStart; i < pageEnd; i++){
                            for (j = 0; j < data.sites[i].icon.length; j++) {
                                if (data.sites[i].icon[j].num>99) {
                                    data.sites[i].icon[j].num = "99+";
                                }
                            }
                            fn(data.sites[i]);
                            if((i + 1) >= data.sites.length){
                                function noDatas() {
                                    me.$root.ascroll.noData();
                                }
                                me.$root.ascroll._options.loadDownFn=noDatas();
                                me.$root.ascroll.resetload();
                                break;
                            }
                        }
                    }
                }
            });
        },
        methods:{
            down: function () {
                var me = this
                //当滚动条距离底部高度等于你在drapload-foot设置的高度时将运行一次此函数
                //if scrollTop = drapload-foot , function run.
                me.$options.loadListData(function (data) {
                    me.a = me.a.concat(data)
                    me.ascroll.resetload()
                });
            }
        }
    })        
```

## 保持联系 Stay In Touch

- [Facebook](https://www.facebook.com/songxtianx)
- [Blog](http://www.cnxusong.com)
- [微博](http://weibo.com/songxtianx)
