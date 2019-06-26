# 微信支付 wxpay

## 说明

[微信支付商户平台](https://pay.weixin.qq.com/static/product/product_index.shtml#payment_product)

微信支付，下面记录一下开发的步骤和细节，微信支付h5开发分2种 `h5支付（非微信浏览器）` `jsapi支付 （微信浏览器）`


### 判断是否微信浏览器

```
/**
 * 判断是否微信浏览器
 * @method true 微信
 * @method false 非微信
 */
function isWeiXin() {
  var ua = window.navigator.userAgent.toLowerCase()
  if (ua.match(/MicroMessenger/i) == 'micromessenger') {
    return true
  } else {
    return false
  }
}
```


### h5支付（非微信浏览器）


[h5支付产品流程](https://pay.weixin.qq.com/static/product/product_intro.shtml?name=h5)

[h5支付开发文档](https://pay.weixin.qq.com/wiki/doc/api/H5.php?chapter=15_1)

[h5支付常见问题](https://pay.weixin.qq.com/wiki/doc/api/H5.php?chapter=15_4)




开发流程

正常流程用户支付完成后会返回至发起支付的页面，如需返回至指定页面，则可以在MWEB_URL后拼接上redirect_url参数，来指定回调页面。
如，您希望用户支付完成后跳转至https://www.wechatpay.com.cn，则可以做如下处理：
```
假设您通过统一下单接口获到的MWEB_URL= https://wx.tenpay.com/cgi-bin/mmpayweb-bin/checkmweb?prepay_id=wx20161110163838f231619da20804912345&package=1037687096
则拼接后的地址为MWEB_URL= https://wx.tenpay.com/cgi-bin/mmpayweb-bin/checkmweb?prepay_id=wx20161110163838f231619da20804912345&package=1037687096&redirect_url=https%3A%2F%2Fwww.wechatpay.com.cn
```

注：

```
链接拼接后不可以直接在浏览器打开，需要通过

1，window.location.href
2，<a href="xxx">xxx</a>

这2种方式进行跳转
```

h5 支付代码片段
```
// h5支付，需要在非微信浏览器调起
handleH5Pay() {
  const prePayNo = this.orderSubmit.prepayNo
  const mercId = this.orderSubmit.mercId

  const dataTwo = {
    methodType: 'DirectPay',
    prePayNo: prePayNo,
    mercId: mercId,
    tradeType: 'MWEB',
    bankCode: 'WEIXIN',
    // openId: this.userData.wxOpenId, // 非必传
    callbackUrl: 'http://test-h5.mingpinmao.cn/pay',
    // callbackUrl: 'https%3A%2F%2Fwww.baidu.com%2F',
    clientIp: this.payClientIp // 获取ip
  }
  console.log('dataTwo', JSON.stringify(dataTwo))
  this.apiPayMobileUnified(dataTwo, (resTwo) => {
    this.mwebUrl = resTwo.data.mwebUrl
    window.location.href = resTwo.data.mwebUrl
  })
},
```



### jsapi支付 （微信浏览器）

[jsapi开发流程](https://pay.weixin.qq.com/static/product/product_intro.shtml?name=jsapi)

[jsapi开发文档](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_1)

[jsapi常见文件](https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_9&index=8)

1, [登录授权获取openid](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421140842)


2，使用 weixin-js-sdk 开发 [微信JS-SDK说明文档](https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115)

jsapi开发片段
```
1，wx.config

const dataTwo = {
  methodType: 'DirectPay',
  prePayNo: prePayNo,
  mercId: mercId,
  tradeType: 'JSAPI',
  bankCode: 'WEIXIN',
  // openId: this.userData.wxOpenId, // 去掉openId测试
  openId: getLoginAuthorization.openid,
  clientIp: this.payClientIp // 获取ip
}
this.apiPayMobileUnified(dataTwo, (resTwo) => {
  this.chooseWXPay = Object.assign({}, resTwo.data)
  const wxData = {
    debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
    appId: this.chooseWXPay.appId, // 必填，公众号的唯一标识
    timestamp: this.chooseWXPay.timeStamp, // 必填，生成签名的时间戳
    nonceStr: this.chooseWXPay.nonceStr, // 必填，生成签名的随机串
    signature: this.chooseWXPay.sign, // 必填，签名
    jsApiList: ['chooseWXPay'] // 必填，需要使用的JS接口列表
  }
  // wx.config
  wx.config(wxData)
})


2，调起支付，需要在微信浏览器调起
// 调起微信浏览器支付
handleWxPay() {
  const self = this
  wx.ready(function() { // 需在用户可能点击分享按钮前就先调用
    wx.chooseWXPay({
      timestamp: self.chooseWXPay.timeStamp, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
      nonceStr: self.chooseWXPay.nonceStr, // 支付签名随机串，不长于 32 位
      package: self.chooseWXPay.package, // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
      signType: self.chooseWXPay.signType, // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
      paySign: self.chooseWXPay.sign, // 支付签名
      success: function(res) {
        console.log('支付成功后的回调函数', res)
        // 支付成功后的回调函数
      },
      fail: function(res) {
        console.log('支付成功后的失败回调函数', res)
      }
    })
  })
  wx.error(function(res) {
    console.log('wx.error', res)
    // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。
  })
}
```



## 官网

[微信支付商户平台](https://pay.weixin.qq.com/static/product/product_index.shtml#payment_product)


## 掉坑经历

1，redirect_uri域名与后台配置不一致，错误代码10003

```
解决参考：
https://blog.csdn.net/weixin_38306434/article/details/81384223
https://blog.csdn.net/pansanday/article/details/80685503
openid 是否一致，我就是搞混了，用了小程序的openid，其实是用公众号的openid。因为我们这小程序和公众号都叫jsapi
https://blog.csdn.net/qq_36943008/article/details/81360051
```


## 参考

```

<!-- 支付成功回调 -->
<template>
  <div class="pay">
    <div class="orderInfoLayout">
      <div class="orderInfo">
        <span class="leftbar">订单编号：</span>
        <span class="rightbar">{{orderSubmit.orderNo}}</span>
      </div>
      <div class="orderInfo">
        <span class="leftbar">支付金额：</span>
        <span class="rightbar">{{orderDetail.orderTotalPrice}}</span>
      </div>
    </div>

    <!-- 微信支付 -->
    <div class="payType">
      <div class="title">支付方式</div>
      <van-radio-group v-model="pay">
        <van-cell-group>
          <van-cell title=""
                    clickable
                    @click="pay = '1'">
            <template slot="title">
              <span class="custom-text">
                <img src="../../assets/img/order/pay_01@2x.png"
                     class="wechatPayImg" />
                <span class="wechatPayText">微信</span>
              </span>
            </template>
            <van-radio name="1" />
          </van-cell>
        </van-cell-group>
      </van-radio-group>
    </div>

    <div class="sendLayout">
      <van-button type="default"
                  class="sendBtn"
                  @click="handlePay">去付款</van-button>
    </div>

    <!-- <div @click="handlePay">非微信浏览器支付</div>
    <div @click="onBridgeReady">微信浏览器支付</div> -->
  </div>
</template>

<script>

import wx from 'weixin-js-sdk'
import commonJs from '@/util/common'
import { Toast } from 'vant'

// console.log('wx', wx)

export default {
  name: 'pay',
  data() {
    return {
      active: 2,
      userData: {},
      orderSubmit: {}, // 订单提交后信息
      mwebUrl: '',
      chooseWXPay: {}, // 微信浏览器支付
      orderDetail: {}, // 订单信息
      pay: '1',
      payClientIp: '' // 获取客户端IP
    }
  },
  mounted() {
    this.init()
  },
  methods: {
    // 初始化
    init() {
      this.userData = JSON.parse(sessionStorage.getItem('userData')) || {}
      this.orderSubmit = JSON.parse(window.sessionStorage.getItem('orderSubmit')) || {}
      this.orderDetail = JSON.parse(window.sessionStorage.getItem('orderDetail')) || {}
      this.apiClientIp(() => {
        if (commonJs.isWeiXin()) {
          this.initPay() // 微信支付
        }
      })
    },
    // 获取客户端IP
    apiClientIp(callback) {
      const data = Object.assign({})
      this.$post(this.$api.payClientIp, data, commonJs.jsSign(data))
        .then(res => {
          if (res.code == 10000) {
            this.payClientIp = res.data.ip
            callback && callback()
          } else {
            Toast.fail(res.msg)
          }
        })
        .catch(res => {
          Toast.fail(res.msg)
          console.log('fail', res)
        })
    },
    // 点击支付的时候
    handlePay() {
      // console.log('isWeiXin', this.isWeiXin())
      if (commonJs.isWeiXin()) {
        this.handleWxPay() // 微信支付
      } else {
        this.handleH5Pay() // h5支付
      }
    },
    // 初始化-微信浏览器支付
    initPay() {
      const prePayNo = this.orderSubmit.prepayNo
      const mercId = this.orderSubmit.mercId
      const getLoginAuthorization = JSON.parse(window.sessionStorage.getItem('loginAuthorization')) || {}
      if (getLoginAuthorization && JSON.stringify(getLoginAuthorization) !== '{}') {
        const dataTwo = {
          methodType: 'DirectPay',
          prePayNo: prePayNo,
          mercId: mercId,
          tradeType: 'JSAPI',
          bankCode: 'WEIXIN',
          // openId: this.userData.wxOpenId, // 去掉openId测试
          openId: getLoginAuthorization.openid,
          clientIp: this.payClientIp // 获取ip
        }
        this.apiPayMobileUnified(dataTwo, (resTwo) => {
          this.chooseWXPay = Object.assign({}, resTwo.data)
          const wxData = {
            debug: false, // 开启调试模式,调用的所有api的返回值会在客户端alert出来，若要查看传入的参数，可以在pc端打开，参数信息会通过log打出，仅在pc端时才会打印。
            appId: this.chooseWXPay.appId, // 必填，公众号的唯一标识
            timestamp: this.chooseWXPay.timeStamp, // 必填，生成签名的时间戳
            nonceStr: this.chooseWXPay.nonceStr, // 必填，生成签名的随机串
            signature: this.chooseWXPay.sign, // 必填，签名
            jsApiList: ['chooseWXPay'] // 必填，需要使用的JS接口列表
          }
          // wx.config
          wx.config(wxData)
        })
      } else {
        Toast.fail('openid不存在')
      }
    },
    // h5支付
    handleH5Pay() {
      const prePayNo = this.orderSubmit.prepayNo
      const mercId = this.orderSubmit.mercId

      const dataTwo = {
        methodType: 'DirectPay',
        prePayNo: prePayNo,
        mercId: mercId,
        tradeType: 'MWEB',
        bankCode: 'WEIXIN',
        // openId: this.userData.wxOpenId, // 非必传
        callbackUrl: 'http://test-h5.mingpinmao.cn/pay',
        // callbackUrl: 'https%3A%2F%2Fwww.baidu.com%2F',
        clientIp: this.payClientIp // 获取ip
      }
      console.log('dataTwo', JSON.stringify(dataTwo))
      this.apiPayMobileUnified(dataTwo, (resTwo) => {
        this.mwebUrl = resTwo.data.mwebUrl
        window.location.href = resTwo.data.mwebUrl
      })
    },
    // 订单支付
    apiPayMobileUnified(parameter, callback) {
      const data = Object.assign({}, parameter)
      this.$post(this.$api.payMobileUnified, data, commonJs.jsSign(data))
        .then(res => {
          if (res.code == 10000) {
            // Toast.success(res.msg)
            callback && callback(res)
          } else {
            Toast.fail(res.msg)
          }
        })
        .catch(res => {
          Toast.fail(res.msg)
          console.log('fail', res)
        })
    },
    // 调起微信浏览器支付
    handleWxPay() {
      const self = this
      wx.ready(function() { // 需在用户可能点击分享按钮前就先调用
        wx.chooseWXPay({
          timestamp: self.chooseWXPay.timeStamp, // 支付签名时间戳，注意微信jssdk中的所有使用timestamp字段均为小写。但最新版的支付后台生成签名使用的timeStamp字段名需大写其中的S字符
          nonceStr: self.chooseWXPay.nonceStr, // 支付签名随机串，不长于 32 位
          package: self.chooseWXPay.package, // 统一支付接口返回的prepay_id参数值，提交格式如：prepay_id=\*\*\*）
          signType: self.chooseWXPay.signType, // 签名方式，默认为'SHA1'，使用新版支付需传入'MD5'
          paySign: self.chooseWXPay.sign, // 支付签名
          success: function(res) {
            console.log('支付成功后的回调函数', res)
            // 支付成功后的回调函数
          },
          fail: function(res) {
            console.log('支付成功后的失败回调函数', res)
          }
        })
      })
      wx.error(function(res) {
        console.log('wx.error', res)
        // config信息验证失败会执行error函数，如签名过期导致验证失败，具体错误信息可以打开config的debug模式查看，也可以在返回的res参数中查看，对于SPA可以在这里更新签名。
      })
    }
  },
  components: {}
}
</script>

<style lang="less" scoped>
.pay {
  background: #f5f5f5;
  position: absolute;
  width: 100%;
  height: 100%;
  .orderInfoLayout {
    padding: 15px 19px 18px;
    background: #fff;
    .orderInfo {
      .leftbar {
        font-size: 15px;
        font-weight: 400;
        color: rgba(51, 51, 51, 1);
        line-height: 21px;
      }
      .rightbar {
        font-size: 15px;
        font-weight: 400;
        color: rgba(153, 153, 153, 1);

        line-height: 21px;
      }
    }
  }
  .callbackTip {
    width: 100%;
    position: absolute;
    left: 0;
    top: 130px;
    .image {
      width: 260px;
      height: 260px;
      display: block;
      margin: 0 auto;
      font-size: 260px;
    }
    .text {
      width: 100%;
      height: 36px;
      font-size: 26px;
      font-weight: 400;
      color: rgba(153, 153, 153, 1);
      line-height: 36px;
      text-align: center;
      display: block;
    }
    .download {
      text-align: center;
    }
  }

  .payType {
    .title {
      font-size: 16px;
      font-weight: 500;
      color: rgba(51, 51, 51, 1);
      line-height: 44px;
      padding: 0 19px;
    }
    /deep/ .van-radio-group {
      padding: 0px;
      .van-radio__icon--checked {
        .van-icon {
          background: rgba(214, 45, 45, 1);
          border-color: rgba(214, 45, 45, 1);
        }
      }
      .van-cell__title {
        line-height: 40px;
      }
      .van-cell__value {
        flex: none;
      }
      .van-radio {
        padding: 10px;
      }
      .van-cell {
        padding: 0 20px;
      }
      .wechatPayImg {
        width: 22px;
        height: 20px;
        display: inline-block;
        vertical-align: middle;
      }
      .wechatPayText {
        display: inline-block;
        vertical-align: middle;
      }
    }
  }
  .sendLayout {
    padding: 20px;
    .sendBtn {
      width: 100%;
      height: 44px;
      background: rgba(214, 45, 45, 1);
      color: #fff;
      border-radius: 4px;
    }
  }
}
</style>


```
