## 在终端输出
```
npm init -y

npm install --save-dev @babel/core @babel/cli @babel/preset-env

npx babel ./src/index.js --presets=@babel/preset-env
```

或则：
npm run log

就可以输出终端es5

## 在webpack中使用
```
npm i webpack webpack-cli -D

npm i babel-loader -D

npm run build
```

在dist输出文件


## 用@babel/polyfill使低版本浏览器也支持所有ES6的语法

```
npm install --save core-js@3
```

在低版本浏览器还是没有比如Promise、数组的map等。所以不仅要使用@babel/preset-env进行ES6转ES5，还要借助 @babel/polyfill把缺失的变量或者函数补充到低版本的浏览器里。
```
这将模拟一个完整的ES2015+环境（即ES6） (没有< Stage 4提案)，并将用于应用程序而不是库/工具。(使用babel-node时自动加载此polyfill)。
这意味着您可以使用新的内置函数，如Promise或WeakMap，静态方法如Array.from或 Object.assign，实例方法比如Array.prototype等。为了做到这一点，polyfill添加了全局范围和原生原型，比如String。
使用polyfill后，打包后的这个入口文件大小相比不使用时要大的多一些了（因为相当于配置了一个完整的ES6环境）。
如果只想补充代码中使用到的ES6语法，而不是把所有的一股脑全补充，那么建议对preset使用useBuiltIns: "usage"（如上方代码里）
```

不应该使用：-D
npm install @babel/polyfill -D

polyfill(它将在您的源代码之前运行)，所以我们需要它是一个依赖项，而不是devDependency,直接使用依赖项:
```
npm install --save @babel/polyfill
```

### babel7的新功能，useBuiltIns,它有三个参数可选：
```js
// 需要在 webpack 的⼊⼝⽂件⾥ import "@babel/polyfill" ⼀次。 babel 会根据你的使⽤情况导⼊垫⽚，没有使⽤的功能不会被导⼊相应的垫⽚
useBuiltIns: "entry",  

// 按需注⼊,不需要 import ，全⾃动检测，但是要安装 @babel/polyfill 
useBuiltIns: "usage",  

// 如果你 import "@babel/polyfill" ，它不会排除掉没有使⽤的垫⽚，程序体积会庞⼤。(不推荐)
useBuiltIns: "false" 
```

第一种：在webpack.config.js中配置
```js
options: {
    presets: [
        [
            "@babel/preset-env",
            {
                targets: {
                    edge: "17",
                    firefox: "60",
                    chrome: "67",
                    safari: "11.1"
                },
            corejs: 2,// 新版本需要指定核⼼库版本
            useBuiltIns: "usage"//按需注⼊
            }
        ]
    ]
}
```

第二种: 新建.babelrc⽂件，把options部分移⼊到该⽂件中
```js
{
    presets: [
        [
            "@babel/preset-env",
            {
                targets: {
                    edge: "17",
                    firefox: "60",
                    chrome: "67",
                    safari: "11.1"
                },
                corejs: 2, // 新版本需要指定核⼼库版本
                useBuiltIns: "usage" // 按需注⼊
            }
        ]
    ]
}
```

```js
      {
        "targets": {
          "edge": "17",
          "firefox": "60",
          "chrome": "67",
          "safari": "11.1"
        },
        "corejs": 2,
        "useBuiltIns": "usage"
      }
```


## 参考:polyfill配置
用了babel还需要polyfill
[用了babel还需要polyfill](https://juejin.cn/post/6845166891015602190)