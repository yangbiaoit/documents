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
   当entry是个对象的时候，我们可以将不同的文件构建成不同的文件，按需使用，比如在我的hello页面中只要\<script src='build/Profile.js'></script>引入hello.js即可： 
  ```
  entry: {
     hello: './js/hello.js',
     form: './js/form.js'
 }
  ```
  
  webpack在构建包的时候会按目录的进行文件的查找，resolve属性中的extensions数组中用于配置程序可以自行补全哪些文件后缀：然后我们想要加载一个js文件时，只要require('common')就可以加载common.js文件了。 
  ```
  resolve:{
     extensions:['','.js','.json']
 }
   ```
   
  
  
  





