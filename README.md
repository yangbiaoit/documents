# documents
用于存放一些文档


##NODE_ENV=production 环境变量怎么设置
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
  
  
  
  
  
