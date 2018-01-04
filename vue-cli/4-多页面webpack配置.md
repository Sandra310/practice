## 多页面配置webpack
多页面配置时，打包入口文件、出口文件可以相应设置多个，但是模板HtmlWebpackPlugin配置就有讲究了；<br>
需要生成几个html文件，就配置几个HtmlWebpackPlugin对象。所以通常采取遍历html文件来配置该HtmlWebpackPlugin。<br>
若默认配置，则会发现所有a.js、b.js等等都将引入到html中，这是不合理的，那么有两种方式可以移除不属于自己的js <br>
excludeChunks 允许跳过某些chunks, 而chunks告诉插件要引用entry里面的哪几个入口 <br>
方法一：设置excludeChunks
```
excludeChunks: Object.keys(pages).filter(item => {
  return (item != page)
})
```
方法二：设置chunks
```
chunks: ['manifest', 'vendor', filename],
```

## 配置公共模块
