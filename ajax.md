# Ajax 学习笔记

> Ajax的作用: Ajax是一种在无需重新加载整个网页,  
> 就能更新部分网页的技术;

## 1. Ajax 的应用场景
 1. 页面上拉加载更多数据;
 2. 列表数据无刷新分页;
 3. 表单项离开焦点数据验证;
 4. 搜索框提示文字下拉列表;

## 2. Ajax 的运行环境
 > Ajax 技术**需要运行在网站环境中才能生效**;
 > > ``` javascript
 > >   // 引入express框架
 > >   const express = require('express')
 > >   // 路径处理模块
 > >   const path = require('path')
 > >   // 创建web服务器
 > >   const app = express()
 > >   // 静态资源访问服务功能
 > >   app.use(express.static(path.join(__dirname,'public')))
 > >   // 监听端口
 > >   app.listen(3000)
 > >   // 控制台提示输出
 > >   console.log('create server success')
 > > ```

## 3. Ajax 的实现步骤
 > 1. 创建Ajax对象
 > > ``` javascript
 > > var xhr = XMLHttpRequest()
 > > ```

 > 2. 告诉Ajax请求地址以及请求方式
 > > ``` javascript
 > > xhr.open('get','http://www.example.com')
 > > ```

 > 3. 发送请求
 > > ``` javascript
 > > xhr.send()
 > > ```

 > 4. 获取服务器端给与客户端的响应数据 
 > >  ``` javascript
 > >  xhr.onload = function() {
 > >  console.log(xhr.responseText) 
 > >    }
 > >  ```

## 4. 服务器端响应的数据格式
  > 1. 在真实的项目中,服务器端**大多数情况下会以**  
  > **JSON对象作为响应数据的格式**,当客户端拿到响应数据时,  
  > 要将JSON数据和HTML字符串进行拼接,然后将拼接的结果展示在页面中;
  > 2. 在http请求与响应的过程中,无论是请求参数还是响应内容,  
  > 如果是对象类型 ,最终都会被转换为对象字符串进行传输; 
  > 3. 使用 **JSON.parse()**  转换为对象格式再进行字符串拼接;

## DOM 内容,不是Ajax
  > **ele.insertAdjacentHTML** 与 **ele.insertAdjacentText**
  >
  > > 添加HTML内容与文本内容以前用的是innerHTML与innerText方法，

  >> 最近发现还有insertAdjacentHTML和 insertAdjacentText方法，
  >> 这两个方法更灵活，可以在指定的地方插入html内容和文本内容。

  >> 方法链接地址[链接地址](https://blog.csdn.net/javagtcpp/article/details/14001321?utm_medium=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param&depth_1-utm_source=distribute.pc_relevant_t0.none-task-blog-BlogCommendFromMachineLearnPai2-1.channel_param)

  > **Object.assign()**
  > > Object.assign方法的第一个参数是目标对象，后面的参数都是源对象。
  > > 注意，如果目标对象与源对象有同名属性，或多个源对象有同名属性，则后面的属性会覆盖前面的属性。如果没有同名属性则会拷贝(**浅拷贝**)
  > > ```javascript
  > >   const target = { a: 1, b: 1 };
  > >   const source1 = { b: 2, c: 2 };
  > >   const source2 = { c: 3 };
  > >   Object.assign(target, source1, source2);
  > >   target // {a:1, b:2, c:3}
  > > ```

## 5. 请求参数传递
> 传统网站表单提交
>> 1. get提交方式: 通过地址栏传递参数;
>> 2. post提交方式: 通过请求主体(存放在Form Date中) 传递;

 ##### Ajax 请求参数传递

>> 1. **get 请求方式**
>>> xhr.open('get','http://www.127.0.0.1:3000?name=keduoli&age=22')  
请求参数是拼接在 xhr Ajax对象中,然后再传递给服务端;
简单来说就是将 input控件中的value值拼接在地址后面如上;

>> 2. **post 请求方式**
>> > - 在发送请求之前,需要手动的修改请求的消息头
>> > ``` javascript
>> > xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded')
>> > ```

>>> - post的请求数据要放在请求主体中  
>>> ``` javascript 
>>>  let params = 'username=' + namevalue + '&age=' + agevalue
>>> xhr.send(params)
>>> ```

###### 总结 get与post
 > **get** : 直接将要传递的数据 放在 **`xhr.open(get,url)`** 拼接到url后面;
 > **post** : 
 >> 1. xhr.setRequestHeader('Content-Type','application/x-www-form-urlencoded') 固定配置
 >> 2. 将请求主体(要传递的数据)放在 **`xhr.send()`** 放在此方法中

###### 请求报文
 > 在HTTP 请求和响应的过程中传递的数据块就叫报文,  
 包括要传送的数据和一些附加信息,这些数据和信息要遵守规定好的格式;

## 6. 请求参数的格式
 1. application/x-www-form-urlencoded  
    这种格式 是传统网站以及Ajax 中的**get** ,**post**请求方式使用的  
 数据格式为: name=zhangsan&sex=男
 2. application/json
    这种格式是 传递**json数据对象格式**使用的 
    数据格式为: {name:'zhangsan',age: '22',sex:'男'}
  > 注意: 使用此格式需要配置以下几点
  > > + 必须是以**post**方式提交
  > >    ``` javascript 
  > >    xhr.open(post,'http://www.example.com')
  > >    ```
  > > + 在请求头中指定Content-Type属性的值是**application/json**,  
  > > 告诉服务器当前请求参数是json
  > >   ``` javascript 
  > >   xhr.setRequestHeader('Content-Type','application/json')
  > >   ```
  > > + 将json对象转换为字符串  
  > >    ``` javascript 
  > >    xhr.send(JSON.stringify(要转换的数据对象))
  > >    ```
  > > + 服务器端配置 **bady-parser** 时注意  
  > >   ``` javascript 
  > >   app.use(bodyParser.urlencoded()) // 将前面的配置更改成后面的  
  > >   app.use(bodyParser.json())
  > >   ```
##### 注意: get请求是不能提交json对象数据格式的,传统网站的表单提交也是不支持json对象格式的

  > > ``` html
  > > <body>
  > > <button>发送ajax请求</button>
  > > <!-- 
  > > 注意: 如果是 post请求,需要向服务器发送json数据格式,需要设置三个地方
  > > 1. 'Content-Type'设置为 'application/json'
  > > 2. 使用JSON.stringify(传递的数据) 进行转换成json数据格式
  > > 假如传递的只是一个字符zhangsan  那么需要在JSON.stringify({自定义一个属性名: zhangsan})
  > > 3. 服务器端的 app.use(bodyParser.urlencoded({extended: false})) 
  > > 改变成 => app.use(bodyParser.json()) 
  > > -->
  > > <script type="text/javascript">
  > > 	var btn = document.querySelector('button')
  > > 	btn.onclick = function() {
  > > 		var xhr = new XMLHttpRequest()
  > > 		xhr.open('post','/post')
  > > 		xhr.setRequestHeader('Content-Type','application/json')
  > > 		xhr.send(JSON.stringify({alais: '小仙女',name: '楚月婵'}))
  > > 		xhr.onload = function() {
  > > 			if(xhr.status == 200) {
  > > 				console.log(xhr.responseText)
  > > 			}
  > > 		}
  > > 	}
  > > </script>
  > > </body>
  > > ```

## 7. 获取服务器端的响应
###### Ajax 状态码
  > 在创建ajax对象,配置ajax对象,发送请求,以及接收完服务器端响应数据,  
  > 这个过程中的每一个步骤都会对应一个数值,这个数值就是ajax状态码;
  >> - 0 : 请求未初始化(还没有调用open());
  >> - 1 : 请求已经建立,但是还没有发送(还没有调用send());
  >> - 2 : 请求已经发送;
  >> - 3 : 请求正在处理中,通常响应中已经有部分数据可以用了;
  >> - 4 : 响应已经完成,可以获取并使用服务器的响应了;
  > **xhr.readyState**  获取ajax状态码
##### onreadyStateChange 事件
 > 当Ajax状态码发生变化时将自动触发事件; 
 > > ``` html
 > >   <script>
 > >         let xhr = new XMLHttpRequest()
 > >             // 状态码0 已经创建了ajax对象 但是还没有对ajax对象进行配置
 > >         console.log(xhr.readyState)
 > >         xhr.open('get', 'http://localhost:3000/readystate')
 > >             // 1 已经对ajax对象进行配置,但是还没有发送请求
 > >         console.log(xhr.readyState)
 > >             // 当ajax状态码发生改变的时候触发
 > >         xhr.onreadystatechange = function() {
 > >                 // 2 请求已经发送
 > >                 // 3 已经接收到服务器端的部分数据了
 > >                 // 4  服务器端的响应数据已经接收完成
 > >                 console.log(xhr.readyState)
 > >                     // 对ajax 状态码进行判断
 > >                     // 如果状态码的值为4就代表数据已经接收完成了
 > >                 if (xhr.readyState == 4 && xhr.status == 200) {
 > >                     console.log(xhr.responseText)
 > >                 }
 > >             }
 > >             // 发送请求
 > >         xhr.send()
 > >     </script>
 > > ```
##### 两种获取服务器端响应方式的区别
> |区别描述|onload事件|onreadystatechang事件|
> |:----:|:----:|:----:|
> |是否兼容IE低版本|不兼容|兼容|
> |是否需要判断Ajax状态码|不需要|需要|
> |被调用次数|一次|多次|
> |**注意是否将事件放置在xhr.send()前面**|放置在xhr.send()**后面**|放置在xhr.send() **前面**|

## 8. Ajax错误处理
 1. 网络畅通,服务器端能接收到请求,服务器端返回的结果不是预期结果.
  - **可以判断服务器端返回的状态码,分别进行处理,xhr.status获取http状态码**
 2. 网络畅通,服务器端没有接收到请求,返回404状态码
  - **检查网络请求地址是否错误**
 3. 网络畅通,服务器端能接收到请求,服务器端返回500状态码
  - **服务器端错误,找后端程序员进行沟通**
 4. 网络中断,请求无法发送到服务器端
  - **会触发xhr对象下面的onerror事件,在onerror事件处理函数中对错误进行处理**

## 低版本 IE 浏览器的缓存问题
> **问题**: 
  >> 在低版本的IE浏览器中,Ajax请求有严重的缓存问题,  
  即在请求地址不发生改变的情况下,只有第一次请求会真正发送到  
  服务器端,后续的请求都会从浏览器的缓存中获取结果,  
  即使服务器端的数据更新了,客户端依然拿到的是缓存中的**旧数据**

> **解决方案**: 
> > 在请求地址的后面添加请求参数,  
> > 保证每一次请求中的请求参数的值不相同
> >
> > ``` javascript 
> > xhr.open('get','http://www.example.com?t='+ Math.random());
> > ```

## Ajax函数封装知识点
  > 1. `xhr.getResponseHeader('Content-Type')` // 获取响应头中的数据
  > 2. js中的 `includes()` 方法 两种用法
  > > - string.includes('指定字符串')
  > > > **includes()** 方法用于判断字符串是否包含指定的子字符串。
  > > > 如果找到匹配的字符串则返回 true，否则返回 false。
  > > > ***注意***： includes() 方法区分大小写。
  > > - Array.includes() 数组也有这个方法
  > > > includes() 方法用来判断一个数组是否包含一个指定的值,  
  > > > 如果是返回 true，否则false。
  > > > ``` javascript 
  > > >   let site = ['runoob', 'google', 'taobao'];
  > > >   site.includes('runoob'); // true 
  > > >   site.includes('baidu'); // false
  > > > ```

  ###### Ajax函数封装代码
   > ``` javascript
   > <body>
   > 	<script type="text/javascript">
   > 		function ajax (options) {
   > 			// 存储的是默认值
   > 			var defaults = {
   > 				type: 'get',
   > 				url: '',
   > 				data: {},
   > 				contentType: {
   > 					'Content-Type': 'application/x-www-form-urlencoded'
   > 				},
   > 				success: function () {},
   > 				error: function () {}
   > 			};
   > 
   > 			// 使用options对象中的属性覆盖defaults对象中的属性
   > 			Object.assign(defaults, options);
   > 
   > 			// 创建ajax对象
   > 			var xhr = new XMLHttpRequest();
   > 			// 拼接请求参数的变量
   > 			var params = '';
   > 			// 循环用户传递进来的对象格式参数
   > 			for (var attr in defaults.data) {
   > 				// 将参数转换为字符串格式
   > 				params += attr + '=' + defaults.data[attr] + '&';
   > 			}
   > 			// 将参数最后面的&截取掉 
   > 			// 将截取的结果重新赋值给params变量
   > 			params = params.substr(0, params.length - 1);
   > 
   > 			// 判断请求方式
   > 			if (defaults.type == 'get') {
   > 				defaults.url = defaults.url + '?' + params;
   > 			}
   > 
   > 			/*
   > 				{
   > 					name: 'zhangsan',
   > 					age: 20
   > 				}
   > 
   > 				name=zhangsan&age=20
   > 
   > 			 */
   > 
   > 			// 配置ajax对象
   > 			xhr.open(defaults.type, defaults.url);
   > 			// 如果请求方式为post
   > 			if (defaults.type == 'post') {
   > 				// 用户希望的向服务器端传递的请求参数的类型
   > 				var contentType = defaults.header['Content-Type']
   > 				// 设置请求参数格式的类型
   > 				xhr.setRequestHeader('Content-Type', contentType);
   > 				// 判断用户希望的请求参数格式的类型
   > 				// 如果类型为json
   > 				if (contentType == 'application/json') {
   > 					// 向服务器端传递json数据格式的参数
   > 					xhr.send(JSON.stringify(defaults.data))
   > 				}else {
   > 					// 向服务器端传递普通类型的请求参数
   > 					xhr.send(params);
   > 				}
   > 
   > 			}else {
   > 				// 发送请求
   > 				xhr.send();
   > 			}
   > 			// 监听xhr对象下面的onload事件
   > 			// 当xhr对象接收完响应数据后触发
   > 			xhr.onload = function () {
   > 
   > 				// xhr.getResponseHeader()
   > 				// 获取响应头中的数据
   > 				var contentType = xhr.getResponseHeader('Content-Type');
   > 				// 服务器端返回的数据
   > 				var responseText = xhr.responseText;
   > 
   > 				// 如果响应类型中包含applicaition/json
   > 				if (contentType.includes('application/json')) {
   > 					// 将json字符串转换为json对象
   > 					responseText = JSON.parse(responseText)
   > 				}
   > 
   > 				// 当http状态码等于200的时候
   > 				if (xhr.status == 200) {
   > 					// 请求成功 调用处理成功情况的函数
   > 					defaults.success(responseText, xhr);
   > 				}else {
   > 					// 请求失败 调用处理失败情况的函数
   > 					defaults.error(responseText, xhr);
   > 				}
   > 			}
   >             xhr.onerror = function() {
   >                 defaults.error('网络中断,请稍后访问')
   >             }
   > 		}
   > 
   > 		ajax({
   > 			type: 'post',
   > 			// 请求地址
   > 			url: 'http://localhost:3000/responseData',
   >              data: {name: '楚月婵',alias: '小仙女'},
   >              contentType: 'application/json',
   > 			success: function (data) {
   > 				console.log('这里是success函数');
   > 				console.log(data)
   > 			}
   > 		})
   > 
   > 		/*
   > 			请求参数要考虑的问题
   > 
   > 				1.请求参数位置的问题
   > 
   > 					将请求参数传递到ajax函数内部, 在函数内部根据请求方式的不同将请求参数放置在不同的位置
   > 
   > 					get 放在请求地址的后面
   > 
   > 					post 放在send方法中
   > 
   > 				2.请求参数格式的问题
   > 
   > 					application/x-www-form-urlencoded
   > 
   > 						参数名称=参数值&参数名称=参数值
   > 
   > 						name=zhangsan&age=20
   > 
   > 					application/json
   > 
   > 						{name: 'zhangsan', age: 20}
   > 
   > 					1.传递对象数据类型对于函数的调用者更加友好
   > 					2.在函数内部对象数据类型转换为字符串数据类型更加方便
   > 
   > 		*/
   > 	</script>
   > </body>
   > ```

------------------------------------------

## day2

### 1. 模板引擎 art-template
 > 官网访问不了,到 **GitHub** 里面搜索 art-template模板下载
 > 使用方法步骤:
 >
 > > ``` html
 > >   <!DOCTYPE html>
 > >   <html lang="en">
 > >   <head>
 > >     <meta charset="UTF-8">
 > >     <title>Document</title>
 > >     <!-- 1. 将模板引擎的库文件引入到当前页面 -->
 > >     <script src="/js/template-web.js"></script>
 > >   </head>
 > >   <body>
 > >     <div class="container"></div>
 > >     <!-- 2. 准备art-template模板 -->
 > >     <script type="text/html" id=tpl>
 > >       <h1>{{name}} 年芳 {{age}}</h1>
 > >     </script>
 > >     <script type="text/javascript">
 > >       // 3. 告诉模板引擎将哪个数据和哪个模板进行拼接
 > >       // 参数一: 模板id
 > >       // 参数二: 对象类型,模板变量
 > >       // template方法返回的就是拼接好的html字符串
 > >       var html = template('tpl',{name: '师妃暄',age: 22})
 > >       console.log(html)
 > >       document.querySelector('.container').insertAdjacentHTML('beforeend',html)
 > >     </script>
 > >   </body>
 > >   </html>
 > > ```


### 2. 输入框事件 oninput
 什么是输入框事件?  
 在文本框中输入任意字符都会触发该事件;

 ##### oninput 事件在用户输入时触发。
 <input> 或 <textarea> 元素的值发生改变时触发。

提示： 该事件类似于 onchange 事件。不同之处在于 oninput 事件在元素值发生变化是立即触发， onchange 在元素失去焦点时触发。另外一点不同是 onchange 事件也可以作用于 <keygen> 和 <select> 元素。

##### onchange 事件
 定义和用法
 onchange 事件会在域的内容改变时发生。
 onchange 事件也可用于单选框与复选框改变后触发的事件。

### 3. FormData对象
 ##### FormDate 对象的作用

**注意:**在使用 FormData 对象时不能设置 请求头信息 setRequestHeader()   
因为 内部会自动添加请求头信息:  multipart/form-data

 1. 模拟HTML表单,相当于将HTML表单映射成表单对象,  
 自动将表单对象中的数据拼接成请求参数的格式;
 2. 异步上传**二进制文件** 
 由于是上传的二进制文件所以服务器端不能再使用 body-parser来接受请求主体了  
 得使用 **formidable**来接受客户端提交的二进制文件;

 ##### FormData 对象的使用
  1. 准备HMTL表单
  ``` html
  <form id="form">
    <input type="text" name="username">
    <input type="password" name="pwd">
    <button>提交</button>
  </form>
  ```
  2. 将HTML表单转化为 formData 对象
  ``` javascript
  var form = document.getElementById('form')
  var formData = new FormData(form)
  ```
  3. 提交表单对象
      `xhr.send(formDate)`
###### 代码演示
> ``` html
>   <!DOCTYPE html>
>   <html lang="en">
>   <head>
>       <meta charset="UTF-8">
>       <title>Document</title>
>   </head>
>   <body>
>       <!-- 创建普通的html表单 -->
>       <form id="form">
>           <input type="text" name="username">
>           <input type="password" name="password">
>           <input type="button" id="btn" value="提交">
>           <!-- <button>提交</button> 不能使用这种方式-->
>       </form>
>       <script type="text/javascript">
>           // 获取按钮
>           var btn = document.getElementById('btn');
>           // 获取表单
>           var form = document.getElementById('form');
>           // 为按钮添加点击事件
>           btn.onclick = function() {
>               // 将普通的html表单转换为表单对象
>               var formData = new FormData(form);
>               // 创建ajax对象
>               var xhr = new XMLHttpRequest();
>               // 对ajax对象进行配置
>               xhr.open('post', 'http://localhost:3000/formData');
>               // 发送ajax请求
>               xhr.send(formData);
>               // 监听xhr对象下面的onload事件
>               xhr.onload = function() {
>                   // 对象http状态码进行判断
>                   if (xhr.status == 200) {
>                       console.log(xhr.responseText);
>                   }
>               }
>           }
>       </script>
>   </body>
>   </html>
> ```

> 后端代码
> ``` javascript
>  const formidable = require('formidable')
>  app.post('/formData', (req, res) => {
>     const form = new formidable.IncomingForm()
>     form.parse(req, (err, fields, files) => {
>         res.send(fields)
>     })
>  })
> ```

##### FormData 对象的实例方法
 1. 获取表单对象中的属性的值
 ``` javascript
 var formData = new FormData(document.querySelector('form'))
 formData.get('属性名')
 ```
 2. 设置表单对象中的属性的值
 ``` javascript
 formData.set('属性名','属性值')
 // 如果设置的表单属性存在,设置的属性值会覆盖原有的值;
 // 如果没有属性名,则会创建其属性名与属性值;
 ```
 3. 删除表单对象中属性的值
 ``` javascript
 formData.delete('属性名')
 ```
 4. 向表单对象中追加属性值
  ``` javascript
  formData.append('属性名','属性值')
  // 注意: set方法与append方法的区别是,
  // 在属性名已存在的情况下,  
  // set会覆盖已有属性名的值,
  // append会保留两个值,会在原有的值后面再进行追加一个值,  
  // 不过打印输出的时候会取最后一个值进行显示;
  ```
##### FormData 二进制文件上传 以及 进图条显示及百分比 还有 图片上传预览
 > ``` html
 >   <!DOCTYPE html>
 >   <html lang="en">
 >   <head>
 >     <meta charset="UTF-8">
 >     <title>Document</title>
 >     <link rel="stylesheet" href="https://cdn.jsdelivr.net/npm/bootstrap@4.5.0/dist/css/bootstrap.min.css" integrity="sha384-9aIt2nRpC12Uk9gS9baDl411NQApFmC26EwAOH8WgZl5MYYxFfc+NcPb1dKGj7Sk" crossorigin="anonymous">
 >     <style type="text/css">
 >         .container {
 >             padding-top: 60px;
 >         }
 >         .padding {
 >             padding: 5px 0 20px 0;
 >         }
 >     </style>
 >   </head>
 >   <body>
 >     <div class="container">
 >         <div class="form-group">
 >             <label>请选择文件</label>
 >             <input type="file" id="file">
 >             <div class="padding" id="box">
 >                 <!--<img src="" class="img-rounded img-responsive">-->
 >             </div>
 >             <div class="progress">
 >                 <div class="progress-bar" style="width: 0%;" id="bar">0%</div>
 >             </div>
 >         </div>
 >     </div>
 >     <script type="text/javascript">
 >         // 获取文件选择控件
 >         var file = document.getElementById('file');
 >         // 获取进度条元素
 >         var bar = document.getElementById('bar');
 >         // 获取图片容器
 >         var box = document.getElementById('box');
 >         // 为文件选择控件添加onchanges事件
 >         // 在用户选择文件时触发
 >         // ==================================实现文件上传
 >         file.onchange = function() {
 >             // 创建空的formData表单对象
 >             var formData = new FormData();
 >             // 将用户选择的文件追加到formData表单对象中
 >             formData.append('attrName', this.files[0]);
 >             // 创建ajax对象
 >             var xhr = new XMLHttpRequest();
 >             // 对ajax对象进行配置
 >             xhr.open('post', 'http://localhost:3000/upload');
 >             // =======================================实现文件上传进度条以及百分比
 >             // 在文件上传的过程中持续触发
 >             xhr.upload.onprogress = function(ev) {
 >                     // ev.loaded 文件已经上传了多少
 >                     // ev.total  上传文件的总大小
 >                     var result = (ev.loaded / ev.total) * 100 + '%';
 >                     // 设置进度条的宽度
 >                     bar.style.width = result;
 >                     // 将百分比显示在进度条中
 >                     bar.innerHTML = result;
 >                 }
 >                 // 发送ajax请求
 >             xhr.send(formData);
 >             // 监听服务器端响应给客户端的数据
 >             xhr.onload = function() {
 >                 // 如果服务器端返回的http状态码为200
 >                 // 说明请求是成功的
 >                 if (xhr.status == 200) {
 >                     //====================================实现文件上传图片预览
 >                     // 将服务器端返回的数据显示在控制台中
 >                     var result = JSON.parse(xhr.responseText);
 >                     // 动态创建img标签
 >                     var img = document.createElement('img');
 >                     // 给图片标签设置src属性
 >                     img.src = result.path;
 >                     // 当图片加载完成以后
 >                     img.onload = function() {
 >                         // 将图片显示在页面中
 >                         box.appendChild(img);
 >                     }
 >                 }
 >             }
 >         }
 >     </script>
 >   </body>
 >   </html>
 > ```

#### 同源政策

 ***什么是同源***
 > 如果两个页面拥有<u>**相同的协议**.**域名**和**端口**</u>,那么这两个页面就是属于同源,  
 > 其中只要有一个不相同,就不是同源;
 > 例如:
 >> http://www.example.com/dir/page.html
 >> http://www.example.com/public/css/index.css **同源**
 >> https://www.example.com/index.html **不同源 协议不同**

 ***Ajax请求限制***
 > Ajax只能向自己的服务器发送请求,比如现在有一个A网站,有一个B网站,  
 > A网站中的HTML文件只能向A网站服务器中发送Ajax请求,
 > B网站中的HTML文件只能向B网站中发送Ajax请求,  
 > 但是A网站是不能向B网站发送Ajax请求的,   
 > 同理,B网站也不能向A网站发送Ajax请求;

 ***同源政策的目的***
 > 同源政策是为了保证用户信息的安全,防止恶意网站窃取数据,  
 最初的同源政策是指A网站在客户端设置的Cookie,B网站是不能访问的;
 > 随着互联网的发展,同源政策也越来越严格,在不同源的情况下,  
 其中有一项规定就是无法向非同源地址发送Ajax请求,如果请求,浏览器就会报错;

----------------------------------------------------------------

## day3
 #### 1. 使用JSONP 解决同源限制问题
 jsonp是json with padding 的缩写,它不属于Ajax请求,但他可以模拟Ajax请求;
 > 客户端
 > ``` html
 >   <script>
 >     function fn(data) {
 >       console.log(data)
 >     }
 >   </script>
 >   <!-- 将非同源服务器的请求地址写在script标签的src属性中 -->
 >   <script src='http://localhost:3001/better?callback=fn'></script>
 > ```

 > 服务器端
 > ``` javascript
 >   const data = 'fn({name:'keduoli',age:'16'})'
 >   res.send(data)
 > ```

 #### 2. JSONP 代码优化
 > 注意: jsonp **不支持** post 请求方式
 > ###### 1. 客户端要将函数名称传递到服务器端
 > > ``` html
 > >   <!-- 客户端代码 -->
 > >    <script>
 > >         function fn2(data) {
 > >             console.log(data)
 > >         }
 > >    </script>
 > >    <script src="http://localhost:3001/better?callback=fn2"></script> 
 > > ```

  >> ``` javascript
  >>   /* 服务器代码 */ 
  >>   app.get('/better', (req, res) => {
  >>     // 接受客户端传递过来的函数名称
  >>     const fnName = req.query.callback
  >>     // 将函数名称对应的函数调用代码返回给客户端
  >>     const result = fnName + '({name:"楚月禅",age:"25"})'
  >>     res.send(result)
  >>   })
  >> ```

 > ###### 2. 将script请求变成动态请求 
 > > ``` html
 > >   <!-- 客户端代码 -->
 > >   <body>
 > >       <button id="btn">点我发送请求</button>
 > >       <script>
 > >           function fn2(data) {
 > >               console.log(data)
 > >           }
 > >       </script>
 > >       <!-- <script src="http://localhost:3001/better?callback=fn"></script> -->
 > >       <script>
 > >           // 获取按钮
 > >           var btn = document.getElementById('btn')
 > >           // 为按钮添加点击事件
 > >           btn.onclick = function() {
 > >               // 创建script标签
 > >               var script = document.createElement('script')
 > >               // 设置src属性
 > >               script.src = "http://localhost:3001/better?callback=fn2"
 > >               // 追加到body中
 > >               document.body.appendChild(script)
 > >               // 为script标签添加onload事件
 > >               script.onload = function() {
 > >                   // 将script标签加载过后没用了就删除掉
 > >                   document.body.removeChild(script)
 > >               }
 > >           }
 > >       </script>
 > >   </body>
 > > ```

 > ##### 3. 封装JSONP函数,方便请求发送
 > > ``` html
 > >   <script>
 > >         function fn2(data) {
 > >             console.log(data)
 > >         }
 > >         // 封装jsonp 函数
 > >         function jsonp(options) {
 > >             // 动态创建script标签
 > >             var script = document.createElement('script')
 > >                 // 为script标签添加src属性
 > >             script.src = options.url
 > >                 // 追加到页面中
 > >             document.body.appendChild(script)
 > >                 // 为script标签添加onload事件
 > >             script.onload = function() {
 > >                 document.body.removeChild(script)
 > >             }
 > >         }
 > >         // 调用jsonp函数
 > >         jsonp({
 > >             // 请求地址
 > >             url: 'http://localhost:3001/better?callback=fn2'
 > >         })
 > >     </script>
 > > ```

 > ### 4. **重点** JSONP函数终极封装代码
 > > ``` html
 > >  <body>
 > >     <button id="btn1">发送请求</button>
 > >     <button id="btn2">发送请求</button>
 > >     <script>
 > >         var btn1 = document.getElementById('btn1')
 > >         btn1.onclick = function() {
 > >             // 调用jsonp函数
 > >             jsonp({
 > >                 // 请求地址
 > >                 url: 'http://localhost:3001/better',
 > >                 // 向服务器传递数据
 > >                 data: {
 > >                   name: '师妃萱',
 > >                   age: 24
 > >                 },
 > >                 success: function(data) {
 > >                     console.log(123)
 > >                     console.log(data)
 > >                 }
 > >             })
 > >         }
 > >         var btn2 = document.getElementById('btn2')
 > >         btn2.onclick = function() {
 > >                 // 调用jsonp函数
 > >                 jsonp({
 > >                     // 请求地址
 > >                     url: 'http://localhost:3001/better',
 > >                     success: function(data) {
 > >                         console.log(123456);
 > >                         console.log(data)
 > >                     }
 > >                 })
 > >             }
 > >             /*
 > >                 function fn2(data) {
 > >                     console.log(data)
 > >                 }
 > >             */
 > >             // 封装jsonp 函数
 > >         function jsonp(options) {
 > >             // 设置随机函数名
 > >             var fnName = 'myJson' + Math.random().toString().replace('.', '')
 > >             // 拼接字符串变量,改变传递数据的格式(将对像变成字符串)
 > >             var params = ''
 > >             for(var attr in options.data) {
 > >               params += '&' + attr + '=' + options.data[attr]
 > >             }
 > >                 // 动态创建script标签
 > >             var script = document.createElement('script')
 > >                 // fn2函数已经不是一个全局函数了
 > >                 // 我们要想办法将他变成全局函数
 > >             window[fnName] = options.success
 > >                 // 为script标签添加src属性
 > >             script.src = options.url + '?callback=' + fnName + params
 > >                 // 追加到页面中
 > >             document.body.appendChild(script)
 > >                 // 为script标签添加onload事件
 > >             script.onload = function() {
 > >                 document.body.removeChild(script)
 > >             }
 > >         }
 > >     </script>
 > > </body>
 > > ```

 > ###### 5. 服务器端的代码优化***res.jsonp()***
 > ``` javascript
 >   app.get('/better', (req, res) => {
 >     /*
 >         // 接受客户端传递过来的函数名称
 >         const fnName = req.query.callback
 >             // 将函数名称对应的函数调用代码返回给客户端
 >             // 模拟数据库中的数据
 >             const data = JSON.stringify({name:"楚月禅",age:"25"})
 >             const result = fnName + '('+ data +')'
 >             // 下面的是直接是传递的字符串所以不用转换
 >         // const result = fnName + '({name:"楚月禅",age:"25"})'
 >         res.send(result)
 >     */
 >     // 使用 res.jsonp() 方法可以简化以上代码
 >     res.jsonp({ name: "楚月禅", age: "25" })
 >  })
 > ```

 ##### 6. 使用JSONP获取腾讯天气网
 > ``` html
 >   <body>
 >     <table class="table table-striped">
 >         <thead>
 >             <tr>
 >                 <th scope="col">时间</th>
 >                 <th scope="col">温度</th>
 >                 <th scope="col">天气</th>
 >                 <th scope="col">风向</th>
 >                 <th scope="col">风力</th>
 >             </tr>
 >         </thead>
 >         <tbody id="tby">
 >         </tbody>
 >     </table>
 >     <script type="text/html" id="tpl">
 >         {{each info}}
 >         <tr>
 >             <td>{{dateFormat($value.update_time)}}</td>
 >             <td>{{$value.degree}}</td>
 >             <td>{{$value.weather}}</td>
 >             <td>{{$value.wind_direction}}</td>
 >             <td>{{$value.wind_power}}</td>
 >         </tr>
 >         {{/each}}
 >     </script>
 >     <script>
 >     // 设置template模板变量(向模板中开放外部变量)
 >     template.defaults.imports.dateFormat = dateFormat
 >     var tby = document.getElementById('tby')
 >         // 向服务器获取天气信息
 >     jsonp({
 >             url: 'https://wis.qq.com/weather/common',
 >             data: {
 >                 source: 'pc',
 >                 weather_type: 'observe|forecast_1h|forecast_24h|index|alarm|limit|tips|rise',
 >                 province: '四川省',
 >                 city: '绵阳市'
 >             },
 >             success: function(data) {
 >                 var html = template('tpl', {
 >                     info: data.data.forecast_1h
 >                 })
 >                 tby.innerHTML = html
 >             }
 >         })
 >         // 设置时间格式处理函数
 >     function dateFormat(time) {
 >         var year = time.substr(0, 4)
 >         var month = time.substr(4, 2)
 >         var day = time.substr(6, 2)
 >         var hour = time.substr(8, 2)
 >             // 进行字符串拼接
 >         return year + '年' + month + '月' + day + '日' + hour + '时'
 >     }
 >     // 封装jsonp 函数
 >     function jsonp(options) {
 >         // 设置随机函数名
 >         var fnName = 'myJson' + Math.random().toString().replace('.', '')
 >             // 拼接字符串变量,改变传递数据的格式(将对像变成字符串)
 >         var params = ''
 >         for (var attr in options.data) {
 >             params += '&' + attr + '=' + options.data[attr]
 >         }
 >         // 动态创建script标签
 >         var script = document.createElement('script')
 >             // fn2函数已经不是一个全局函数了
 >             // 我们要想办法将他变成全局函数
 >         window[fnName] = options.success
 >             // 为script标签添加src属性
 >         script.src = options.url + '?callback=' + fnName + params
 >             // 追加到页面中
 >         document.body.appendChild(script)
 >             // 为script标签添加onload事件
 >         script.onload = function() {
 >             document.body.removeChild(script)
 >         }
 >     }
 >    </script> 
 >   </body>
 > ```

 ##### 7. CORS跨域资源共享
 第二种解决跨域资源访问的方法
 **跨域资源共享**也就是 **非同源资源共享**
 **CORS**: 全称为Cross-origin resource sharing,即跨域资源共享,  
 它允许浏览器向跨域服务器发送Ajax请求,克服了Ajax只能同源使用的限制
 ``` html
     <script>
        /* 当前是3000 端口的服务器使用Ajax访问 3001端口的服务器 */
        var xhr = new XMLHttpRequest()
        xhr.open('get', 'http://localhost:3001/cors')
        xhr.send()
        xhr.onload = function() {
            if (xhr.status == 200) {
                console.log(xhr.responseText);
            }
        }
    </script>
 ```
 ``` javascript
  app.use((req,res,next) => {
      // 1. 允许哪些客户端访问我 *号 代表所有
      // res.header('Access-Contorl-Allow-Origin','http://localhost:3000')
      res.header('Access-Control-Allow-Origin','*')
      // 2. 允许客户端使用哪些请求方式访问
      res.header('Access-Control-Allow-Methods','get,post')
      // 释放控制权
      next()
  })
  app.get('/cors', (req, res) => {
      res.send('ok')
  })
 ```

 ##### 8. 访问非同源数据,服务器端解决方案
  同源政策是浏览器给与Ajax技术的限制,服务器端是不存在同源政策限制的  
  使用A服务器去访问B服务器就绕过了服务器的同源限制;
  > ``` html
  >    <script>
  >         // 客户端使用Ajax正常访问自己的服务器
  >         var xhr = new XMLHttpRequest()
  >         xhr.open('get', 'http://localhost:3000/server')
  >         xhr.send()
  >         xhr.onload = function() {
  >             if (xhr.status == 200) {
  >                 console.log(xhr.responseText);
  >             }
  >         }
  >     </script>
  > ```

  > ``` javascript
  >    // 使用第三方模块 向其他服务器端请求数据的模块
  >    const request = require('request')
  >    app.get('/server', (req, res) => {
  >      request('http://localhost:3001/cors', (err, response, body) => {
  >         if (err) {
  >             console.log(err);
  >         } else {
  >             res.send(body)
  >         }
  >     })
  >    })
  >    app.listen(3000)
  > ```

 ##### 9. withCredentials 属性
  在使用Ajax技术发送跨域请求时,默认情况下不会再请求中携带cookie信息  
  withCredentials: 指定在涉及到跨域请求时,是否携带cookie信息,默认值是false;  
  Access-Control-Allow-Credentials: true  允许客户端发送请求时携带cookie

  > ``` javascript
  >    // 客户端
  >    login.onclick = function() {
  >             var formData = new FormData(logform)
  >             var xhr = new XMLHttpRequest()
  >             // 当发送跨域请求时,携带cookie信息
  >             xhr.withCredentials = true
  >             xhr.open('post', 'http://localhost:3001/login')
  >             xhr.send(formData)
  >             xhr.onload = function() {
  >                 if (xhr.status == 200) {
  >                     console.log(xhr.responseText);
  >                 }
  >             }
  >         }
  > ```

  > ``` javascript
  > // 服务器端,跨域的服务器
  > // 拦截所有请求
  > app.use((req, res, next) => {
  >  // 1.允许哪些客户端访问我
  >  // * 代表允许所有的客户端访问我
  >  // 注意：如果跨域请求中涉及到cookie信息传递，值不可以为*号 比如是具体的域名信息
  >  res.header('Access-Control-Allow-Origin', 'http://localhost:3000')
  >  // 2.允许客户端使用哪些请求方法访问我
  >  res.header('Access-Control-Allow-Methods', 'get,post')
  >  // 允许客户端发送跨域请求时携带cookie信息
  >  res.header('Access-Control-Allow-Credentials', true);
  >  /* 注意: 如果想要用ContentType:application/json发送跨域请求，
  >     服务器端还必须设置一个名为Access-Control-Allow-Headers的Header，
  >     将它的值设置为 Content-Type，
  >     表明服务器能够接收到前端发送的请求中的ContentType属性并使用它的值
  >   */
  >  res.header('Access-Control-Allow-Headers', 'Content-Type')
  >  next();
  > });
  > ```

  > 实例
  > ``` html
  >     <body>
  >     <form id="form">
  >         <input type="text" name="username">
  >         <input type="password" name="password">
  >         <input type="button" id="btn" value="提交">
  >         <input type="button" id="btn2" value="验证">
  >     </form>
  >   </body>
  >   <script>
  >     var btn = document.getElementById('btn')
  >     var form = document.getElementById('form')
  >     var btn2 = document.getElementById('btn2')
  >     btn.onclick = function() {
  >         var formData = new FormData(form)
  >         var xhr = new XMLHttpRequest()
  >             // 携带cookie 信息
  >         xhr.withCredentials = true
  >         xhr.open('post', 'http://localhost:3001/login')
  >         xhr.send(formData)
  >         xhr.onload = function() {
  >             if (xhr.status == 200) {
  >                 console.log(xhr.responseText)
  >             }
  >         }
  >     }
  >     btn2.onclick = function() {
  >         var xhr = new XMLHttpRequest()
  >             // 携带cookie 信息
  >         xhr.withCredentials = true
  >         xhr.open('get', 'http://localhost:3001/login')
  >         xhr.send()
  >         xhr.onload = function() {
  >             if (xhr.status == 200) {
  >                 console.log(xhr.responseText)
  >             }
  >         }
  >     }
  >   </script>
  > ```

> ``` javascript
>   app.use((req, res, next) => {
>     res.header('Access-Control-Allow-origin', 'http://localhost:3000')
>     res.header('Access-Control-Allow-Method', 'get,post')
>     res.header('Access-Control-Allow-Credentials', true)
>     next()
>   })
> 
>   app.post('/login', (req, res) => {
>     var form = new formidable.IncomingForm()
>     form.uploadDir = path.join(__dirname, 'public')
>     form.keepExtensions = true
>     form.parse(req, (err, fields, files) => {
>         var data = { uasrname: 'keduoli', password: 123 }
>         if (fields.username == 'keduoli' && fields.password == '123') {
>             // 设置cookie 方便判断验证
>             req.session.info = true
>             res.send('登录成功')
>         } else {
>             res.send(fields)
>         }
>     })
>  })
> 
>   app.get('/login', (req, res) => {
>       if (req.session.info) {
>           // 携带了cookie
>           res.send('ok')
>       } else {
>           // 没有携带cookie
>           res.send('bad')
>       }
>   })
> ```


 #### 10. $.ajax()方法概述
 > 作用: 发送Ajax请求
 > > ``` javascript
 > >    $.Ajax({
 > >       // 请求方法
 > >        type: 'get',
 > >        // 请求地址
 > >        url: 'http://www.example.com',
 > >        // 传递给服务器的数据
 > >        // 也可以直接传递字符串 
 > >        // data: 'name=zhangsan&age=20'
 > >        data: { name: 'zhangsan',age: '20' },
 > >        // 传递的数据类型
 > >        contentType: 'application/x-www-form-urlencoded',
 > >        // 发送请求之前做的事情
 > >        beforeSend: function() {return false},
 > >        // 接收服务器向客户端响应的数据
 > >        success: function(response) {
 > >          // response为服务器端返回的数据
 > >          // 方法内部会自动将json字符串转换为json对象
 > >          console.log(response)
 > >        },
 > >        // 请求失败以后函数被调用
 > >        error: function(xhr) {
 > >          console.log(xhr)
 > >        }
 > >     })
 > > ```

  > serialize方法
  > > ``` html
  > >    <body>
  > >       <form id="form">
  > >         <input type="text" name="username">
  > >         <input type="password" name="password">
  > >         <input type="submit" value="提交">
  > >       </form>
  > >       <script src="/js/jquery.min.js"></script>
  > >       <script type="text/javascript">
  > >         $('#form').on('submit', function () {
  > >           // 将表单内容拼接成字符串类型的参数
  > >           // var params = $('#form').serialize();
  > >           // console.log(params)
  > >           serializeObject($(this));
  > >           return false;
  > >         });
  > >         // 将表单中用户输入的内容转换为对象类型
  > >         function serializeObject (obj) {
  > >           // 处理结果对象
  > >           var result = {};
  > >           // [{name: 'username', value: '用户输入的内容'}, {name: 'password', value: '123456'}]
  > >           var params = obj.serializeArray();
  > >           // 循环数组 将数组转换为对象类型
  > >           $.each(params, function (index, value) {
  > >             result[value.name] = value.value;
  > >           })
  > >           // 将处理的结果返回到函数外部
  > >           return result;
  > >         }
  > >       </script>
  > >    </body> 
  > > ```

-----------------------------------------------------

## day4
#### $ajax()方法发送JSONP请求
> 作用: 发送jsonp请求
> > ``` javascript
> >   $.ajax({
> >     url: 'http://www.example.com',
> >     // 指定当前发送jsonp请求
> >     dataType: 'jsonp',
> >     // 修改callback参数名称
> >     jsonp: 'cb',
> >     // 指定函数名(了解即可,没什么用)
> >     jsonpCallback: 'fnName',
> >     success: function(response) {
> >       console.log(response)
> >     }
> >   })
> > ```

#### $get()和$post()方法
> 两个方法格式一样
> $.get()方法用于发送get请求
> $.post()方法用于发送post请求
> > ``` html
> >  <body>
> >     <button id="btn">发送Ajax请求</button>
> >     <script>
> >         $(function() {
> >             $('#btn').on('click', function() {
> >                 $.get('/get', 
> >                       {name: '秦梦瑶',age: 18}, 
> >                       function(response) {console.log(response)}
> >                      )
> >                 // 第二个参数可以是对象格式也可以是字符串格式; 两个方法格式都一样
> >                 $.post( 'http://localhost:3000/post', 
> >                        'name=珂朵莉&age=17',
> >                        function(response) {console.log(response);}
> >                 )
> >             })
> >         })
> >     </script>
> >  </body>
> > ```

#### TOdo 案例
##### 为todo数据库添加账号
> 1. 使用 mongo命令进入mongodb数据库;
> 2. 使用use admin 命令进入到admin数据库中;
> 3. 使用 db.auth('root','root')命令登录数据库;
> 4. 使用 use todo 命令切换到todo数据库;
> 5. 使用 db.createUser({user: 'itcast',pwd: 'itcast',roles:['readWrite']}) 创建todo数据库账号;

#### jQuery中的全局事件
> 只要页面中有Ajax请求发送,对应的全局事件就会被触发;
> 1. **.ajaxStart(callback)** 当请求开始发送时触发
> 2. **.ajaxComplete(callback)** 当请求完成时触发
> ``` javascript
>   // 注意 如果是全局事件 就要使用 document处理全局事件,不能使用其他元素
>   // 当页面中有ajax请求发送时触发
>   $(document).on('ajaxStart,function() {
>     // 这里使用了jquery插件 NProgress插件
>     NProgress.start()// 进度条开始运动
>   })
>   
>   $(document).ajaxStart(function() {
>     // 与上面是等价的
>     NProgress.start()// 进度条开始运动
>   })
> 
>   $(document).on('ajaxComplete,function() {
>     // 这里使用了jquery插件 NProgress插件
>     NProgress.done()  // 进度条结束运动
>   })
> ```

> 3. NProgress.js 插件  进度条插件
> 使用步骤:
> > ``` html
> >   <link rel="stylesheet" href="nprogress.css">
> >   <script src="nprogress.js"></script>
> >    NProgress.start() <!-- 进度条开始运动 -->
> >    NProgress.done() <!--  进度条结束运动 -->
> > ```

#### RESTful 风格的 API
**传统请求地址回顾**
> - GET http://www.example.com/getUsers        // 获取用户列表
> - GET http://www.example.com/getUser?id=1    // 比如获取某一个用户的信息
> - POST http://www.example.com/modifyUser     // 修改用户信息
> - GET http://www.example.com/deleteUser?id=1 // 删除用户信息

**RESTful API 概述**
定义: 一套关于设计请求的规范
**注意**: put和 delete 这两个请求方式只能在 ajax中使用 

> + GET :    获取数据
> + POST:    添加数据
> + PUT :    更新数据
> + DELETE:  删除数据

**RESTful API 的实现**
> - GET:    http://www.example.com/users       // 获取用户列表数据
> - POST:   http://www.example.com/users       // 创建(添加)用户数据
> - GET:    http://www.example.com/users/1     // 获取用户ID为1的用户信息
> - PUT:    http://www.example.com/users/1     // 修改用户ID为1的用户信息
> - DELETE: http://www.example.com/users/1     // 删除用户ID为1的用户信息

> 实例
> > ``` html
> >   <body>
> >     <script>
> >         $.ajax({
> >             type: 'get',
> >             url: '/users/1',
> >             success: function(response) {
> >                 console.log(response)
> >             }
> >         })
> >         $.ajax({
> >             type: 'put',
> >             url: '/users/10',
> >             success: function(response) {
> >                 console.log(response)
> >             }
> >         })
> >         $.ajax({
> >             type: 'delete',
> >             url: '/users/10',
> >             success: function(response) {
> >                 console.log(response)
> >             }
> >         })
> >     </script>
> > </body>
> > ```

>> ``` javascript
>> app.get('/users/:id', (req, res) => {
>>     const id = req.params.id
>>     res.send(`当前我们是在获取id为${id}的用户信息`)
>> })
>> app.put('/users/:id', (req, res) => {
>>     const id = req.params.id
>>     res.send(`当前我们是在修改id为${id}的用户信息`)
>> })
>> app.delete('/users/:id', (req, res) => {
>>     const id = req.params.id
>>     res.send(`当前我们是在删除id为${id}的用户信息`)
>> })
>> ```

#### XML
> 1. 什么是 XML
> > xml 的全称是: extensible markup language 可拓展的标记语言;
> >  作用: 用于 **传输和存储数据**;并非用于展示数据
> 2. 语法规范
> > - XML 可以保存独立的 **.xml**文件;
> > - 也可以以字符串的方式出现
> > + XML的声明需要放在文件的最顶端
> > > <?xml version="1.0" encoding="utf-8" ?> 
> > + XML标记语法
> > > 1. 标记必须成对出现 <name></name> <age></age>
> > > 2. 严格区分大小写,开始与结束标记必须一致
> > > 3. 允许标记嵌套
> > > 4. 允许自定义属性,格式与html一致,但是**注意:** **属性值必须用引号""引起来**
> > > > <student age="10" score="100"></student>
> > >
> > > 5. 每个xml文件必须有一个跟标记(根元素),例如html中的<html></html>
> 3. 使用 ajax 获取服务器端返回的**xml格式的**响应报文
> > ``` javascript
>    btn.onclick = function() {
>             var xhr = new XMLHttpRequest()
>             xhr.open('get','/xml')
>             xhr.send()
>             xhr.onload = function() {
>                 /* xhr.resposeXML 获取服务器端返回的xml数据 */
>                 console.log(xhr.responseXML)
>             }
>         }
> > ```
>
> 4. 获取xml元素,或者对xml元素进行操作跟html一样 使用DOM 进行操作