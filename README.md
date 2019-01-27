# AMapJS

[![Build Status](https://travis-ci.org/iDerekLi/amap-js.svg?branch=master)](https://travis-ci.org/iDerekLi/amap-js)
[![npm version](https://img.shields.io/npm/v/amap-js.svg?style=flat-square)](https://www.npmjs.com/package/amap-js)
[![npm downloads](https://img.shields.io/npm/dm/amap-js.svg?style=flat-square)](https://www.npmjs.com/package/amap-js)
[![npm license](https://img.shields.io/npm/l/amap-js.svg?style=flat-square)](https://github.com/iderekli/amap-js)

> AMapJS是AMap高德地图加载模块。帮助您加载高德sdk，这对于在组件化应用、异步编程中非常有帮助。

## 安装
使用npm:
```
npm install amap-js --save
```
使用yarn:
```
yarn add amap-js
```
使用cdn:
```html
<script type="text/javascript" src="https://unpkg.com/amap-js/dist/amap-js.min.js"></script>
```
### 兼容性
AMapJS **不支持** IE8 及以下版本，因为 AMapJS 使用了 IE8 无法模拟的 ECMAScript 5 特性。但它支持所有兼容 ECMAScript 5 的浏览器。


## 示例

### AMap JS API的加载:
```JavaScript
import AMapJS from "amap-js";

// 创建AMapJSAPI Loader
const aMapJSAPILoader = new AMapJS.AMapJSAPILoader();

aMapJSAPILoader.load().then(AMap => {
  // 其他逻辑
});
```
或者
```javascript
// 创建AMapJSAPI Loader
const aMapJSAPILoader = new AMapJS.AMapJSAPILoader({
  key: "您申请的key值",
  v: "1.4.12", // 版本号
  params: {}, // 其他参数
  protocol: "https:", // 脚本加载协议
  crossOrigin: "anonymous", // 脚本crossOrigin设置
  keepScriptTag: false // 加载完成后是否保留脚本标签
});

aMapJSAPILoader.load().then(AMap => {
  // 其他逻辑
});
```


### AMap UI组件库的加载:
```JavaScript
import AMapJS from "amap-js";

// 创建AMapJSAPI Loader
const aMapJSAPILoader = new AMapJS.AMapJSAPILoader({ key: "您申请的key值" });

// 创建AMapUI Loader
const aMapUILoader = new AMapJS.AMapUILoader();

aMapJSAPILoader.load().then(AMap => {
  aMapUILoader.load().then(initAMapUI => {
    const AMapUI = initAMapUI(); // 这里调用initAMapUI初始化并返回AMapUI
    // 其他逻辑
  });
});
```
或者
```javascript
// ...

// 创建AMapUI Loader
const aMapUILoader = new AMapJS.AMapUILoader({
  v: "1.0", // UI组件库版本号
  protocol: "https:", // UI组件库 API脚本加载协议
  crossOrigin: "anonymous",
  AMapUIProtocol: "https:", // UI组件的脚本加载协议
  keepScriptTag: false // 加载完成后是否保留脚本标签
});

aMapJSAPILoader.load().then(AMap => {
  aMapUILoader.load().then(initAMapUI => {
    const AMapUI = initAMapUI(); // 这里调用initAMapUI初始化并返回AMapUI
    // 其他逻辑
  });
});
```
预加载
```javascript
// 创建AMapJSAPI Loader
const aMapJSAPILoader = new AMapJS.AMapJSAPILoader({ key: "您申请的key值" });

// 创建AMapUI Loader
const aMapUILoader = new AMapJS.AMapUILoader();

// 预加载aMapJSAPI和aMapUI
const aMapJSAPILoad = aMapJSAPILoader.load();
const aMapUILoad = aMapUILoader.load();

// 使用
aMapJSAPILoad.then(AMap => {
  aMapUILoad.then(initAMapUI => {
    const AMapUI = initAMapUI(); // 这里调用initAMapUI初始化并返回AMapUI
    // 其他逻辑
  });
});
```



### AMapJS.load加载Loader：
```JavaScript
import AMapJS from "amap-js";

// 创建AMap JS加载器
let aMapJSAPILoader = new AMapJS.AMapJSAPILoader({ key: "您申请的key值" });

// 创建AMap UI组件库加载器
let aMapUILoader = new AMapJS.AMapUILoader();

// 使用AMapJS.load添加加载器
AMapJS.load([aMapJSAPILoader, aMapUILoader]).then(([AMap, initAMapUI]) => {
  const AMapUI = initAMapUI(); // 这里调用initAMapUI初始化并返回AMapUI
   // 其他逻辑
});
```



## 手册

### Loaders
- AMapJSAPILoader   -   高德地图JSAPI加载器
- AMapUILoader      -   高德地图UI加载器
- Loader      -   加载器基类

### Loader / load
- load([loaders]) - 同时加载多个Loaders。


## AMapJS API

#### AMapJSAPILoader
高德地图JSAPI加载器。

| 构造函数 | 说明 |
| :------ | :------ |
| AMapJS.AMapJSAPILoader(config:AMapJSAPILoaderConfig) | 构造一个高德地图JSAPI加载器，通过AMapJSAPILoaderConfig设置加载器属性。 |  

| AMapJSAPILoaderConfig | 说明 | 类型 | 默认值 |
| :------ | :------ | :------ | :------ |
| key | 您申请的高德key值，(实例化后该属性存在params中) | String | - |
| v | 高德地图JS API版本号，(实例化后该属性存在params中) | String | 1.4.12 |
| callback | 回调函数名，(实例化后该属性存在params中) | String | onAMapJS${随机数} |
| params | 脚本加载参数 | Object | null |
| protocol | 脚本加载协议 | String | https: |
| crossOrigin | 脚本crossOrigin属性 | String | anonymous |
| path | 资源地址 | String | webapi.amap.com/maps |
| keepScriptTag | 加载完成后是否保留脚本标签 | Boolean | false |  

| 方法 | 返回值 | 说明 |
| :------ | :------ | :------ |
| load( ) | Promise | 加载高德地图JSAPI。then接收AMap对象 |



### AMapUILoader
高德地图UI组件库。

| 构造函数 | 说明 |
| :------ | :------ |
| AMapJS.loaders.AMapUILoader(config:AMapUILoaderConfig) | 构造一个高德地图UI组件库加载器，通过AMapUILoaderConfig设置加载器属性。 |



| AMapUILoaderConfig | 类型 | 说明 |
| :------ | :------ | :------ |
| v | String | 高德UI组件库版本号，默认"1.0" |
| protocol | String | 加载组件库的脚本协议，默认https: |
| AMapUIProtocol | String\undefined | [ "https:" \ "http:" ] 默认情况下，组件加载优先使用与应用页面相同的协议(https:下用https:，http:或者file:下用http:)，如果需要强制https协议（比如file:场景下） |
| isAutoInitAMapUI | Boolean | 调用load()后，then接收的initAMapUI方法首次调用是否自动完成加载AMapUI实例的初始化创建, (开启后调用则直接返回AMapUI实例。关闭后调用则需先进行initAMapUI()初始化并返回AMapUI实例) |



| 方法 | 返回值 | 说明 |
| :------ | :------ | :------ |
| load( ) | Promise | 加载高德地图UI组件库。then接收initAMapUI方法 |


| 方法 | 返回值 | 说明 |
| :------ | :------ | :------ |
| load([loaders]) | Promise | then()接收一个Array顺序接收每个loader.load()的数据 |


## 许可
MIT
