## ***1 webpack安装***
首先新建一个练习文件夹demo，在文件中打开命令终端，输入下列指令即可安装webpack
```
//全局安装
npm install -g webpack
```

安装完之后，demo里会多一个node_modules文件夹和package-lock.json文件。
 
接下来输入
`npm init`
会自动创建package.json文件。安装的时候一路回车即可
 
然后全局安装webpack-cli
`npm install -g webpack-cli`
 
## ***2 创建文件***
在项目根目录新建一个文件hello.js，
 
并在其中输入代码：
```
function hello(str) {
    alert(str);
}
hello('hello world!');
```
尝试打包：
`webpack hello.js bundle.js`
出现了提示：
```
 WARNING in configuration
The 'mode' option has not been set. Set  'mode' option to 'development' or 'production' to enable defaults for this enviroment.
ERROR in multi ./hello.js bundle.js
Module not found:ERROR:Can't resolve 'bundle.js' in 'C:/Users/你的用户名/Desktop/webpack-test'
@ multi ./hello.js bundle.js
```
翻译成中文：
```
配置警告： 
“mode”选项尚未设置。将“mode”选项设为“development”或“production”以启用此环境的默认设置。
multi错误 ./ hello.js bundle.js
未发现模块：错误：无法解析’C:/Users/你的用户名/Desktop/webpack-test’中的bundle.js
@ multi ./hello.js bundle.js
```

## ***3 设置mode***
这里提示我们存在的第一个问题是没有配置webpack的mode选项，默认有production和development两种模式可以设置，因此我们尝试设为development模式，在命令行输入：
```
webpack --mode development
或者
webpack --mode production
```
我们看到进行了打包并显示了Hash、Version、Time、Build at信息，表明已经可以打包。不过，仍然有错误提示：
` ERROR in Entry module not found:ERROR:Can't resolve './src' in 'C:/Users/你的用户名/Desktop/webpack-test'`
翻译成中文：
`未找到入口模块发生错误：错误：无法解析’C:/Users/你的用户名/Desktop/webpack-test’中的’./src’`
## ***4 创建入口文件***
这表明webpack4.x是以项目根目录下的'./src'作为入口，但我们的项目中缺乏该路径，因此我们在根目录下创建src文件夹，事实上webpack4.x以'./src/index.js'作为入口，单单创建src文件而没有index.js文件仍然会报错，因此我们将hello.js移动到'./src'，并重命名为index.js
再次执行
`webpack index.js bundle.js`
会提示can.t resolve相关的错误
 
原因是，webpack4.x的打包已经不能用webpack 文件a 文件b的方式，而是直接运行webpack --mode development或者webpack --mode production，这样便会默认进行打包，入口文件是'./src/index.js'，输出路径是'./dist/main.js'，其中src目录即index.js文件需要手动创建，而dist目录及main.js会自动生成。 
因此我们不再按webpack 文件a 文件b的方式运行webpack指令，而是直接运行
```
webpack --mode development
或者
webpack --mode production
```

可以看见项目中的dist文件夹多了一个main.js的文件，该文件就是index.js打包后的文件
 


# ***任务描述***
## **1.支持js、css格式的解析**
Webpack 4本就支持js格式的解析，不需要再下载任何插件。

***解析css格式：***
下载安装style-loader和css-loader
`npm install --save-dev style-loader css-loader`
 
下载html-webpack-plugin
`npm install html-webpack-plugin --save-dev`
 
然后在src文件夹中添加一个index.html文件、一个a.js文件、css文件夹和js文件夹，将index.js移动到js文件夹中，在css文件夹中新建一个a.css文件，
 
a.css代码：
```
 body {
    background: #ccc;
}
```
a.js代码：
```
 require('./css/a.css');
require('./js/index.js');
```
并在项目文件夹内新建一个webpack.config.js文件
```
webpack.config.js代码：
const path = require('path');
const HtmlWebpackPlugin = require('html-webpack-plugin');
const Webpack = require('webpack');

module.exports = {
    entry:{
        entry: './src/a.js'
    }, 
    output:{
        path:path.resolve(__dirname,'dist'),
        filename:'[name]-bundle.js' 
    },
    module:{ 
        //配置一个rules(规则),rules是一个数组,里面包含一条一条的规则
        rules:[
            {
                // test 表示测试什么文件类型
                test:/.css$/,
                // 使用 'style-loader','css-loader'
                loader:['style-loader','css-loader']
            }
        ]
    },
    devServer:{
        contentBase:path.resolve(__dirname,'dist'), //最好设置成绝对路径
        host:'localhost',
        port:8090,
        open:true,
        hot:true
    },
    plugins:[
        new Webpack.HotModuleReplacementPlugin(),
        new HtmlWebpackPlugin({
            title:'Hello World',
            template: './src/index.html' //模板地址
        })
    ]
}
```
执行`webpack --mode development`，
 
dist文件夹中出现entry-bundle.js和index.html两个文件，其中entry-bundle.js为webpack打包css和js文件后生成的文件，index.html为打包html文件后生成的文件
 

## **2.区分 development / production 环境**
1.	生产能够实现各种优化以生成优化的捆绑包 
2.	生产不支持观看，开发针对快速增量重建进行了优化 
3.	生产也使模块连接（范围提升） 
4.	process.env.NODE_ENV 被设置为生产或开发（仅在构建代码中，而不是在配置中）

## **3.使用npm scripts设罝dev、test、build命令**
package.json中scripts中加入两个成员：
```
"dev":"webpack --mode development",
"build":"webpack --mode production"
```
 
以后我们只需要在命令行执行npm run dev便相当于执行webpack --mode development，执行npm run build便相当于执行webpack --mode production。 

## **4.写一个san组件并在浏览器中显示hello world**
参考san文档即能实现

## **5.使用babel-loader进行js代码转换**
使用命令 
`npm install --save-dev babel-loader babel-core`
 
因为ES6语法每年都在更新，因此，我们需要一定的规则去转换。
`npm install --save-dev babel-preset-env`
 
上面仅仅是安装了三个包，如果要使babel起作用，便需要配置babel规则。 
　　在package.json文件中增加一个“babel"属性，该属性是一个JSON对象，作用是设置项目中的babel转码规则和使用到的babel插件，其基本格式如下：
```
"babel":{
  "presets": ["env"],
  "plugins": []
}
```
//”presets”属性字段设定转码规则，”plugins”属性设置使用到的插件
    上面的设置告诉npm本项目将使用babel，并且使用bable-preset-env规则进行转码，即实现对ES2015+语法进行转码。
在webpack.config.js中的rules中添加一条规则：
```
{
     test: /.js$/,
     exclude: /node_modules/, 
     loader: "babel-loader"
}
```
这就告诉webpack打包时，一旦匹配到.js文件就使用babel-loader进行处理，babel-loader调用babel-core的api使用bable-preset-env的规则进行转码。
