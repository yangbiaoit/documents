# documents


## NODE_ENV=production 环境变量怎么设置
windows 下 <br/>

```javascript
 "scripts": {
        "test": "set process.env.NODE_ENV=production  mocha --compilers js:babel-core/register --recursive",
        "test:watch": "set process.env.NODE_ENV=production  mocha --compilers js:babel-core/register --recursive --watch"
  }
  ``` 
  
非windows 下 <br/>
```javascript
 "scripts": {
        "test": "NODE_ENV=production  mocha --compilers js:babel-core/register --recursive",
        "test:watch": "NODE_ENV=production  mocha --compilers js:babel-core/register --recursive --watch"
  }
  ```
  
 webpack.config.js  
  
  plugins 
   ```
    var commonsPlugin = new webpack.optimize.CommonsChunkPlugin('common.js');
   
    ...
    plugins: [commonsPlugin]
    ...
   ```
   plugins 是插件项，这里我们使用了一个 CommonsChunkPlugin 的插件，它用于提取多个入口文件的公共脚本部分，然后生成一个 common.js 来方便多页面之间进行复用。
 
 
  entry可以是个字符串或数组或者是对象。
  
   当entry是个字符串的时候，用来定义入口文件： 
  ```
   entry: './js/main.js'
  ```
   当entry是个数组的时候，里面同样包含入口js文件，另外一个参数可以是用来配置webpack提供的一个静态资源服务器，webpack-dev-server。webpack-dev-server会监控项目中每一个文件的变化，实时的进行构建，并且自动刷新页面： 
  ```
  entry: [
     'webpack/hot/only-dev-server',
     './js/app.js'
  ]
  ```
  当entry是个对象时，可以将不同的文件构建成不同的文件按需使用，比如在hello页面中只要<script src='build/Profile.js'></script>
  引入hello.js即可： 
  ```
  entry: {
     hello: './js/hello.js',
     form: './js/form.js'
 }
  ```
  
  entry 与 output结合
   ```
    entry: {
        page1: "./page1",
        //支持数组形式，将加载数组中的所有模块，但以最后一个模块作为输出
        page2: ["./entry1", "./entry2"]
    },
    output: {
        path: "dist/js/page",
        filename: "[name].bundle.js"
    }
   ```
   该段代码最终会生成一个 page1.bundle.js 和 page2.bundle.js，并存放到 ./dist/js/page 文件夹下。 
  
  
  webpack在构建的时候会按目录进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀,然后想要加载一个js文件时，只要require('common')就可以加载common.js文件了。
  ```
  resolve: {
        //查找module的话从这里开始查找
        root: '/pomy/github/flux-example/src', //绝对路径
        
        //自动扩展文件后缀名，意味着require模块可以省略不写后缀名
        extensions: ['', '.js', '.json', '.scss'],
        
        //模块别名定义，方便后续直接引用别名，无须多写长长的地址
        alias: {
            AppStore  : 'js/stores/AppStores.js',//后续直接 require('AppStore') 即可
            ActionType : 'js/actions/ActionType.js',
            AppAction : 'js/actions/AppAction.js'
        }
    }
   ```
  
  Webpack 的配置提供了 resolve 和 resolveLoader 参数来设置模块解析的处理细节，resolve 用来配置应用层的模块（要被打包的模块）解析，resolveLoader 用来配置 loader 模块的解析。

当引入通过 npm 安装的 node.js 模块时，可能出现找不到依赖的错误。Node.js 模块的依赖解析算法很简单，是通过查看模块的每一层父目录中的 node_modules 文件夹来查询依赖的。当出现 Node.js 模块依赖查找失败的时候，可以尝试设置 resolve.fallback 和 resolveLoader.fallback 来解决问题。
  ```
  resolve: {
    alias: {
      'redux-devtools': path.join(__dirname, '..', '..', 'src'),
      'react': path.join(__dirname, 'node_modules', 'react')
    }
  },
  resolveLoader: {
    'fallback': path.join(__dirname, 'node_modules')
  },
  ```
  
   关于模块(Module)加载，我们就定义在module.loaders中。这里通过正则表达式去匹配不同后缀的文件名，然后给它们定义不同的加载器。比如说给less文件定义串联的三个加载器（！用来定义级联关系）： 
  ```
  module: {
    loaders: [
        { test: /\.js?$/, loaders: ['react-hot', 'babel'], exclude: /node_modules/ },
        { test: /\.js$/, exclude: /node_modules/, loader: 'babel-loader'},
        { test: /\.css$/, loader: "style!css" },
        { test: /\.less/, loader: 'style-loader!css-loader!less-loader'},
        { test: /\.(png|jpg)$/,loader: 'url-loader?limit=10000'} 
    ]
}
```
  还可以添加用来定义png、jpg这样的图片资源在小于10k时自动处理为base64图片的加载器： 
  给css和less还有图片添加了loader之后，不仅可以像在node中那样require js文件了，还可以require css、less甚至图片文件：  
  [PS:这里的loader是可以省略掉 -loader 这样的，也就是原本应该写成 style-loader!css-loader!sass-loader]
  [PS:!级联关系 如 !style!css!sass 先用sass处理，然后用css处理，最后用style处理]
 在上面示例代码中配置的第一个loaders我们可以看到一个叫做react-hot的加载器。我的项目是用来学习react写相关代码的，所以配置了一个react-hot加载器，通过它，可以实现对react组件的热替换。我们已经在entry参数中配置了`webpack/hot/only-dev-server`,所以我们只要在启动webpack开发服务器时开启--hot参数，就可以使用react-hot-loader了。在package.json文件中这样定义： 
 ```
 "scripts": {
     "start": "webpack-dev-server --hot --progress --colors",
     "build": "webpack --progress --colors"
 }
```
 plugin webpack提供了[丰富的组件](http://webpack.github.io/docs/list-of-plugins.html)用来满足不同的需求，当然我们也可以自行实现一个组件来满足自己的需求。若项目里面没有特殊的需求，于是便只是配置了NoErrorsPlugin插件，用来跳过编译时出错的代码并记录，使编译后运行时的包不会发生错误： 
```
 plugins: [
     new webpack.NoErrorsPlugin()
 ]
```

externals
当我们想在项目中require一些其他的类库或者API，而又不想让这些类库的源码被构建到运行时文件中，这在实际开发中很有必要。此时我们就可以通过配置externals参数来解决这个问题：
```
 externals: {
     "jquery": "jQuery"
 }
```
这样我们就可以放心的在项目中使用这些API了：var jQuery = require("jquery");


shimming 在 AMD/CMD 中，我们需要对不符合规范的模块（比如一些直接返回全局变量的插件）进行 shim 处理，这时候我们需要使用 exports-loader
```
{ test: require.resolve("./src/js/tool/swipe.js"),  loader: "exports?swipe"}
```
之后在脚本中需要引用该模块的时候，这么简单地来使用就可以了：
```
require('./tool/swipe.js');
swipe();
```


自定义公共模块提取
在文章开始我们使用了 CommonsChunkPlugin 插件来提取多个页面之间的公共模块，并将该模块打包为 common.js 。
但有时候我们希望能更加个性化一些，我们可以这样配置：
```
var CommonsChunkPlugin = require("webpack/lib/optimize/CommonsChunkPlugin");
module.exports = {
    entry: {
        p1: "./page1",
        p2: "./page2",
        p3: "./page3",
        ap1: "./admin/page1",
        ap2: "./admin/page2"
    },
    output: {
        filename: "[name].js"
    },
    plugins: [
        new CommonsChunkPlugin("admin-commons.js", ["ap1", "ap2"]),
        new CommonsChunkPlugin("commons.js", ["p1", "p2", "admin-commons.js"])
    ]
};
// <script>s required:
// page1.html: commons.js, p1.js
// page2.html: commons.js, p2.js
// page3.html: p3.js
// admin-page1.html: commons.js, admin-commons.js, ap1.js
// admin-page2.html: commons.js, admin-commons.js, ap2.js
```




[原文](http://www.cnblogs.com/Leo_wl/p/4862714.html)
[原文](http://blog.csdn.net/yczz/article/details/49250623)

  
  
  





