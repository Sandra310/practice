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


```
resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      'vue$': 'vue/dist/vue.esm.js',
      '@': resolve('src'),
    }
  },
```

