# 编辑器点击跳转 webpack alias 别名

## 需求

在 WebStorm 中，配置能够识别 Vue CLI 3 创建的项目的别名 alias @


## 1. 直接引入

```js
import util from './libs/util.js';
```

这个优点可以直接使用。缺点是文件目录多了，就需要 ../../ 等

## 2. 别名

我们使用 Webpack 经常会配置一些别名 alias 指向特定的目录，这样在使用 import 等语句时就不用写一大堆的相对路径了，常见的是将项目的 src 设置为 @，比如某文件的路径是 src/libs/util.js，配置后，任何地方就可以这样导入：

```js
import util from '@/libs/util.js';
```


当然，这是很基础的 Webpack 配置，但本文介绍的是 IDE WebStorm 能够通过 Command + 点击左键直接进入别名的某个目录下。WebStorm 默认是能够解析 webpack 配置文件的，也就是说你配置了 webpack 别名 alias 后，天生就支持这种快速跳转文件。

但是 Vue CLI 3 里，是没有传统的 webpack 配置文件的，因为使用另一种写法集成到了 vue.config.js 里，这时 WebStorm 就无法识别了，以至于上例导入进来的 util 在上下文里都无法被 WebStorm 解析到，那使用 WebStorm 的意义就大打折扣了。

这时，我们只需要在项目根目录创建一个文件 alias.config.js：


```js
/**
 * 由于 Vue CLI 3 不再使用传统的 webpack 配置文件，故 WebStorm 无法识别别名
 * 本文件对项目无任何作用，仅作为 WebStorm 识别别名用
 * 进入 WebStorm preferences -> Language & Framework -> JavaScript -> Webpack，选择这个文件即可
 * */
const resolve = dir => require('path').join(__dirname, dir);

module.exports = {
    resolve: {
        alias: {
            '@': resolve('src')
        }
    }
};
```


这个配置文件对于项目无任何意义，也没有任何地方使用到，只是为了让 WebStorm 能够识别，在 WebStorm 的配置里，将它选为 webpack 配置文件就大功告成了：

![webpack配置图](/assets/images/webpackAlias.png)

而且，设置仅对当前项目有效，并不会影响到其它项目。
快去试试吧！


## 3. node path

通过node path方式引入兼容所有编辑器，本人并没有研究出来。
http://nodejs.cn/api/path.html

## 参考

```
https://dev.iviewui.com/articles/1108915726372179968https://dev.iviewui.com/articles/1108915726372179968
```
