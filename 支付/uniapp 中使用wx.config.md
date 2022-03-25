#### `uniapp`  中使用 `wx.config` 报错

> 原因是uniapp里面内置了一个名为wx的全局变量,结果是肯定的
>
> wx变量都被重写了，wx.config肯定也是undefined了

#### 解决

> ```shell
> npm install jweixin-module --save
> ```

##### 使用 `jweixin-module`

```js
const jweixin = require("jweixin-module")
```

```js
// 微信支付
toWXPay(payParams) {
    const that = this
    jweixin.config({
        debug: false,
        appId: payParams.AppId,
        timestamp: payParams.Timestamp,
        nonceStr: payParams.NonceStr,
        signature: payParams.Signature,
        jsApiList: ['chooseWXPay']
    })
    jweixin.ready(res => {
        jweixin.checkJsApi({
            jsApiList: ['chooseWXPay'],
            success: res => {
                console.log('checked api success:', res)
            },
            fail: err => {
                console.log('check api fail:', err)
            }
        })
    })
    jweixin.chooseWXPay({
        timestamp: payParams.Timestamp, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
        nonceStr: payParams.NonceStr, // 支付签名随机串，不长于 32 位
        package: payParams.Package, // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
        signType: payParams.SignType, // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
        paySign: payParams.PaySign, // 支付签名
        success: (res) => {
            // 支付成功后的回调函数
            // 跳转到支付成功页
            that.isPay = true
            uni.reLaunch({
                url: "pages/tvGamePay/paySuccess"
            })
            console.log('支付成功：', res)
        },
        cancel: (err) => {
            // tips.innerHTML = "支付失败！"
            console.log('支付失败：', err)
            // 支付失败后的回调函数
        },
    });
},
```



##### 参考文献

[uniapp 中出现 wx.config is not a function](https://www.cnblogs.com/shiazhen/p/14113526.html)

[UNI-APP 开发微信公众号（H5）JSSDK 的使用方式](https://ask.dcloud.net.cn/article/35380)

