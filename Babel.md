## babel是什么
Babel是一个编译器，能将ES6的代码转换成ES5，让开发者在旧版浏览器上使用新语法；

## babel转换什么
Babel对不属于ES5的语法做了转换，例如：
- for-of
- 箭头函数
- 块级作用域
- let/const
...

## babel不转换什么
- 一些内置的全局变量，例如：Promise、Symbol、Set、WeakMap
- 一些Object新增的方法，如： Array.includes、Object.assign

## 如何处理babel不转换的内容
- babel-polyfill

引入babel-polyfill，它会检测运行环境是否支持相应的方法，如果不支持，会创建全局的新增方法；
- babel-runtime

babel-runtime也能做到，与babel-polyfill不同的是它不会污染全局作用域，它会导出一个module给使用的地方；

## babel相关包
- babel-core —— babel的核心包；
- babel-loader —— babel的loader包；
- babel-preset-es2015 —— 解析es6的包；
- babel-preset-env —— 解析es6的包；（官方最新推荐）
- babel-preset-react —— 解析React的JSX的包；

## webpack配置babel
package.json增加相关依赖包：
```
"devDependencies": {
  "@babel/core": "^7.4.4",
  "@babel/preset-env": "^7.4.4",
  "babel-loader": "^8.0.0-beta.0",
  "webpack": "^4.31.0",
  "webpack-cli": "^3.3.2"
}
```
webpack.config.js中配置babel-loader:
```
module: {
  rules: [
    {
      test: /\.js$/,
      exclude: /node_modules/,
      use: {
        loader: 'babel-loader',
      }
    }
  ]
}
```
.babelrc中配置解析器
```
{
  "presets": ["@babel/preset-env"],
}
```