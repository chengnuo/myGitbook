# 支付宝支付 alipay

## 说明

[支付宝开发平台](https://openhome.alipay.com/platform/appDaily.htm?tab=info)


这里选择 [手机网站支付](https://docs.open.alipay.com/203)	商户在网页中调用支付宝提供的网页支付接口调起支付宝客户端内的支付模块，通过网页跳转到支付宝中完成支付。






## 官网

[支付宝开发平台](https://openhome.alipay.com/platform/appDaily.htm?tab=info)


## 掉坑经历

1.  提交数据用form提交。
```
<form id="alipaysubmit" name="punchout_form" method="post" action="">
  <input id="biz_content" type="hidden" name="biz_content" value="">
  <input type="submit" value="立即支付" style="display:none" >
</form>
```

2. js处理数据会使数据变成
```
原因，如果写死html，input的value浏览器会自动转义，提交的时候变成json=>{"out_trade_no":"2019080100000224","timeout_express":"90m","total_amount":"1.00"}
如果js赋值不会转义=>  {&quot;out_trade_no&quot;:&quot;2019080100000224&quot;,&quot;timeout_express&quot;:&quot;90m&quot;,&quot;total_amount&quot;:&quot;1.00&quot;}
-----分割线-----
反例：biz_content: {&quot;out_trade_no&quot;:&quot;2019080100000224&quot;,&quot;timeout_express&quot;:&quot;90m&quot;,&quot;total_amount&quot;:&quot;1.00&quot;}
正常：biz_content: {"out_trade_no":"2019080100000224","timeout_express":"90m","total_amount":"1.00"}
```

3. 需要用中间变量使数据格式化
```
 const div = document.createElement('div') // 获取到转义后的值
/* 此处form就是后台返回接收到的数据 */
div.innerHTML = bizContent; // 这句是关键
document.body.appendChild(div)
document.getElementById('biz_content').value = div.innerHTML // 这句是拿到转义后的值
document.forms.alipaysubmit.action = requestUrl
document.forms[0].submit()
```

4. 提交正确格式如图，这里是没开权限，开权限就会正常支付

![正确格式示例](/assets/images/blog/alipay.png)


## 参考

```

<template>
  <div class="aliPay">
    <div class="sendLayout">
        <van-button
                    type="default"
                    class="sendBtn"
                    @click="handlePay">去付款</van-button>
    </div>

    <form id="alipaysubmit" name="punchout_form" method="post" action="">
      <input id="biz_content" type="hidden" name="biz_content" value="">
      <input type="submit" value="立即支付" style="display:none" >
    </form>
  </div>
</template>

<script>

import commonJs from '@/util/common'
import wx from 'weixin-js-sdk'
import { Toast } from 'vant'

export default {
  name: 'aliPay',
  // https://cn.vuejs.org/v2/guide/components-props.html
  props: {
    dataSource: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: () => {
        return {}
      }
    },
    urlSource: {
      type: Object,
      // 对象或数组默认值必须从一个工厂函数获取
      default: () => {
        return {}
      }
    }
  },
  data() {
    return {
      payClientIp: '' // ip
    }
  },

  created() {
  },
  mounted() {
    this.init()
  },
  methods: {
    // 初始化
    init() {
      this.apiClientIp()
    },
    // 获取客户端IP
    async apiClientIp(callback) {
      const data = Object.assign({})
      const res = await this.$post(this.$api.payClientIp, data)
      try {
        if (res.code.toString() === '10000') {
          this.payClientIp = res.data.ip
          callback && callback()
        } else {
          Toast.fail(res.msg)
        }
      } catch (error) {
        Toast.fail(error)
      }
    },
    // 订单支付
    async apiPayMobileUnified(parameter, callback) {
      const data = Object.assign({}, parameter)
      const res = await this.$post(this.$api.payMobileUnified, data)
      try {
        if (res.code.toString() === '10000') {
          callback && callback(res)
        } else {
          Toast.fail(res.msg)
        }
      } catch (error) {
        console.log('error', error)
        Toast.fail(error)
      }
    },
    // 支付宝支付
    handlePay(target) {
      console.log('target', target)
      console.log('bankCode', this.bankCode)
      const prePayNo = this.$route.query.prePayNo
      const mercId = this.$route.query.mercId
      // const openid = this.$route.query.openid
      // const source = this.$route.query.source


      const callbackUrl = `${commonJs.goPayConfig(this.urlSource.source).PAYPaySucceed}` // 跳回原来地方

      const dataTwo = {
        methodType: 'DirectPay',
        prePayNo: prePayNo,
        mercId: mercId,
        tradeType: 'MWEB',
        bankCode: 'ALIPAY',
        // openId: this.userData.wxOpenId, // h5支付没有openId
        callbackUrl: callbackUrl,
        clientIp: this.payClientIp, // 获取ip
        sysCnl: 'H5'
      }
      this.apiPayMobileUnified(dataTwo, (resTwo) => {
        if (resTwo.code.toString() === '10000') {
          const bizContent = resTwo.data.bizContent
          const requestUrl = resTwo.data.requestUrl
          const div = document.createElement('div') // 获取到转义后的值
          /* 此处form就是后台返回接收到的数据 */
          div.innerHTML = bizContent
          document.body.appendChild(div)
          document.getElementById('biz_content').value = div.innerHTML
          document.forms.alipaysubmit.action = requestUrl
          document.forms[0].submit()
        } else {
          console.log(resTwo)
        }
      }).catch((error) => {
        console.error(error)
      })
    }

  }
}
</script>

<style lang="less" scoped>
.aliPay {
  .sendLayout {
    padding: 20px;
    .sendBtn {
      width: 100%;
      height: 44px;
      background: #333333;
      color: #fff;
      border-radius: 4px;
    }
  }
}
</style>


```
