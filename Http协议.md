# Http
HTTP（hypertext transport protocol）协议【超文本传输协议】，协议详细规定了浏览器和万维网服务器之间互相通信的规则。
是一个约定、规则
<br/>
<br/>

## 请求报文
重点是格式与参数
```
行      POST/GET  /s?ie=utf-8   HTTP/1.1
头      Host: atguigu.com
        Cookie: name=guigu
        Content-type: application/x-www-form-urlencoded
        User-Agent: chrome 83
空行
体      username=admin&password=admin
```
<br/>
<br/>

## 响应报文
```
行      HTTP/1.1  200(statu)  OK
头      Content-Type: text/html;charset=utf-8
        Content-length: 2048
        Content-encoding: gzip
空行
体
        <html>
            <head>
            </head>
            <body>
                <h1></h1>
            </body>
        </html>
```
* 404
* 403
* 401
* 500
* 200
* ...


# node.js & Express简单使用

## 1、安装node.js 和 Express
到各自官网获取安装方式
## 2、选择位置初始化，新建一个js文件
1. 引入express
   ```javascript
   const express = require('express');
   ```
2. 创建应用对象
    ```javascript
    const app = express();
    ```
3. 创建路由规则
    ```javascript
    app.get('/server', (request, response) => {
        //设置响应头， 设置允许跨域
        response.setHeader('Access-Control-Allow-Origin', '*');
        //对路径设置响应
        response.send('Hello Express');
    });
    ```
4. 监听端口启动
   ```javascript
   app.listen(8000, ()=>{
       console.log("服务已启动，8000端口监听中...");
   })
   ```


## AJAX 调用/请求
```javascript
const btn = document.getElementsByTagName('button')[0];
const result = document.getElementById('result');
//绑定事件
btn.conclick = function(){
    //创建对象
    const xhr = new XMLHttpRequest();
    //初始化
    xhr.open('GET', 'http://127.0.0.1:8000/server');
    //发送
    xhr.send();
    //事件绑定，处理服务端返回的结果
    xhr.onreadystatechange = function(){
        //判断，服务端返回了所有的结果
        if(xhr.readyState === 4){
            //判断状态码
            //2xx表示成功
            if(xhr.status >= 200 & xhr.status < 300){
                //处理结果 行 头 空行 体
                //响应行
                // console.log(xhr.status); //状态码
                // console.log(xhr.statusText); //状态字符串
                // console.log(xhr.getAllResponseHeaders())//处理响应头
                // console.log(xhr.response);//响应体
                result.innerHTML = xhr.response;
            }
            else{

            }
        }
    }
}
```





12