# vue-manage-system

## Project setup
```
npm install
```

### Compiles and hot-reloads for development
```
npm run serve
```

### Compiles and minifies for production
```
npm run build
```

### Lints and fixes files
```
npm run lint
```

### Customize configuration
See [Configuration Reference](https://cli.vuejs.org/config/).

### 请安装以下工具
+ 使用了mockjs
+ 使用了axios

### 第一天
1. 配置vue.config.js文件
2. 配置vscode的中eslint，需要安装Eslint，prettier，Vetor三个插件
3. 进入setting.json文件进行配置
4. 清理项目，删除多余组件，路由
5. 下载element-ui依赖包，yarn add element-ui -S
6. 创建了CommonAside,CommonHeader组件并引入到Main.vue组件中完毕

### 第二天
1. 创建CommonTab组件
2. 在Main.vue中添加tab标签
3. 为侧边栏每个选项列表渲染添加点击事件触发vuex中的事件
4. 将点击的选项渲染到标签中