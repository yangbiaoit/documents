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
  
 webpack.config.js entry 
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
  
  webpack在构建的时候会按目录进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀,然后想要加载一个js文件时，只要require('common')就可以加载common.js文件了。
  ```
  resolve:{
     extensions:['','.js','.json']
 }
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
1 externals: {
2     "jquery": "jQuery"
3 }
```
这样我们就可以放心的在项目中使用这些API了：var jQuery = require("jquery");


[原文](http://www.cnblogs.com/Leo_wl/p/4862714.html)


  
  
  





