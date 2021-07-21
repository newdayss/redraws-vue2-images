# redraws-vue2-images

## install
```
yarn add redraws-vue2-images
or
npm install redraws-vue2-images
```
## use

`<RedrawImage :type="**" :url="**"></RedrawImage>`
### gitgub 
```
Not yet
```

### doc

- type | 默认值1 | 1:简易模糊模式，2:canvas模糊模式 
 **(canvas 模糊需要后端图片支持跨域（即Access-Control-Allow-Origin: *模式）,否则只能绘制同源图片，
 同时本组件提供传入url转base64接口地址参数,配合后端接口方式实现绘制跨域绘制)**

- blurOn | 默认值true | true:打开模糊，false:关闭模糊
- gauss | 默认值1 | canvas 模糊程度，数值越大程度越高
****************
**本区间为dev调试模式开关，pro环境一般不适用，除非在服务器上配置了相应的ngnix**
- isDev | 默认为false  | false:关闭dev模式，true：打开dev模式
- localhost | 默认为"" | 值为空时，读取本地localhost域名替换图片路径,有值时用值替换图片路径
- proxyName | 默认为"" | 设置的代理路径，值为空时需要设置代理为"/",有值时读取/proxyName为代理路径
- vue.config.js配置为
```javascript
 proxy: {
   "/proxyName": {
        target: "http://********:port/",
        changeOrigin: true,
        pathRewrite: {
          "/proxyName": ""
        }
      }
   }
```
- httpType | 默认为http | http:图片模式替换为http模式,https:图片模式替换为https模式
****************
#### isBase64
- 此模式与isdev 相关配置为互斥事件，isBase64模式生效时isdev
- (接受接口为code为0,data为base64图片的返回值,即{code:0,success:true,data:"***"})
- isBase64 | 默认为fasle | false:关闭使用接口转换为base64,true:打开使用接口转换为base64 **(循环时慎用)**
- base64Api | 默认为"" | 后端提供的url转base64接口地址
- methodType | 默认为"get" | base64Api该接口的请求方式

### author
```
wangbo
```
