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