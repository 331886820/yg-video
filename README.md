[博客](https://juejin.cn/post/7369783680425852938) |
[NPM地址](https://www.npmjs.com/~yg331886820)  |
[git源码](https://github.com/331886820/yg-video) |
[gitee源码](https://gitee.com/esthergege/live-video)

# 本示例 只展示海康威视 demo 运行结果如下图
![0711](https://github.com/331886820/yg-video/assets/29726492/67145c8e-1672-4fa3-a4dc-a176f0ef9cf9)

说明：1.请先安装浏览器视频预览插件  见文章[《vue如何实时展示海康威视摄像头多画面？》](https://juejin.cn/post/7369783680425852938) 插件安装；
     2.  /publish/index.html 中  需引入 
     
                                    <script src="./hk/jquery-1.7.1.min.js"></script>
                                    
                                    <script src="./hk/webVideoCtrl.js"></script>
                                    
                                    <script src="./hk/jsVideoPlugin-1.0.0.min.js"></script>
                                    
      这3个文件；否则也会报错； 
      
      3. 项目拉下来后 重点看  \src\package\yg-video\hk-video 组件
      

# yg-vue3-init-demo

## Project setup
```
yarn install
```

### Compiles and hot-reloads for development
```
yarn serve
```

### Compiles and minifies for production
```
yarn build
```

### Lints and fixes files
```
yarn lint
```
![0711-02](https://github.com/331886820/yg-video/assets/29726492/bff2e327-0f12-47dd-a09d-54ca30fb59a7)

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).
