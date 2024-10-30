# 1.fs模块

```html
<script>
	// 1. 加载fs模块对象
    const fs = require('fs')
    // 2. 写入文件内容 fs.writeFile('文件路径','内容',回调函数)
    fs.writeFile('./test.txt','hello,Node.js',(err) => {
        if (err) console.log(err)
        else console.log('写入成功')
    })
    // 3. 读取文件内容 fs.readFile('文件路径',回调函数)
    fs.readFile('./test.txt',(err,data) => {
        if (err) console.log(err)
        // data是buffer16进制数据流对象，用toString()转换成字符串
        else console.log(data.toString())
    })
</script>
```

# 2.path模块

```html
<script>
	// 1. 引入path模块对象
    const path = require('path')
    // 2. 调用path.join()配合__dirname组成目标文件的绝对路径
    console.log(__dirname)
    console.log(path.join(__dirname,'../test.txt'))
</script>
```

# 3.http模块

```html
<script>
	// 1. 加载http模块，创建Web服务对象
    const http = require('http')
    const server = http.createServer()
    
    // 2. 监听request请求事件，设置响应头和响应体
    server.on('request',(req,res) => {
        // 设置响应头 内容类型：普通文本以及中文编码格式
        res.setHeader('Content-Type','text/plain;charset=utf-8')
        // 设置响应体内容，结束本次请求与响应
        res.end('欢迎使用 Web服务')
    })
    
    // 3. 配置端口号启动web服务
    server.listen(3000,() => {
    	console.log('Web服务启动成功')
    })
</script>
```

# 4.模块化

CommonJS标准

```html
<script>
	// 1. 导出 在文件utils.js中写
    const baseURL = 'http://hmajax.itheima.net'
     module.exports = {
         url:baseURL
     }
    
    // 2. 导入 在其他文件写
    const obj = require('./utils.js')
    console.log(obj)
</script>
```

ECMASript标准默认导出导入

```html
<script>
	// 1. 在运行模块文件夹内创建package.json文件写入
    {"type":"module"}
    
    // 2. 导出 在文件utils.js中写
    const baseURL = 'http://hmajax.itheima.net'
     export default {
         url:baseURL
     }
    
    // 3. 导入 在其他文件写
    import obj from './utils.js'
    console.log(obj)
</script>
```

ECMASript标准命名导出导入

```html
<script>
	// 1. 在运行模块文件夹内创建package.json文件写入
    {"type":"module"}
    
    // 2. 导出 在文件utils.js中写
    export const baseURL = 'http://hmajax.itheima.net'
    
    // 3. 导入 在其他文件写
    import {baseURL} from './utils.js'
    console.log(obj)
</script>
```

包相关内容：黑马AJAXp93

# 5.Webpack

## （1）打包过程

```html
<script>
	// 1. 准备项目和源代码
    
    // 2. 准备打包环境 
    // 2.1 下载软件包 npm i webpack webpack-cli --save-dev
    // 2.2 在package.json文件中配置 "scripts" : { "build" : "webpack" }
    
    // 3. 运行自定义命令打包 npm run build
    
</script>
```

## （2）修改打包入口和出口

```html
<script>
	// 1. 项目根目录新建webpack.config.js配置文件
    // 2. 导出配置对象，配置入口出口文件路径（官方文档查询）
</script>
```

## （3）自动生成html文件

```html
<script>
	// 1. 下载html-webpack-plugin软件包
    // 2. 配置webpack.config.js（官方文档查询）
    const HtmlWebpackPlugin = require('html-webpack-plugin')
    plugins: [
        new HtmlWebpackPlugin({
            template: 文件地址（绝对路径）// 模版文件
            filename: 文件地址（绝对路径）// 输出文件
        })
    ]
</script>
```

## （4）打包CSS代码（在js文件中）

```html
<script>
    // 1. 在打包入口文件中引入css代码
    import 'css文件地址'
	// 2. 下载css-loader和style-loader软件包
    // 3. 配置webpack.config.js（官方文档查询）
    module: {
        rules:[
            {
                test:/\.css$/i,
                use:["style-loader","css-loader"]
            }
        ]
    }
</script>
```

## (5)提取打包的css代码

```html
<script>
	// 1. 在打包入口文件中引入css代码
    import 'css文件地址'
	// 2. 下载css-loader和mini-css-extract-plugin软件包
    // 3. 配置webpack.config.js（官方文档查询）
    const MimiCssExtractPlugin = require('mini-css-extract-plugin')
    // 插件
    plugins:[
        new MimiCssExtractPlugin({
            filename:'打包输出路径' // 相对路径
        })
    ]
    // 加载器（识别更多模块文件内容）
    module: {
        rules:[
            {
                test:/\.css$/i,
                use:[MimiCssExtractPlugin.loader,"css-loader"]
            }
        ]
    }
</script>
```

## （6）压缩css文件

```html
<script>
	// 1. 下载css-minimizer-webpack-plugin软件包
    // 2. 配置webpack.config.js（官方文档查询）
    const CssMinimizerPlugin = require('css-minimizer-webpack-plugin')
    // 优化打包过程
    optimization:{
        // 最小化
        minimizer:[
            `...`,
            new CssMinimizerPlugin()
        ]
    }
</script>
```

## （7）打包图片

```html
<script>
	// 1. 在打包入口文件引入图片资源
    import imgObj from '图片路径'
    // 2. 配置webpack.config.js（官方文档查询）
    // 加载器
    module: {
        rules:[
            {
                test:/\.(png|jpg|joeg|gif)$/i,
                type:'asset',
                generator:{
                    filename:'assets/[hash][ext][query]'
                }
            }
        ]
    }
</script>
```

## （8）搭建开发环境

```html
<script>
	// 1.下载webpack-dev-server软件包
    // 2. 配置package.json文件
    "scripts" : { "dev" : "webpack serve --open" }
    // 3. 配置webpack.config.js（官方文档查询）
    // 打包模式
    mode:'development'
    // 4. 使用npm run dev启动开发服务器
</script>
```

后续看黑马 webpack P29+