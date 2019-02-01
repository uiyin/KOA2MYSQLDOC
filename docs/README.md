# YJ_One

## 它是什么

`YJ_One` 是基于 KOA2+MYSQL 写的一个轻量级 OA 系统测试接口合集,让刚入门的 KOA2 开发者学会更快速的搭建后台环境,又或者让前端开发者不用管后台环境部署和搭建。直接使用接口来完成前端的渲染.

`YJ_One` 实质的作用就是为了提供实际项目开发体验,方便前端初学者完成一个小型 OA 系统的搭建,从而尽快的上手工作环境。它很大程度上简化了代码,让使用者更轻松的专注于前端工程的部署和搭建,不用去网上在搜集 POST 或者 get 请求的接口测试接口，带给前端工程师沉浸式编程体验.

## 不是什么

`YJ_One`不是万能的,比如你想获取地图接口，或者大数据接口又或者图形接口等等,
很抱歉它是做不到的,因为本人没有时间去完成了.我在此也期待一些志同道合的朋友一起来完善它.

## 作者概述

本人是一名来自天津的前端工程师。网名`七宗罪`

擅长技术栈:

前端:jquery+Vue+React+小程序

后端: koa2+Express+mysql

作为一名前端工程师大家都经历过苦于找不到接口,无奈只有自己 Mock 数据

但实际项目体验与 mock 数据是截然不同的。

所以为了解决前端初学者从业难题，我由此有了创立`YJ_One`的想法。

2019 年 1 月 7 日 `YJ_One`诞生了。它不完美，但是它是我走向全栈工程师的第一步.

> 如果你在使用过程中遇到什么问题建议可以给我发一个[issue](https://github.com/uiyin/koa2mysql/issues/new)

## 特性

- 基于 Koa2+Mysql 开发
- 简洁的 API 操作
- 注解驱动
- 支持跨域
- 支持跨域带 cookie 请求
- 支持 JWT(令牌环)
- 支持验证码
- 支持 RBAC(权限分级)

# 快速开始

## 引入依赖

`YJ_One` 必须依赖的环境是 Node+mysql,这里我就不在叙述怎么安装这两个了。然后导入 koa2.sql 文件即可。网上教程一大堆。要是实在不会的话，请联系我,我给你发线上接口.

从[这里](https://github.com/uiyin/koa2mysql) 下载后 cd 到下载好的文件根目录然后使用

```javascript
npm i

```

网络不好也可以使用淘宝镜像来代替。使用淘宝镜像的话就敲

```javascript
cnpm i

```

安装开发依赖包,安装好依赖包后，请到 MySQL 文件夹下面的 mysqlconfig.js 下面修改配置文件,然后在文件的根目录敲击命令

```javascript
node app.js
```

这样项目就启动了。

服务器端启动默认端口为 23000,若不想使用 23000 端口,可到 app.js 里面自行修改.

```javascript
app.listen(23000, () => {
  console.log('服务启动了')
})
// 23000换成其他接口就行。但不要使用3306接口，因为数据库占用了。尽量用2W以后的接口
```

## 查看接口项目

项目启动后,查看项目的话,在浏览器中输入

```
http://localhost:23000/test

```

他要是返回的是

```javascript
{
  code:401,
  message:"token过期了"
}
```

证明项目安装部署成功

# 接口文档

## 调用前须知

> 为了实现实际的开发环境,接口里面有 GET 请求也有 Post 请求

> 在注册和登陆的接口必须要携带 `xhrFields: { withCredentials: true }`如果没有携带会导致注册或者登陆不成功

> 状态码 200 表示成功。里面的 message 各有不同。401 表示 token 有问题不是过期了就是验证码不对.402 表示注册的时候,输入有问题

## 用户模块

下面开始是关于用户模块的注册，登陆，和一些关于用户模块的增删改查

### 1.验证码请求

**(i)请求:GET**

**(ii)必填参数:无**

**(iii)接口地址: `/checkpic`**

**(iv)实例化 DEMO**

在 HTML 中要是想显示可以这样写

```html
<img
  src="http://localhost:23000/checkpic"
  onclick="this.src='http://localhost:23000/checkpic?'+Math.random();"
/>
<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

### 2.用户注册

> 用户注册这里，必须是跨域带 cookie 因为第一次请求验证码的时候，服务器端加密存到了 cookie 里面,所以请求的时候必须带上 `xhrFields: { withCredentials: true }`要不然服务器端收不到 cookie

> 后台设置了 cookie 的有效期是 5 分钟,要是超出了请刷新图片

> TOKEN 的有效期是一个月。一个月以后请重新登陆

**(i)请求:POST**

**(ii)必填参数:**

```javascript
{
  // 必填 验证码上面的数字
     checkpic,
  // 必填 用户输入的邮箱地址(后台验证了)
     email,
  // 必填 用户输入用的用户名(用户名格式为6-16位的字母，数字，下划线，减号)(后台验证了)
     name,
  // 必填 密码只能输入6-20个字母、数字、下划线(后台验证了)
     password,
  // 必填 昵称 昵称只能是2-20位的字母,数字,汉字(后台验证了)
     nick,
  // 选填 个人简介只能是1-200的字母,数字,汉字(后台验证了)
     detail_info,
  // 必填 这里就是RBAC权限分级了。
  // 默认就是1 里面包括的模块有资金管理,资金流水,地图分类,地图展示,大数据,报表汇总
  // 最高级是0 资金管理,资金流水,用户信息,查找用户,地图分类,地图展示,大数据,报表汇总
  // 最低级的2 里面有资金管理,资金流水,地图分类,地图展示,大数据,报表汇总
     rule:1
}
```

**(iii)接口地址**

```html
http://localhost:23000/user/signup
<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)注册成功返回的数据**

```Json
{
  success: true,
  message: "成功",
  data: null,
  // token
  token: "Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx",
  //用户级别
  level: 2
}
//因为用了JWT令牌环所以注册成功后它会给你返回一个token
//请每次发送请求的时候带上具体的后面会有具体说明
```

**实例 DEMO**

利用 jquery 写的完整例子。要是 axios 或者 fetch 请自行对照

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/signup',
  xhrFields: {
    withCredentials: true
  },
  data: {
    checkpic,
    email,
    name,
    password,
    nick,
    detail_info,
    rule: 2
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})

// 我这里data用了ES6的写法。要是不会ES6可换成 checkpic: "获取的值"
```

要是 axios 的话

第一种方法

```javascript
axios({
  method: 'post',
  url: 'http://localhost:23000/user/signup',
  data: postData,
  withCredentials: true
  crossDomain:true

}).then(res => {
  console.log(res)
})

```

第二种方法

```javascript
axios.defaults.withCredentials = true
```

### 3.查询所有用户

> 因为用了令牌所以使用的时候必须要加上 `headers` 具体的请看下面

**(i)请求:POST**

**(ii)参数**

```JSON
{
  // 必填 当前的页码要是分页可以值为2，3等等
  page,
  // 选填 不填的话默认就是20 他表示一页多少条数据
  pagesize
}
```

**(iii)请求地址**

```
http://localhost:23000/user/selectuser
<!--冒号后面表示端口号，请对照app.js里面的端口号-->

```

**(iv)返回的接口数据**

```json
{
  "total": 1,
  "code": 200,
  "result": [
    {
      "id": 2,
      "email": "zhangsan@emai.com",
      "password": "xxxxx",
      "name": "zhangsan",
      "nick": "张三",
      "detail_info": "这就是测试",
      "create_time": "1548147180079",
      "avaster": "http://localhost:23000/touxiang.png",
      "level": 2
    }
  ]
}
// total表示总数，方便前端分页
// result 里面返回的就是依据pagesize查询的条数
// id就是序号 email：注册的邮箱 password：注册的密码
// name：用户名 nick:昵称 detail_info: 个人简介
// create_time : 注册时候的时间戳
// avaster: 默认头像
// level:用户级别
```

**(v)完整版 demo**

jquery 例子

```javascript
$.ajax({
    type: "POST",
    url: "http://localhost:23000/user/selectuser",
    headers:{
		Authorization:"Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
		},
    data: {
		 page:1,
		 pagesize:10
        },
    dataType:"json",
    success(res) {
          console.log(res)
        }
    })
	})

// 以后发送请求必须要头部带上token有效期1个月

```

axios 就不列举了。可以把这个放到拦截器里面。这样每次都会自动发送。省时省力

### 4.查询单个用户的所有信息

**(i)请求:POST**

**(ii)参数:**

```json
{
  // 必填 用户的ID
  id
}
```

**(iii)请求地址**

```
http://localhost:23000/user/selectone

<!--冒号后面表示端口号，请对照app.js里面的端口号-->

```

**(iv)返回的接口数据**

```json
{
  "total": 1,
  "code": 200,
  "result": [
    {
      "id": 2,
      "email": "zhangsan@emai.com",
      "password": "xxxxx",
      "name": "zhangsan",
      "nick": "张三",
      "detail_info": "这就是测试",
      "create_time": "1548147180079",
      "avaster": "http://localhost:23000/touxiang.png",
      "level": 2
    }
  ]
}
// total表示总数，方便前端分页
// result 里面返回的就是依据pagesize查询的条数
// id就是序号 email：注册的邮箱 password：注册的密码
// name：用户名 nick:昵称 detail_info: 个人简介
// create_time : 注册时候的时间戳
// avaster: 默认头像
// level:用户级别
```

**(v)完整版 demo**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/selectone',

  headers: {
    Authorization:
      'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },

  data: {
    id: 2
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 5.查询用户名

**(i)请求:POST**

**(ii)必填参数**

```
{
  //用户名
  name
}
```

**(iii)接口地址**

```
http://localhost:23000/user/selectusername

<!--冒号后面表示端口号，请对照app.js里面的端口号-->

```

**(iv)返回的数据**

```json
{
  "total": 1,
  "code": 200,
  "result": [
    {
      "id": 2,
      "email": "zhangsan@emai.com",
      "password": "xxxxx",
      "name": "zhangsan",
      "nick": "张三",
      "detail_info": "这就是测试",
      "create_time": "1548147180079",
      "avaster": "http://localhost:23000/touxiang.png",
      "level": 2
    }
  ]
}
// total表示总数，方便前端分页
// result 里面返回的就是依据pagesize查询的条数
// id就是序号 email：注册的邮箱 password：注册的密码
// name：用户名 nick:昵称 detail_info: 个人简介
// create_time : 注册时候的时间戳
// avaster: 默认头像
// level:用户级别
```

**(v)完整 DEMO**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/selectusername',

  headers: {
    Authorization: 'Bearer xxxxxxxxx'
  },
  data: {
    name: 'xxxxx'
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 6.查询注册时间到指定时间结束

**(i)请求:POST**

**(ii)参数**

```json
{
  // 必填 开始时间时间戳 数值就是 比如 new Date("2019-01-09").valueOf();
	start: starttime,
  // 必填 结束时间时间戳 格式同start
	end:endtime,
  // 必填 当前页
	page:1,
  // 选填如果不填写 默认就是20
	pagesize:10 // 要是不填默认就是20
		},
```

**(iii)接口地址**

```
http://localhost:23000/user/selectdata

<!--冒号后面表示端口号，请对照app.js里面的端口号-->

```

**(iv)返回数据格式**

```json
{
  "total": 1,
  "code": 200,
  "result": [
    {
      "id": 2,
      "email": "zhangsan@emai.com",
      "password": "xxxxx",
      "name": "zhangsan",
      "nick": "张三",
      "detail_info": "这就是测试",
      "create_time": "1548147180079",
      "avaster": "http://localhost:23000/touxiang.png",
      "level": 2
    }
  ]
}
// total表示总数，方便前端分页
// result 里面返回的就是依据pagesize查询的条数
// id就是序号 email：注册的邮箱 password：注册的密码
// name：用户名 nick:昵称 detail_info: 个人简介
// create_time : 注册时候的时间戳
// avaster: 默认头像
// level:用户级别
```

**(v)完整 DEMO 例子**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/selectdata',
  data: {
    start: starttime,
    end: endtime,
    page: 1,
    pagesize: 10 // 要是不填默认就是20
  },
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxx'
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 7.按照 ID 正序和倒序排列

**(i)请求:GET**

**(ii)参数**

```json
{
  //依据ID
  keys:"id",
  //当前页
  page:1,
  //1 是正序 2就是倒序
  order:1,
  // 要是不填默认就是20
  pagesize:10
},
```

**(iii)接口地址**

```
http://localhost:23000/user/selectpaixu

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据格式**

```json
{
  "total": 1,
  "code": 200,
  "result": [
    {
      "id": 2,
      "email": "zhangsan@emai.com",
      "password": "xxxxx",
      "name": "zhangsan",
      "nick": "张三",
      "detail_info": "这就是测试",
      "create_time": "1548147180079",
      "avaster": "http://localhost:23000/touxiang.png",
      "level": 2
    }
  ]
}
// total表示总数，方便前端分页
// result 里面返回的就是依据pagesize查询的条数
// id就是序号 email：注册的邮箱 password：注册的密码
// name：用户名 nick:昵称 detail_info: 个人简介
// create_time : 注册时候的时间戳
// avaster: 默认头像
// level:用户级别
```

**(v)完整 DEMO**

jquery 例子

```javascript
$.ajax({
  type: 'GET',
  url: 'http://localhost:23000/user/selectpaixu',
  data: {
    keys: 'id',
    page: 1,
    order: 1, // 正序
    pagesize: 10 // 要是不填默认就是20
  },
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 8.查询邮箱有没有被注册过

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户输入的邮箱
  "result": value
}
```

**(iii)接口地址**

```
http://localhost:23000/user/checkemail

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据的格式**

```json
{
  "code": 200,
  //total 要是1就表示注册过，0就表示没有注册过
  "total": 0
}
```

**(v)完整 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/checkemail',
  data: {
    result: value
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 9.查询昵称有没有注册过

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户输入的昵称
  "result": value
}
```

**(iii)接口地址**

```
http://localhost:23000/user/checknick

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据的格式**

```json
{
  "code": 200,
  //total 要是1就表示注册过，0就表示没有注册过
  "total": 0
}
```

**(v)完整 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/checknick',
  data: {
    result: value
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 10.用户登陆

> 同理这里必须带上 `withCredentials:true`

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户名
  "name": username,
  //密码
  password,
  //图片验证码
  checkpic
}
```

**(iii)接口地址**

```
http://localhost:23000/user/login

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据的格式**

```json
{
  //状态码
  "code": 200,
  //结果
  "result": [
    {
      //昵称
      "nick": "张三",
      //用户级别
      "level": 0
    }
  ],
  // token
  "token": "Bearer xxxxxxxxxxxxxxxxxxxxxxx"
}
```

**(v)完整 DEMO**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/login',
  xhrFields: {
    withCredentials: true
  },
  data: {
    name: username,
    password,
    checkpic
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 11.依据用户级别获取第一层菜单

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户级别,登陆后就会获取到
		level:level
,

```

**(iii)接口地址**

```
http://localhost:23000/user/userlevel

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据格式**

```json
{
  "code": 200,
  "result": [
    {
      //id就是子菜单的父id,通过用户点击后,把这个值传给后台,后台在给你子菜单
      "id": "1",
      "name": "资金管理"
    },
    {
      "id": "3",
      "name": "地图分类"
    },
    {
      "id": "4",
      "name": "大数据"
    }
  ]
}
```

**(v)完整 DEMO 例子**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/userlevel',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    level: 1
  },
  success(res) {
    console.log(res)
  }
})
```

### 12.依据父元素的 id 返回子元素

**(i)请求:POST**

**(ii)参数**

```json
{
  //就是刚才获取父菜单的时候id
  level
}
```

**(iii)接口地址**

```
http://localhost:23000/user/userson

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回的数据格式**

```json
{
  "code": 200,
  "result": [
    {
      "name": "资金流水"
    }
  ]
}
```

**(v)完整 DEMO 例子**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/userson',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    level: level
  },
  success(res) {
    console.log(res)
  }
})
```

### 13.删除指定的用户

**(i)请求:POST**

**(ii)参数**

```javascript
{
  id: 3
}
```

**(iii)接口地址**

```
http://localhost:23000/user/deleteuser

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回的数据格式**

```
{
  "code": 200,
  "message": "删除成功"
}
```

**(v)完整版 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/deleteuser',
  data: {
    id: value
  },
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

### 14.查询验证码输入是否正确

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户输入的昵称
  "result": value
}
```

**(iii)接口地址**

```
http://localhost:23000/user/checkma

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据的格式**

```json
{
  //code 200 表示正确 402 表示错误
  "code": 200,
  //total 要是1就表示注册过，0就表示没有注册过
  "message":"验证码正确"
}
```

**(v)完整 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/checkma',
  xhrFields: {
                withCredentials: true
            },
  data: {
    result: value
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```
### 15.查询用户名输入是否唯一

**(i)请求:POST**

**(ii)参数**

```json
{
  //用户输入的昵称
  "result": value
}
```

**(iii)接口地址**

```
http://localhost:23000/user/checkusername

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据的格式**

```json
{
  "code": 200,
  //total 要是1就表示注册过，0就表示没有注册过
  "total": 0
}
```

**(v)完整 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/user/checkusername',
  data: {
    result: value
  },
  dataType: 'json',
  success(res) {
    console.log(res)
  }
})
```

## 资金模块

### 1.插入数据

**(i)请求:POST**

**(ii)参数**

```json
{
  //插入时间,时间戳
	create_time:time,
  //类型
  type:"优惠券",
  //收入必须是整型
	income:200,
  //支出必须是整型
	pay:100,
  // 现金
	cash:500,
  // 备注
	info:"我刚才花了100元"
},
```

**(iii)接口地址**

```
http://localhost:23000/money/insertmoney

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据格式**

```json
{
  "code": 200,
  "message": "插入成功"
}
```

**(v)完整 DEMO**

jquery

```javascript
var time = new Date().valueOf()
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/money/insertmoney',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    create_time: time,
    type: '优惠券',
    income: 200,
    pay: 100,
    cash: 500,
    info: '我刚才花了100元'
  },
  success(res) {
    console.log(res)
  }
})
```

### 2.查询所有数据

**(i)请求:POST**

**(ii)参数**

```json
{
  // 选填表示一页多少条数据
	pagesize:10,
  // 必填 表示当前页数
	page:1,
},
```

**(iii)接口地址**

```
http://localhost:23000/money/selectmoney

<!--冒号后面表示端口号，请对照app.js里面的端口号-->

```

**(iv)返回数据格式**

```json
{
  "code": 200,
  "result": [
    {
      "id": 1,
      "create_time": "1548167922369",
      "type": "优惠券",
      "income": 200,
      "pay": 100,
      "cash": 500,
      "info": "我刚才花了100元"
    }
  ],
  "total": 1
}
```

**(v)完整 DEMO**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/money/selectmoney',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    pagesize: 10, // 非必需
    page: 1
  },
  success(res) {
    console.log(res)
  }
})
```

### 3.查询指定 ID 的详细信息数据

**(i)请求方式:POST**

**(ii)参数**

```
{
	id:1
}
```

**(iii)接口地址**

```
http://localhost:23000/money/selectonemoney

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据类型**

```json
{
  "code": 200,
  "result": [
    {
      "id": 1,
      "create_time": "1548167922369",
      "type": "优惠券",
      "income": 200,
      "pay": 100,
      "cash": 500,
      "info": "我刚才花了100元"
    }
  ]
}
```

**(v)完整版 DEMO**

```javascript
  $.ajax({
    type: "POST",
    url: "http://localhost:23000/money/selectonemoney",
		headers:{
		Authorization:"Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx"
		},
		data:{
		  id:1
		},
    success(res) {
          console.log(res)
        }
	   })

})
```

### 4.更新指定 ID 的数据

**(i)请求方式:POST**

**(ii)参数**

```json
{
  "id": 2,
  "type": "提现3333",
  //必须是整型
  "income": 5000,
  //必须是整型
  "pay": 50000,
  //必须是整型
  "cash": 10000,
  "info": "提现成功"
}
```

**(iii)接口地址**

```
http://localhost:23000/money/updatemoney

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据格式**

```
{
  "code": 200,
  "message": "更新成功"
}
```

**(v)完整版 demo**

jquery 例子

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/money/updatemoney',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    id: 1,
    type: '提现3333',
    income: 5000,
    pay: 50000,
    cash: 10000,
    info: '提现成功'
  },
  success(res) {
    console.log(res)
  }
})
```

### 5.删除指定 ID 数据

**(i)请求方式:POST**

**(ii)参数**

```json
{
  "id": 1
}
```

**(iii)接口地址**

```
http://localhost:23000/money/deletemoney

<!--冒号后面表示端口号，请对照app.js里面的端口号-->
```

**(iv)返回数据类型**

```JSON
{
  "code": 200,
  "message": "删除成功"
}
```

**(v)完整版 DEMO**

```javascript
$.ajax({
  type: 'POST',
  url: 'http://localhost:23000/money/deletemoney',
  headers: {
    Authorization: 'Bearer xxxxxxxxxxxxxxxxxxxxxxxxxxxxxxxx'
  },
  data: {
    id: 1
  },
  success(res) {
    console.log(res)
  }
})
```
