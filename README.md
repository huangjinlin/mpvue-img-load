# mpvue-img-load

小程序图片预加载组件,基于 [mpvue](https://github.com/Meituan-Dianping/mpvue) 框架的重写[wxapp-img-loader](https://github.com/o2team/wxapp-img-loader),更好的满足工程化,模块化,自动化的需求

## 使用方式
1.安装依赖

``` bash
npm install mpvue-img-load
```

2.调用组件-单张图片

``` html
<div>
  <image v-if="imgUrl" :src="imgUrl" />
  <img-load ref="imgLoad"></img-load>
</div>
```

``` javascript
  import imgLoad from 'mpvue-img-load'

  export default {
    data () {
      return {
        imgUrl: ''
      }
    },
    components: {
      imgLoad
    },
    methods: {
      loadImage () {
        // 加载缩略图 80x50 3KB
        this.imgUrl = 'http://storage.360buyimg.com/mtd/home/lion1483683731203.jpg'
        // 原图 3200x2000 1.6MB
        const imgUrlOriginal = 'http://storage.360buyimg.com/mtd/home/lion1483624894660.jpg'
        // 同时对原图进行预加载，加载成功后再替换
        this.$refs.imgLoad.load(imgUrlOriginal, (err, data) => {
          if (!err) {
            this.imgUrl = data.src
          }
        })
      }
    }
  }
```
3.调用组件-多张图片

``` html
<div v-for="(item,index) in imgList" :key="index" class="img_wrap">
  <image v-if="item.loaded" :src="item.url" class="fade_in"/>
</div>
<img-load ref="imgLoad"></img-load>
```

``` javascript
import imgLoad from 'mpvue-img-load'

export default {
  data () {
    return {
      imgList: [
        {url:'http://img10.360buyimg.com/img/s600x600_jfs/t3586/215/2086933702/144606/c5885c8b/583e2f08N13aa7762.png',loaded:false},
        {url:'http://img10.360buyimg.com/img/s600x600_jfs/t3643/111/394078676/159202/a59f6b72/5809b3ccN41e5e01f.jpg',loaded:false},
      ]
    }
  },
  components: {
    imgLoad
  },
  methods: {
    loadImage () {
      this.imgList.forEach((item, index, list) => {
        this.$refs.imgLoad.load(item.url,(err, data) => {
          item.loaded = true
        })
      })
    }
  }
}
```

注:
(1) load 方法进行图片加载，第一个参数为图片链接，第二个参数可选，为该张图片加载完成时的回调方法
(2)图片加载完成的回调方法的第一个参数为错误信息（加载成功则为 null），第二个参数为图片信息（Object 类型，包括 src、width 及 height）。
(3)demo运行效果如下图所示：

![单张图片预加载](http://storage.360buyimg.com/mtd/home/single-img-load1483686270312.gif)
![多张图片预加载](http://storage.360buyimg.com/mtd/home/multi-img-load1483686388552.gif)
