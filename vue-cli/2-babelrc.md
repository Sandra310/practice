## .babelrc 配置

### webpack 补充1
书接上文，webpack有个插件，[HtmlWebpackPlugin](https://github.com/jantimon/html-webpack-plugin#configuration)用来生成html文件。如果默认new HtmlWebpackPlugin() 仅仅能生成一个基础的html文件，无法做到提供<div id=app></div> 插入口。因此利用配置中的模板，如下是好用的写法。
```
new HtmlWebpackPlugin({
      title: 'Output Management',
      filename: 'index.html',
      template: 'index.html',
      inject: true //模板注入位置，bottom,true为底部，false,head 为顶部
    }),
```
写这个配置踩了坑，首先是必须要有一个模板，再次，一定要设置inject，因为如果默认，webpack会把js打包在body的头部，这样会报错识别不到app

### webpack 补充2


报错：
[Vue warn]: You are using the runtime-only build of Vue where the template option is not available. Either pre-compile the templates into render functions, or use the compiler-included build.

解决办法： 'vue$': 'vue/dist/vue.esm.js',不明白为什么，这篇链接有讲原因，但是并不能看懂啊 http://www.imooc.com/article/17868
```
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
```

## babelrc
### 初始设置
`"presets": [ "es2015" ]` presets 字段设定转码规则，根据需要安装
```
# ES2015转码规则
$ npm install --save-dev babel-preset-es2015

# react转码规则
$ npm install --save-dev babel-preset-react

# ES7不同阶段语法提案的转码规则（共有4个阶段），选装一个
$ npm install --save-dev babel-preset-stage-0
$ npm install --save-dev babel-preset-stage-1
$ npm install --save-dev babel-preset-stage-2
$ npm install --save-dev babel-preset-stage-3
```

相对应的.babelrc
```
 {
    "presets": [
      "es2015",
      "react",
      "stage-2"
    ],
    "plugins": []
  }
```
### 路由懒加载
`const Home = () => import('@/views/apple')` 如果使用是babel，需要添加 [syntax-dynamic-import](https://babeljs.io/docs/plugins/syntax-dynamic-import/) 插件，才能使 Babel 可以正确地解析语法。
npm后添加.babelrc文件
```
{
  "presets": [ "es2015" ],
  "plugins": [ "syntax-dynamic-import" ]
}
```






### 参考
http://www.ruanyifeng.com/blog/2016/01/babel.html
