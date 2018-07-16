# 微信小程序 WePy Utils

微信小程序 WePy 工具集整合并封装了常用的小程序 API 和 HTTP Request 持续更新中...

## 安装

```bash
npm install wepy-utils
```

## 按需引入

```javascript
import { UTILS, HTTP, TIPS } from 'wepy-utils'
```

## Utils

##### `UTILS.now()`

> 获取当前时间戳

```javascript
let now = UTILS.now()
console.log(now)
```

##### `UTILS.random()`

> 返回任意区间随机数

```javascript
let random = UTILS.random(1, 5)
console.log(random)
```

#### `UTILS.param()`

> 将对象解析成 url 字符串

```javascript
let obj = {} // 需解析的对象
let strResult = UTILS.param(obj, true)  // 第二个参数 true | false 表示是否使用unDecodeURI编码，默认 false
```

#### `UTILS.unparam()`

> 将 url 字符串解析成对象

```javascript
let url = 'url' // 需解析的对象
let objResult = UTILS.unparam(url, true)  // 第二个参数 true | false 表示是否使用unDecodeURI解码，默认 false
```

## HTTP Request

> `GET` `POST` `PATCH` `PUT` `DELETE`

##### `HTTP.get()`


> 第1种使用方法是URL不带参数。第2种使用方法是在请求URL后带参数，如：`?id=1&name=ming`

- `HTTP.get(url).then((data) => {}).catch((error) => {})`
- `HTTP.get({url: url, params: [JSON Object] }, mask: [Boolean]).then((data) => {}).catch((error) => {})`

```javascript
let url = 'urlpath'
HTTP.get({
  url: url,
  params: {id: 1, name: 'ming'},
  mask: true
}).then((data) => {
  console.log(data)
}).catch((error) => {
  console.log(error)
})
```

##### `HTTP.post()`

> 可自定义 headers，如需 `Authorization` 等，默认：`'Content-Type': 'application/json'`

```javascript
HTTP.post({
  url: url, params: {id: 1, name: 'ming' },
  mask: true,
  headers: {'X-Requested-With': 'XMLHttpRequest'}
}).then((data) => {
  console.log(data)
}).catch((error) => {
  console.log(error)
})
```

##### `HTTP.patch()` `HTTP.put()` `HTTP.delete()` 请求方式与 `HTTP.post()` 写法类似

```javascript
// HTTP PATCH
HTTP.patch({url: url, params: [JSON Object], headers: [JSON Object], mask: [Boolean] }).then((data) => {}).catch((error) => {})
// HTTP PUT
HTTP.put({url: url, params: [JSON Object], headers: [JSON Object], mask: [Boolean] }).then((data) => {}).catch((error) => {})
// HTTP DELETE
HTTP.delete({url: url, params: [JSON Object], headers: [JSON Object], mask: [Boolean] }).then((data) => {}).catch((error) => {})
```

> `mask` 是否显示透明蒙层，防止触摸穿透，默认：`false`

## Tips

##### `TIPS.toast()`

> 显示消息提示框（可自定义 wx.showToast 的所有参数，除 success、fail、complete）

```javascript
TIPS.toast({title: '提示标题'})

// 设置 duration > 0 后，隐藏后可支持回调（duration 默认 1500）
TIPS.toast({
  title: '提示标题'
}).then(() => {
  console.log('隐藏后回调')
})
```

##### `TIPS.confirm()`

> 显示模态弹窗（payload 为 Promise.resolve 是可选项），除 showCancel 改为 cancel 外，其它可选参数与 wx.showModal 相同

```javascript
TIPS.confirm({
  title: '提示标题',
  content: '提示内容',
  payload: [1,2,3]
}).then((arr) => {
  console.log('点击了确定', arr[2]) // 3
}).catch(() => {
  console.log('点击了取消')
})
```

##### `TIPS.go()`

> 保留当前页面，跳转到应用内的某个页面

```javascript
TIPS.go('test?id=1')
```

##### `TIPS.setTitle()`

> 动态设置当前页面的标题

```javascript
TIPS.setTitle('Hello WePy')
```

##### `TIPS.loading()`

> 显示 loading 提示框，可自定义提示内容，默认显示透明蒙层，防止触摸穿透

```javascript
TIPS.loading('加载标题')
```

##### `TIPS.loaded()`

> 隐藏 loading 提示框

```javascript
TIPS.loaded()
```

##### `TIPS.downloadSaveFile()`

> 下载单个文件

```javascript
let url = 'url'
TIPS.downloadSaveFile({
  url: url,
  success: (res) => {
    console.log(res)
  },
  fail: (err) => {
    console.log(err)
  }
})
```

##### `TIPS.downloadSaveFiles()`

> 下载多个文件

```javascript
let urls = ['url1','url2','url3']
TIPS.downloadSaveFiles({
  urls: urls,
  progress: true,
  success: (res) => {
    // 下载进度（如果设置 progress: false 数据将在全部下载完成后返回）
    console.log(`下载进度:${res.step}%`)
    // 全部加载完成
    if (res.step === 100) {
      console.log(res)
      res.forEach((value, key) => {
        console.log(`Key:${key} = Value:${value.savedFilePath}`)
      })
    }
  },
  fail: (err) => {
    console.log(err)
  }
})
```

#### `TIPS.share()`

> 转发分享

```javascript
let url = 'url'
TIPS.share('小程序名称', url)
```

#### `TIPS.navigateTo()`

> 保留当前页面，跳转到应用内的某个页面

```javascript
TIPS.navigateTo(url, params)
```

#### `TIPS.redirectTo()`

> 关闭当前页面，跳转到应用内的某个页面

```javascript
TIPS.redirectTo(url, params)
```

#### `TIPS.switchTab()`

> 跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面

```javascript
TIPS.switchTab(url, params)
```

#### `TIPS.reLaunch()`

> 关闭所有页面，打开到应用内的某个页面

```javascript
TIPS.reLaunch(url, params)
```

#### `TIPS.getLocation()`

> 获取地理位置

```javascript
TIPS.getLocation({
    type                   : "wgs84", // [String], 默认 'wgs84', 返回 GPS 坐标
    success                : function (res) {}, // [Function], 获取地理位置成功后回调
    fail                   : function () {}, // [Function], 获取地里位置失败后回调
    complete               : function (res) {}, // [Function], 接口调用结束后回调
    cancel                 : null, // [Function], 当点击授权提示弹窗"取消"按钮后回调，可不写，默认使用 fail 参数的回调
    modalAuthorizationTitle: "提示", // [String], 授权提示弹窗标题
    modalAuthorizationTxt  : "允许获取地理位置信息", // [String], 授权提示弹窗文字内容
})
```

#### `TIPS.getLocateInfo()`

> 根据经纬度获取地理位置信息，默认坐标：北京市人民政府

```javascript
let latitude = 39.90403, longitude = 116.407526
TIPS.getLocateInfo(latitude, longitude)
    .then(function(res){}, function(err){})
```
