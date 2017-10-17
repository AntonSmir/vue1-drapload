<p align="center"><a href="https://vuejs.org" target="_blank"><img width="100"src="https://vuejs.org/images/logo.png"></a></p>

<p align="center">
  <a href="https://circleci.com/gh/vuejs/vue/tree/dev"><img src="https://img.shields.io/circleci/project/vuejs/vue/dev.svg" alt="Build Status"></a>
  <a href="https://codecov.io/github/vuejs/vue?branch=dev"><img src="https://img.shields.io/codecov/c/github/vuejs/vue/dev.svg" alt="Coverage Status"></a>
  <br>
  <a href="https://saucelabs.com/u/vuejs"><img src="https://saucelabs.com/browser-matrix/vuejs.svg" alt="Sauce Test Status"></a>
</p>


## 上滑加载更多数据

数据源用的静态json,也可以使用rap作为接口。改进了加载完数据重复加载的问题。

### 实例DEMO（在线网址和二维码）

- [在线 DEMO 网址链接](https://songxtianx.github.io/vue1-drapload/)  <br>
[https://songxtianx.github.io/vue1-drapload/](https://songxtianx.github.io/vue1-drapload/)

- 二维码  <br>
![img](https://songxtianx.github.io/vue1-drapload/temp/qr.png)

### 代码

通过前端循环数据，判定数据加载完成之后修改JS里的数据状态。


```JavaScript
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
        }
```
