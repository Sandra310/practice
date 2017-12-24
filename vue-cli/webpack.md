## 跟着webpack官网配置
1. 从安装起步开始，管理输入为src/index.js，输出为[name].bundle.js 这样设置是为了防止入口文件为多个时，能够按照名字生成相应文件。
2. 增加了资源loader vue文件需要vue-loader、js文件需要用babel-loader进行ES6的转换、css文件使用style-loader与css-loader、其余是file-loader
3. 开发模式中，使用devtool: 'inline-source-map' 准确定位出错文件，启用模块热替换
4. 代码分离防止重复 CommonsChunkPlugin
5. 生产开发环境配置分离，使用merge融合公共配置
6. Resolve解析 resolve.alias设置别名 resolve.extensions 自动解析相应的扩展

### 初始设置目录树如下：
```
├── README.md
├── build
│   ├── webpack.common.js
│   ├── webpack.dev.js
│   └── webpack.prop.js
├── dist
├── index.html
├── package-lock.json
├── package.json
└── src
    ├── App.vue
    ├── assets
    │   └── logo.png
    ├── components
    ├── index.js
    ├── pages
    ├── router
    │   └── index.js
    └── views
        └── apple.vue
```

### 初始设置代码如下：
webpack.common.js
```
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');  // 生成html插件
const CleanWebpackPlugin = require('clean-webpack-plugin');  // 清除dist无用文件，重新打包  tip:只能作用于当前方式的打包，如果有历史包经过修改后重新打包，并不会清空该文件夹
const webpack = require('webpack');

function resolve(dir) {
  return path.join(__dirname, '..', dir)
}

module.exports = {
  entry: {
    app: './src/index.js',
    vendor: [
      'lodash'
    ]
  },
  output: {
    filename: '[name].bundle.js',
    path: path.resolve(__dirname, '../dist')
  },
  resolve: {
    extensions: ['.js', '.vue', '.json'],
    alias: {
      '@': resolve('src'),
    }
  },
  plugins: [
    new CleanWebpackPlugin(['dist']),
    new HtmlWebpackPlugin({
      title: 'Output Management',
      filename: 'index.html',
      template: 'index.html',
      inject: true //模板注入位置，bottom,true为底部，false,head 为顶部
    }),
    new webpack.HashedModuleIdsPlugin(), // 解决更改代码结果第三方库的hash也改变的问题
    new webpack.optimize.CommonsChunkPlugin({
      name: 'vendor'
    }),
    new webpack.optimize.CommonsChunkPlugin({ // 以将公共的依赖模块提取到已有的入口 chunk 中，或者提取到一个新生成的 chunk。
      name: 'common' // 指定公共bundle的名称
    })
  ],
  module: {
    rules: [
      {
        test: /\.vue$/,
        use: [
          'vue-loader'
        ]
      },
      {
        test: /\.js$/,
        loader: 'babel-loader',
        include: [resolve('src')]
      },
      {
        test: /\.css$/,
        use: [
          'style-loader',
          'css-loader'
        ]
      },
      {
        test: /\.(png|svg|jpg|gif)$/,
        use: [
          'file-loader'
        ]
      }
    ]
  }
};
```

webpack.dev.js
```
const merge = require('webpack-merge');
const common = require('./webpack.common');
const webpack = require('webpack');

module.exports = merge(common, {
  devtool: 'inline-source-map',  // 为了更容易地追踪错误和警告,JavaScript提供了source map功能，将编译后的代码映射回原始源代码。如果一个错误来自于 b.js，source map 就会明确的告诉你
  devServer: {
    contentBase: './dist',  // 用来自动编译自动刷新。告知 webpack-dev-server，在 localhost:8080 下建立服务，将 dist 目录下的文件，作为可访问文件。
    hot: true  // 模块热替换
  },
  plugins: [
    new webpack.NamedModulesPlugin(),  // 模块热替换  更容易查看要修补(patch)的依赖
    new webpack.HotModuleReplacementPlugin()  // 模块热替换
  ]
});
```

webpack.prop.js
```
const webpack = require('webpack')
const merge = require('webpack-merge');
const common = require('./webpack.common');
const UglifyJSPlugin = require('uglifyjs-webpack-plugin');  // 精简输出插件 import export标识出了未引用代码 能够删除未引用代码(dead code)的压缩工具(minifier)


module.exports = merge(common, {
  devtool: 'source-map',  // 鼓励在生产环境中启用 source map，因为它们对调试源码(debug)和运行基准测试(benchmark tests)很有帮助
  plugins: [
    new UglifyJSPlugin({
      sourceMap: true
    }), // 精简输出，删除无引用代码
    new webpack.DefinePlugin({
      'process.env': {
        'NODE_ENV': JSON.stringify('production')
      }
    })  // 判断环境
  ]
});
```
