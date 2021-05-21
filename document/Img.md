### vue cli静态资源可引用

- 在 JavaScript 被导入或在 template/CSS 中通过相对路径(以 . 开头)被引用(诸如 < img src="..." >、background: url(...) 和 CSS @import 的资源)。这类引用会被 webpack 处理。src目录下图片资源复制到dist时会文件名变为[name].[hash].png，小于4k的资源会转为base64。例如，url(./image.png) 会被翻译为 require('./image.png')
- 放置在 public 目录下图片需要通过绝对路径被引用。这类资源将会直接被拷贝到dist，而不会经过 webpack 的处理，你需要通过绝对路径来引用它们。
- 写在 template 中内联 style 的 background: url(...) 样式，在当前版本的测试中，即使使用了相对路径也不会被webpack处理，图片会找不到
```
直接在html模板里，在dom对象的style里加背景图，不起效果
<div style="height:100px; background:url('./assets/logo.png') center top no-repeat">不起效果。。。</div>
```

- 1、常见的引入方式，路径是固定的字符串，图片会被webpack处理，文件若丢失会直接在编译
```
时报错，生成的文件包含了哈希值
<img src="./assets/images/01.jpg" alt="">
//编译后:
<img src="/img/01.f0cfc21d.jpg" alt="">
```
- 2、 错误的引入方式，使用:src调用了v-bind指令处理其内容，相对路径不会被webpack的file-loader处理
```
<img :src="'./assets/images/02.jpg'" alt="">
// 编译后:
<img src="./assets/images/02.jpg" alt="">
```
- 3、当路径的文件名需要拼接变量的时候，可使用 require() 引入，在 template 的:src 或者 script 的 data computed 中都可以进行 require 引入或拼接
```
<img :src="require('./assets/images/03.jpg')" alt=""> // √
<img :src="require('./assets/images/'+ this.imgName +'.jpg')" alt=""> // √
<img :src="img3" alt=""> // √
<script>
export default:{
    data(){
        return {
          imgName:'03.jpg',
          img3:require('./assets/images/03.jpg'),
        }
      },
}
</script>
// 编译后:
<img src="/img/03.ea62525c.jpg" alt="">
```
- 4、用绝对路径引入时，路径读取的是public文件夹中的资源，任何放置在 public 文件夹的静态资源都会被简单的复制到编译后的目录中，而不经过 webpack特殊处理。
当你的应用被部署在一个域名的根路径上时，比如http://www.abc.com/，此时这种引入方式可以正常显示
但是如果你的应用没有部署在域名的根部，那么你需要为你的 URL 配置 publicPath 前缀
publicPath 是部署应用包时的基本 URL，在 vue.config.js 中进行配置
```
<img src="/images/04.jpg" alt=""> // -
// 编译后:
<img src="/images/04.jpg" alt="">
```

- 5、引入publicPath并且将其拼接在路径中，实现引入路径的动态变动
```
<img :src="this.publicPath + 'images/05.jpg'" alt=""> // √
// 编译后:
<img src="/foo/images/05.jpg" alt="">
<script>
export default:{
    data(){
        return {
          publicPath: process.env.BASE_URL,
        }
    },
}
</script>

//vue.config.js
module.exports = {
    publicPath:'/foo/',
    ...
}
```