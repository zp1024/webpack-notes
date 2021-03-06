# webpack核心概念简介

[Webpack](https://webpack.github.io/) 是一个模组打包工具（module bundler），它可以将许多模块封装成几个捆绑资源。

webpack 允许根据需要去加载应用程序的部件。使得 Javascript 应用可以高度复用。

学习 webpack ，你最先需要了解的是四个核心概念：`entry`、`output`、`loaders`、`plugins` 。

## Entry

webpack 将创建所有应用程序的依赖关系，而关系的起点被称为入口点（entry point）。

入口点会告诉 webpack 在哪里开始，并按照依赖关系来指导如何去捆绑。

webpack 的 entry 属性就是用来描述入口点的。

**webpack.config.js**

```javascript
module.exports = {
  entry: './path/to/my/entry/file.js'
};
```

> **总结：`entry` 定义 webpack 的输入。**

## Output

当你将所有资源捆绑在一起后，还需要告诉 webpack 在哪里捆绑我们的应用程序。 

webpack 的 `output` 属性就是用来描述描述如何处理捆绑代码。

**webpack.config.js**

```javascript
var path = require('path');

module.exports = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  }
};
```

> **总结：`output` 定义 webpack 的输出。**

## Loaders

webpack 将每种文件（.css，.html，.scss，.jpg等）作为一个模块。但是，**webpack 本身只能理解 JavaScript**。

`加载器（Loaders）` 的作用就是：帮助 webpack 将这些文件转换为模块，并将其添加到依赖关系中。

**webpack.config.js**

```javascript
var path = require('path');

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    path: path.resolve(__dirname, 'dist'),
    filename: 'my-first-webpack.bundle.js'
  },
  module: {
    rules: [
      {test: /\.(js|jsx)$/, use: 'babel-loader'}
    ]
  }
};

module.exports = config;
```

如上例所示，我们为模块定义了 `rules` 属性，来指定如何使用加载器。

- `test` 属性表示加载器应该处理的文件类型。
- `use` 属性表示使用的加载器。

> **总结：webpack 处理不了的文件使用加载器（`Loaders`）来转译。**

## Plugins

加载器只能在独立的文件上进行转译操作，而对于涉及多文件的操作就无能为力了。

所以，你还需要使用 webpack 插件。插件最常用在（但不限于）捆绑模块的 “compilations” 或 “chunks” 功能上。

使用插件，你只需要使用 `require` 引用它。

**webpack.config.js**

```javascript
const HtmlWebpackPlugin = require('html-webpack-plugin'); //installed via npm
const webpack = require('webpack'); //to access built-in plugins

const config = {
  entry: './path/to/my/entry/file.js',
  output: {
    filename: 'my-first-webpack.bundle.js',
    path: './dist'
  },
  module: {
    rules: [
      {test: /\.(js|jsx)$/, use: 'babel-loader'}
    ]
  },
  plugins: [
    new webpack.optimize.UglifyJsPlugin(),
    new HtmlWebpackPlugin({template: './src/index.html'})
  ]
};

module.exports = config;
```

> **总结：加载器（`Loaders`）只能处理单个文件。加载器（`Loaders`）不能完成的功能使用插件（`Plugins`）**