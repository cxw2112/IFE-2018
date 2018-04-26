## ***1 webpack��װ***
�����½�һ����ϰ�ļ���demo�����ļ��д������նˣ���������ָ��ɰ�װwebpack
```
//ȫ�ְ�װ
npm install -g webpack
```

��װ��֮��demo����һ��node_modules�ļ��к�package-lock.json�ļ���
 
����������
`npm init`
���Զ�����package.json�ļ�����װ��ʱ��һ·�س�����
 
Ȼ��ȫ�ְ�װwebpack-cli
`npm install -g webpack-cli`
 
## ***2 �����ļ�***
����Ŀ��Ŀ¼�½�һ���ļ�hello.js��
 
��������������룺
```
function hello(str) {
    alert(str);
}
hello('hello world!');
```
���Դ����
`webpack hello.js bundle.js`
��������ʾ��
```
 WARNING in configuration
The 'mode' option has not been set. Set  'mode' option to 'development' or 'production' to enable defaults for this enviroment.
ERROR in multi ./hello.js bundle.js
Module not found:ERROR:Can't resolve 'bundle.js' in 'C:/Users/����û���/Desktop/webpack-test'
@ multi ./hello.js bundle.js
```
��������ģ�
```
���þ��棺 
��mode��ѡ����δ���á�����mode��ѡ����Ϊ��development����production�������ô˻�����Ĭ�����á�
multi���� ./ hello.js bundle.js
δ����ģ�飺�����޷�������C:/Users/����û���/Desktop/webpack-test���е�bundle.js
@ multi ./hello.js bundle.js
```

## ***3 ����mode***
������ʾ���Ǵ��ڵĵ�һ��������û������webpack��modeѡ�Ĭ����production��development����ģʽ�������ã�������ǳ�����Ϊdevelopmentģʽ�������������룺
```
webpack --mode development
����
webpack --mode production
```
���ǿ��������˴������ʾ��Hash��Version��Time��Build at��Ϣ�������Ѿ����Դ������������Ȼ�д�����ʾ��
` ERROR in Entry module not found:ERROR:Can't resolve './src' in 'C:/Users/����û���/Desktop/webpack-test'`
��������ģ�
`δ�ҵ����ģ�鷢�����󣺴����޷�������C:/Users/����û���/Desktop/webpack-test���еġ�./src��`
## ***4 ��������ļ�***
�����webpack4.x������Ŀ��Ŀ¼�µ�'./src'��Ϊ��ڣ������ǵ���Ŀ��ȱ����·������������ڸ�Ŀ¼�´���src�ļ��У���ʵ��webpack4.x��'./src/index.js'��Ϊ��ڣ���������src�ļ���û��index.js�ļ���Ȼ�ᱨ��������ǽ�hello.js�ƶ���'./src'����������Ϊindex.js
�ٴ�ִ��
`webpack index.js bundle.js`
����ʾcan.t resolve��صĴ���
 
ԭ���ǣ�webpack4.x�Ĵ���Ѿ�������webpack �ļ�a �ļ�b�ķ�ʽ������ֱ������webpack --mode development����webpack --mode production���������Ĭ�Ͻ��д��������ļ���'./src/index.js'�����·����'./dist/main.js'������srcĿ¼��index.js�ļ���Ҫ�ֶ���������distĿ¼��main.js���Զ����ɡ� 
������ǲ��ٰ�webpack �ļ�a �ļ�b�ķ�ʽ����webpackָ�����ֱ������
```
webpack --mode development
����
webpack --mode production
```

���Կ�����Ŀ�е�dist�ļ��ж���һ��main.js���ļ������ļ�����index.js�������ļ�
 


# ***��������***
## **1.֧��js��css��ʽ�Ľ���**
Webpack 4����֧��js��ʽ�Ľ���������Ҫ�������κβ����

***����css��ʽ��***
���ذ�װstyle-loader��css-loader
`npm install --save-dev style-loader css-loader`
 
����html-webpack-plugin
`npm install html-webpack-plugin --save-dev`
 
Ȼ����src�ļ��������һ��index.html�ļ���һ��a.js�ļ���css�ļ��к�js�ļ��У���index.js�ƶ���js�ļ����У���css�ļ������½�һ��a.css�ļ���
 
a.css���룺
```
 body {
    background: #ccc;
}
```
a.js���룺
```
 require('./css/a.css');
require('./js/index.js');
```
������Ŀ�ļ������½�һ��webpack.config.js�ļ�
```
webpack.config.js���룺
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
        //����һ��rules(����),rules��һ������,�������һ��һ���Ĺ���
        rules:[
            {
                // test ��ʾ����ʲô�ļ�����
                test:/.css$/,
                // ʹ�� 'style-loader','css-loader'
                loader:['style-loader','css-loader']
            }
        ]
    },
    devServer:{
        contentBase:path.resolve(__dirname,'dist'), //������óɾ���·��
        host:'localhost',
        port:8090,
        open:true,
        hot:true
    },
    plugins:[
        new Webpack.HotModuleReplacementPlugin(),
        new HtmlWebpackPlugin({
            title:'Hello World',
            template: './src/index.html' //ģ���ַ
        })
    ]
}
```
ִ��`webpack --mode development`��
 
dist�ļ����г���entry-bundle.js��index.html�����ļ�������entry-bundle.jsΪwebpack���css��js�ļ������ɵ��ļ���index.htmlΪ���html�ļ������ɵ��ļ�
 

## **2.���� development / production ����**
1.	�����ܹ�ʵ�ָ����Ż��������Ż�������� 
2.	������֧�ֹۿ���������Կ��������ؽ��������Ż� 
3.	����Ҳʹģ�����ӣ���Χ������ 
4.	process.env.NODE_ENV ������Ϊ�����򿪷������ڹ��������У��������������У�

## **3.ʹ��npm scripts���Ddev��test��build����**
package.json��scripts�м���������Ա��
```
"dev":"webpack --mode development",
"build":"webpack --mode production"
```
 
�Ժ�����ֻ��Ҫ��������ִ��npm run dev���൱��ִ��webpack --mode development��ִ��npm run build���൱��ִ��webpack --mode production�� 

## **4.дһ��san����������������ʾhello world**
�ο�san�ĵ�����ʵ��

## **5.ʹ��babel-loader����js����ת��**
ʹ������ 
`npm install --save-dev babel-loader babel-core`
 
��ΪES6�﷨ÿ�궼�ڸ��£���ˣ�������Ҫһ���Ĺ���ȥת����
`npm install --save-dev babel-preset-env`
 
��������ǰ�װ�������������Ҫʹbabel�����ã�����Ҫ����babel���� 
������package.json�ļ�������һ����babel"���ԣ���������һ��JSON����������������Ŀ�е�babelת������ʹ�õ���babel������������ʽ���£�
```
"babel":{
  "presets": ["env"],
  "plugins": []
}
```
//��presets�������ֶ��趨ת����򣬡�plugins����������ʹ�õ��Ĳ��
    ��������ø���npm����Ŀ��ʹ��babel������ʹ��bable-preset-env�������ת�룬��ʵ�ֶ�ES2015+�﷨����ת�롣
��webpack.config.js�е�rules�����һ������
```
{
     test: /.js$/,
     exclude: /node_modules/, 
     loader: "babel-loader"
}
```
��͸���webpack���ʱ��һ��ƥ�䵽.js�ļ���ʹ��babel-loader���д���babel-loader����babel-core��apiʹ��bable-preset-env�Ĺ������ת�롣
