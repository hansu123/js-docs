https模块


1.客户端请求 https.get(url,[options],callback)

```
const https = require("https");
var url = "https://juejin.im/post/5c602ba6e51d457fc75f7d09";
https.get(url, (res) => {
    console.log(res.statusCode);//获取状态码
    console.log(res.headers);//获取请求头
    res.on('data', (data) => {
        console.log(data.toString());
    });//获取网页数据
});
```
2.req相关的api


3.res相关的api

