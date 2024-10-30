# 1.axios语法

```html
<script>
    const sendData = {
      	id: 1,
    	name: "张三"
    }
    
    // 1. 普通函数写法
	axios({
    	method: 'get',
    	url: '/api/users'
    })
    .then(function(response) {
    	console.log(response.data);
    })
    .catch(function(error) {
    	console.error(error);
  	})
    
    // 2. 剪头函数写法 带参数
    axios({
    	method: 'post',
      	url: '/api/users'
        params: {
        	params1: 'value1',
        	params2: 'value2'
    	}
    })
    .then((response) => {
        console.log(response.data);
    })
    .catch((error) => {
        console.error(error);
    })
    
    // 3. 剪头函数写法 传数据
    axios({
    	method: 'post',
      	url: '/api/users'
        data: sendData
    })
    .then((response) => {
        console.log(response.data);
    })
    .catch((error) => {
        console.error(error);
    })
    
    // 4. 简洁写法 带参数
    axios.post('/api/users', {
        params: {
        	params1: 'value1',
        	params2: 'value2'
    	}
    })
    .then((response) => {
    	console.log(response.data);
    })
    .catch((error) => {
        console.error(error);
    })
    
    // 5. 简洁写法 传数据
    axios.post('/api/users', sendData)
    .then((response) => {
    	console.log(response.data);
    })
    .catch((error) => {
        console.error(error);
    })
</script>
```

# 2.form-serialize插件

一键获取form表单所有数据

```html
<script src="./lib/form-serialize.js"></script>
<script>
	document.querySelector('.btn').addEventListener('click',() => {
        const form = document.querySelector('.example-form')
        const data = serialize(form,{hash:true, empty:true})
        // hash:true设置获取数据结构为JS对象，false为查询字符串
        // empty:true设置获取空值，false不获取
    })
</script>
```

# 3.BootStrap弹框

```html
<html>
    // 1. 引入bootstrap.css和bootstrap.js
    // 2. 在bootstrap官方文档找到弹框结构(Modal)
    // 3. 通过属性控制弹框显示隐藏
    // 3.1 控制显示按钮绑定属性(data-bs-toggle="modal" data-bs-target= "css选择器")
    // 3.2 关闭按钮绑定属性(data-bs-dismiss="modal")
    
    // 控制显示按钮
    <button type="button" class="btn btn-primary" data-bs-toggle="modal" data-bs-target= ".mymodal">显示弹框</button>
    
    // 弹窗结构
    <div class="modal mymodal" tabindex="-1">
        <div class="modal-dialog">
        	<div class="modal-content">
          		<div class="modal-header">
                    <h5 class="modal-title">标题</h5>
                    <button type="button" class="btn-close" data-bs-dismiss="modal" aria-label="Close"></button>
          		</div>
                <div class="modal-body">
                    <p>内容</p>
                </div>
                <div class="modal-footer">
                    <button type="button" class="btn btn-secondary" data-bs-dismiss="modal">关闭</button>
                    <button type="button" class="btn btn-primary">保存</button>
                </div>
        	</div>
      	</div>
	</div>
</html>

<script>
    // 4. 通过JS控制弹框显示隐藏
    // 4.1 创建弹框对象
    const modalDom = doucument.querySelector('css选择器')
    const modal = new bootstrap.Modal(modalDom)
    // 4.2 显示弹框
    modal.show()
    // 4.3 隐藏弹框
    modal.hide()
</script>
```

# 4.上传图片

```html
<script>
	// 1. 绑定改变事件
    document.querySelector('.upload').addEventListener('change', e => {
        // 2. 使用FormData携带图片文件
        const fd = new FormData()
        fd.append('img',e.target.files[0])
        // 3. 提交到服务器
        axios({
            url: 'http://hmajax.itheima.net/api/uploadimg',
            method: 'POST',
            data: fd
        }).then(result => {
            // 4. 取出图片展示
            const imgUrl = result.data.data.url
            documet.querySelector('.my-img').src = imgUrl
        })
    })
</script>
```

# 5.AJAX原理

## XMLHttpRequest对象的基础使用：

```html
<script>
	// 1. 创建XHR对象
    const xhr = new XMLHttpRequest()
    
    // 2. 配置请求方法和路径
    xhr.open('GET','http://hmajax.itheima.net/api/province')
    
    // 3. 监听事件，接收响应结果
    xhr.addEventListener('loadend',() => {
        const data = JOSN.parse(xhr.response)
        console.log(data.list.join('<br>'))
    })
    
    // 4. 发起请求
    xhr.send()
</script>
```

## XMLHttpRequest对象携带查询参数使用：

```html
<script>
    const xhr = new XMLHttpRequest()
    
    // 拼接查询参数 ? 参数1=xx & 参数2=xx
    xhr.open('GET','http://hmajax.itheima.net/api/city?pname=辽宁省')
    
    xhr.addEventListener('loadend',() => {
        const data = JOSN.parse(xhr.response)
        console.log(data.list.join('<br>'))
    })
    
    xhr.send()
</script>
```

## 参数转为查询参数字符串:

```html
<script>
	// 1. 创建URLSearchParams对象
    const paramsObj = new URLSearchParams({
        参数名1: 值1,
        参数名2: 值2
    })
    // 2. 生成指定格式
    const queryString = paramsObj.toString()
</script>
```

## XMLHttpRequest对象携带请求参数使用：

```html
<script>
    const xhr = new XMLHttpRequest()
    
    xhr.open('POST','http://majax.itheima.net/api/register')
    
    xhr.addEventListener('loadend',() => {
        console.log(xhr.response)
    })
    
    // 设置请求头，写明内容类型
    xhr.setRequestHeader('Content-Type','application/json')
    
    // 准备数据
    const userObj = {
        username: 'itheima007',
        password: '654321'
    }
    // 转为JSON字符串
    const userStr = JSON.stringify(userObj)
    
    // 设置请求体，发送请求
    xhr.send(userStr)
</script>
```

# 6.Promise

## 使用：

```html
<script>
	// 1. 创建Promise对象
    const p = new Promise((resolve,reject) => {
        const xhr = new XMLHttpRequest()
        xhr.open('GET','http://jax.itheima.net/api/provice')
        // 2.执行异步任务，传递结果
        // 成功调用resolve(值)触发then()执行
        // 失败调用reject(值)触发catch()执行
        xhr.addEventListener('loadend',() => {
        	if(xhr.status >= 200 && xhr.status < 300) {
                resolve(JOSN.parse(xhr.response))
            } else {
                reject(new Error(xhr.response))
            }
    	})
        xhr.send()
    })
    
    // 3. 接收结果
    p.then(result => {
        console.log(result)
    }).catch(error => {
        console.dir(error) // dir打印详细信息
    })
</script>
```

## 三种状态：

待定(pending)：初始状态

已兑现(fulfilled)：操作成功

已拒绝(reject)：操作失败

一个promise对象必然处于三种状态之一，一旦被兑现/拒绝，状态无法再次更改

## 链式调用：

使用then函数返回新Promise对象的特性，解决回调函数嵌套问题

```html
<script>
	axios({url:'http://hmajax.itheima.net/api/province'})
    .then(result => {
        pname = result.data.list[0]
        ducument.querySelector('.province').innerHTML = pname
        // 通过return返回一个新的promise对象
        return axios({url:'http://hm.net/api/city',params:{pname}})
    }).then(result => {
        const cname = result.data.list[0]
        document.querySelector('.city').innerHTML = cname
    })
</script>
```

## promise.all静态方法：

可以合并多个promise对象，等待所有同时成功完成可调用then，若有至少一个失败，则调用catch

```html
<script>
    // 1. 创建单个Promise对象
	const bjP = axios({url:'weather',params:{city:'1'}}) 
    const shP = axios({url:'weather',params:{city:'2'}}) 
    const gzP = axios({url:'weather',params:{city:'3'}}) 
    const szP = axios({url:'weather',params:{city:'4'}}) 
    
    // 2. 使用Promise.all合并
    const p = Promise.all([bjp,shp,gzp,szp])
    p.then(result => {
        console.log(result) // 返回的是按合并顺序排列的数组对象
        const str = result.map(item => {
            return `<li>${item.data.data.weather}</li>`
        }).join('')
        doucument.querySelector('ul').innerHTML = str
    }).catch(error => {
        console.dir(error)
    })
</script>
```

# 7.封装axios函数

## 简易封装：

```html
<script>
    // 1. 定义myAxios函数，接收配置对象，返回Promise对象
	function myAxios(config) {
        return new Promise((resolve,reject) => {
            // 2. 发起XHR请求，默认请求方法为GET
            const xhr = new XMLHttpRequest()
            xhr.open(config.method || 'GET',config.url)
            // 3. 调用成功/失败的处理程序
            xhr.addEventListener('loadend',() => {
                if(xhr.status >= 200 && xhr < 300) {
                    resolve(JOSN.parse(xhr.response))
                } else {
                    reject(new Error(xhr.response))
                }
            })
            // 4. 发送请求
            xhr.send()
        })
    }
    
    // 5. 使用myAxios函数
    myAxios({
        url:'http://ajax.itheima.net/api/provice'
    }).then(result => {
        console.log(result)
    }).catch(error => {
        console.log(error)
    })
</script>
```

## 支持查询参数：

```html
<script>
	// 定义myAxios函数，接收配置对象，返回Promise对象
	function myAxios(config) {
        return new Promise((resolve,reject) => {
            // 发起XHR请求，默认请求方法为GET
            const xhr = new XMLHttpRequest()
            
            // 1. 判断有参数选项
            if(config.params){
                // 2. 使用URLSearchParams转换参数格式携带到url上
                const paramsObj = new URLSearchParams(config.params)
                const queryString = paramsObj.toString()
                config.url += `?${queryString}`
            }
            
            xhr.open(config.method || 'GET',config.url)
            // 调用成功/失败的处理程序
            xhr.addEventListener('loadend',() => {
                if(xhr.status >= 200 && xhr < 300) {
                    resolve(JOSN.parse(xhr.response))
                } else {
                    reject(new Error(xhr.response))
                }
            })
            // 发送请求
            xhr.send()
        })
    }
    
    // 使用myAxios函数
    myAxios({
        url:'http://ajax.itheima.net/api/area',
        params: {
            pname: '辽宁省',
            cname: '大连市'
        }
    }).then(result => {
        console.log(result)
    }).catch(error => {
        console.log(error)
    })
</script>
```

## 支持请求参数：

```html
<script>
	// 定义myAxios函数，接收配置对象，返回Promise对象
	function myAxios(config) {
        return new Promise((resolve,reject) => {
            // 发起XHR请求，默认请求方法为GET
            const xhr = new XMLHttpRequest()
            xhr.open(config.method || 'GET',config.url)
            // 调用成功/失败的处理程序
            xhr.addEventListener('loadend',() => {
                if(xhr.status >= 200 && xhr < 300) {
                    resolve(JOSN.parse(xhr.response))
                } else {
                    reject(new Error(xhr.response))
                }
            })
            
            // 1. 判断有参数选项
            if(config.data) {
                // 2. 转为JSON字符串
            	const jsonStr = JSON.stringify(config.data)
                // 3. 设置请求头，写明内容类型
                xhr.setRequestHeader('Content-Type','application/json')
                // 4. 发送请求
            	xhr.send(jsonStr)
            } else {
                // 5. 如果没有请求参数则正常发送
                xhr.send()
            }
        })
    }
    
    const btn = document.querySelector('.btn')
    btn.addEventListener('click',() => {
        // 使用myAxios函数
        myAxios({
            url:'http://hmajax.itheima.net/api/register',
            method: 'POST'
            data:{
				username: 'itheima',
            	password: '123456'
            }
        }).then(result => {
            console.log(result)
        }).catch(error => {
            console.dir(error)
        })
    })
</script>
```

# 8.异步解决方案

```html
<script>
	// 1. 使用async关键字修饰函数为异步函数
    // 2. 在函数内使用关键字swait获取Promise对象成功状态的结果
    // 3. await会阻止代码的执行，原地等待结果，获得结果后继续执行
    async function getData() {
        const p = await axios({url:'hm.net/api/province'})
        const pname = p.data.list[0]
        const c = await axios({url:'hm.net/api/city'}, params:{pname})
        const cname = c.data.list[0]
        
        ducument.querySelector('.province').innerHTML = pname
        document.querySelector('.city').innerHTML = cname
    }
</script>
```

错误捕获：

```html
<script>
    // 1. try包裹可能产生错误的代码，若某处产生错误，后续代码不会执行
	try {
        async function getData() {
            const p = await axios({url:'hm.net/api/province'})
            const pname = p.data.list[0]
            const c = await axios({url:'hm.net/api/city'}, params:{pname})
            const cname = c.data.list[0]

            ducument.querySelector('.province').innerHTML = pname
            document.querySelector('.city').innerHTML = cname
        }
    } catch (error) {
        // 2. 调用catch块接收错误信息
        console.dir(error)
    }
</script>
```

# 9.基地址配置

```html
<script>
	// 在文件夹utils中的request.js文件中配置
    axios.defaults.baseURL = '接口前部分公共地址'
</script>
```

# 10.axios请求拦截器

在发起请求前触发的配置函数，对请求参数进行额外配置

```html
<script>
	// 在文件夹utils中的request.js文件中配置
    axios.interceptors.request.use(function (config) {
        // 在发送请求前做什么
        // 统一携带token令牌字符串在请求头上
        const token = localStorage.getItem('token')
        token && config.headers.Authorization = `Bearer ${token}`
        return config
    },function (error) {
        // 对请求错误做什么
        return Promise.reject(error)
    })
</script>
```

# 11.axios响应拦截器

在响应回到then/catch之前，触发的拦截函数，对响应结果进行统一处理

```html
<script>
	// 在文件夹utils中的request.js文件中配置
    axios.interceptors.response.use(function (response) {
        // 对响应数据做什么
        const result = response.data
        return result
    },function (error) {
        // 对响应错误做什么，例如统一对401身份验证失败情况做处理
        if(error?.response?.status === 401){
            alert('身份验证失败，请重新登录')
            localStorage.clear()
            location.href = '../login/index.html'
        }
        return Promise.reject(error)
    })
</script>
```

# 12.富文本编辑器

```html
<script>
	// 查找官方文档（wangeditor.com）引入wangeditor富文本编辑器插件样式
</script>
```