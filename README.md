# vue项目填坑(基于vue-cli@2.9.2,webpack@3.6.0)
不定时更新
## vue-cli、webpack相关

### vue-cli构建的项目在打包后良好，开发环境低版本的浏览器白屏

这是因为"webpack-dev-server"版本更新后引入新模块的缘故，在
webpack.base.conf.js中，配置babel-loadel
```js
{
  test: /\.js$/,
  loader: 'babel-loader',
  include: [resolve('src'), resolve('test'), resolve('/node_modules\/webpack-dev-server/')]
}
```
---
### 让其他设备访问到开发环境的项目

首先知道自己本地ip地址，然后在config目录下index.js文件，配置
```js
host:'192.168.0.54',
port: 9494
```
---

### 通过本地代理解决跨域

仅限开发模式，可以配置一个本地的node代理
编辑config/index.js文件中的dev.proxyTable选项
```js
proxyTable: {
  // 代理所有以/api开始的请求到jsonplaceholder
  '/api': {
    target: 'http://192.168.0.98', //服务器ip
    changeOrigin: true,
    pathRewrite: {
      '^/api': ''
    }
  }
}
```
---

## aixos相关

### 常见配置
```js
//让ajax携带cookie
axios.defaults.withCredentials = true;
//设置请求baseURL,仅开发环境用于代理
axios.defaults.baseURL = '/api';
//设置请求超时时间
axios.defaults.timeout = 5000;
//设置请求头
axios.defaults.headers.post['Content-Type'] = 'application/x-www-form-urlencoded; charset=UTF-8';
```
---

### get和post两种请求要注意的地方

get请求官方的两种写法,参数形式的，记得params要带上，post那里不用带
```js
// Make a request for a user with a given ID
axios.get('/user?ID=12345')
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });

// Optionally the request above could also be done as
axios.get('/user', {
    params: {
      ID: 12345
    }
  })
  .then(function (response) {
    console.log(response);
  })
  .catch(function (error) {
    console.log(error);
  });
```

post请求的参数数据格式默认不是form-data,需要转码,官方的说明是引入qs来编码
```js
import qs from 'qs';
//注意，这里的params又不用加params这个键了
axios.post('/foo', qs.stringify({ 'bar': 123 }));
```

---
