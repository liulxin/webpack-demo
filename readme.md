# webpack demo

## 当前环境
```
$ node -v
v10.15.3
$ npm -v
6.4.1
```
## 安装
```
$ cnpm i webpack webpack-cli -D
$ npx webpack -v
4.34.0
```
## demo查看
```
$ cd c1
$ npm i
$ npm run dev
```
## c1: entry file

index.js
```
var test = 'test';

console.log(test)
```

webpack.config.js
```
const path = require('path');

module.exports = {
    mode: 'production',
    entry: './index.js',
    output: {
        filename: 'bundle.js'
    }
}
```
index.html
```
<!DOCTYPE html>
<html lang="en">
<body>
    <script src="./dist/bundle.js"></script>
</body>
</html>
```

## c2: multiple entry files
webpack.config.js
```
module.exports = {
    mode: 'production',
    entry: {
        index1: './src/index1.js',
        index2: './src/index2.js'
    },
    output: {
        filename: '[name].bundle.js'
    }
}
```

## c3: file-loader
webpack.config.js
```
const path = require('path');

module.exports = {
    mode: 'production',
    entry: {
        index: './src/js/index.js'
    },
    module: {
        rules: [
            {
                test: /\.(png|jpg|gif)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            name: '[name]_[hash:6].[ext]',
                            publicPath: './dist/images', //发布目录路径--其实就是打包文件中引用此文件的路径
                            outputPath: './images', //输出目录--相对于打包文件夹目录，当前是相对 dist 文件夹
                            useRelativePath: true
                        }
                    }
                ]
            },
            {
                test: /\.(ttf|eot|woff|woff2|svg)$/,
                use: [
                    {
                        loader: 'file-loader',
                        options: {
                            name: '[name]_[hash:6].[ext]',
                            publicPath: './dist/fonts', //发布目录路径--其实就是打包文件中引用此文件的路径
                            outputPath: './fonts' //输出目录--相对于打包文件夹目录，当前是相对 dist 文件夹
                        }
                    }
                ]
            },
            {
                test: /\.css$/,
                use: ['style-loader','css-loader']
            }
        ]
    },
    output: {
        filename: './js/[name].bundle.js',
    }
}
```
index.js
```
// import img from './logo@2x.png';
const img = require('../images/logo@2x.png');
import '../fonts/iconfont.css';

console.log(img);

var root = document.getElementById('root');
var img_ = new Image();
img_.src = img;
img_.onload = function() {
    root.appendChild(img_);
}
```
index.html
```
<!DOCTYPE html>
<html lang="en">
<head>
</head>
<body>
    <div id="root"></div>
    <div class="img"></div>
    <i class="iconfont icon-file"></i>
    <script src="./dist/js/index.bundle.js"></script>
</body>
</html>
```
