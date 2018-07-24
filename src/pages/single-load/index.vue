<template>
  <div>
    <div class="img_wrap">
      <image v-if="imgUrl" :src="imgUrl" />
    </div>

    <button @click="loadImage">Click To Load Image</button>

    <view class="msg">{{ msg }}</view>
    <img-load ref="imgLoad"></img-load>
  </div>
</template>

<script>
  import imgLoad from '@/components/imgLoad'

  // 缩略图 80x50 3KB
  const imgUrlThumbnail = 'http://storage.360buyimg.com/mtd/home/lion1483683731203.jpg'
  // 原图 3200x2000 1.6MB
  const imgUrlOriginal = 'http://storage.360buyimg.com/mtd/home/lion1483624894660.jpg'

  export default {
    data () {
      return {
        msg: '',
        imgUrl: ''
      }
    },
    components: {
      imgLoad
    },
    methods: {
      loadImage () {
        // 加载缩略图
        this.msg = '大图正在拼命加载..'
        this.imgUrl = imgUrlThumbnail
        // 同时对原图进行预加载，加载成功后再替换
        setTimeout(() => {
          this.$refs.imgLoad.load(imgUrlOriginal, (err, data) => {
            console.log('图片加载完成', err, data.src)
            this.msg = '大图加载完成~'
            if (!err) {
              this.imgUrl = data.src
            }
          })
        }, 1500)
      },
      _imgOnLoad () {
        console.log('index._imgOnLoad')
      },
      _imgOnLoadError () {
        console.log('index._imgOnLoadError')
      }
    },
    created () {
    }
  }
</script>

<style scoped>
  .img_wrap {
      margin-bottom: 10px;
      width: 750rpx;
      height: 468rpx;
      background: #ececec;
  }

  image {
      width: 100%;
      height: 100%;
  }

  .msg {
      margin: 10px 0;
      text-align: center;
  }
</style>
