<template>
  <div>
    <image v-for="(item,index) in imgLoadList" :key="index" :src="item" :data-src="item" @load="_imgOnLoad" @error="_imgOnLoadError" style="width:0;height:0;opacity:0;" />
  </div>
</template>
<script>
  export default {
    data: () => {
      return {
        imgLoadList: [],
        callbacks: {},
        imgInfo: {},
        defaultCallback: function () {}
      }
    },
    methods: {
      load (src, callback) {
        if (!src) return
        let list = this.imgLoadList
        let imgInfo = this.imgInfo[src]

        if (callback) {
          this.callbacks[src] = callback
        }
        if (imgInfo) {
          this._runCallback(null, {
            src: src,
            width: imgInfo.width,
            height: imgInfo.height
          })
        } else if (list.indexOf(src) === -1) {
          list.push(src)
          this.imgLoadList = list
        }
      },
      _imgOnLoad (ev) {
        let src = ev.currentTarget.dataset.src
        let width = ev.target.width
        let height = ev.target.height

        // 记录已下载图片的尺寸信息
        this.imgInfo[src] = { width, height }
        this._removeFromLoadList(src)

        this._runCallback(null, { src, width, height })
      },
      _imgOnLoadError (ev) {
        let src = ev.currentTarget.dataset.src
        this._removeFromLoadList(src)
        this._runCallback('Loading failed', { src })
      },
      _removeFromLoadList (src) {
        let list = this.imgLoadList
        list.splice(list.indexOf(src), 1)
        this.imgLoadList = list
      },
      _runCallback (err, data) {
        let callback = this.callbacks[data.src] || this.defaultCallback
        callback(err, data)
        delete this.callbacks[data.src]
      }
    }
  }
</script>
