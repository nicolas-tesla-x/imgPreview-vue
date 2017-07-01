<template>
  <div id="r-img-preview">
    <ul class="img-list">
      <li class="img" v-for="(img, index) in imgs" :key="index">
        <img :src="img" alt="" @click.stop="bigImg(index)">
      </li>
    </ul>
    <transition name="fade">
      <div class="pswp" tabindex="-1" role="dialog" aria-hidden="true">
        <div class="pswp__bg"></div>
        <div class="pswp__scroll-wrap">
          <div class="pswp__container">
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
            <div class="pswp__item"></div>
          </div>
          <div class="pswp__ui pswp__ui--hidden">
            <div class="pswp__top-bar">
              <div class="pswp__counter"></div>
              <button class="pswp__button pswp__button--close" title="Close (Esc)"></button>
              <button class="pswp__button pswp__button--share" title="Share"></button>
              <button class="pswp__button pswp__button--fs" title="Toggle fullscreen"></button>
              <button class="pswp__button pswp__button--zoom" title="Zoom in/out"></button>
              <div class="pswp__preloader">
                <div class="pswp__preloader__icn">
                  <div class="pswp__preloader__cut">
                    <div class="pswp__preloader__donut"></div>
                  </div>
                </div>
              </div>
            </div>
            <div class="pswp__share-modal pswp__share-modal--hidden pswp__single-tap">
              <div class="pswp__share-tooltip"></div>
            </div>
            <button class="pswp__button pswp__button--arrow--left" title="Previous (arrow left)">
            </button>
            <button class="pswp__button pswp__button--arrow--right" title="Next (arrow right)">
            </button>
            <div class="pswp__caption">
              <div class="pswp__caption__center"></div>
            </div>
          </div>
        </div>
      </div>
    </transition>
  </div>
</template>
<script>
// 获取PhotoSwipe构造器
import PhotoSwipe from './js/photoswipe'
import PhotoSwipeUIDefault from './js/photoswipe-ui-default'
export default {
  data () {
    return {
      show: false,
      rImgPreview: null
    }
  },
  props: ['imgs'],
  components: {
  },
  methods: {
    bigImg (index) {
      if (!this.rImgPreview) {
        this.rImgPreview = this.$el
      }
      let pswpElement = this.rImgPreview.getElementsByClassName('pswp')[0]
      // 创建图像数组
      let [items, len, i] = [[], this.imgs.length, 0]
      // 给图像数组中添加图像对象
      for (; i < len; i++) {
        let img = new Image()
        img.src = this.imgs[i]
        items[i] = {
          src: this.imgs[i],
          w: img.width,
          h: img.height,
          pid: 'img_' + (i + 1)
        }
        img = null  // 图像信息添加完成后，释放一开始创建的Image对象
      }
      // define options (if needed)
      let options = {
        // optionName: 'option value'
        // for example:
        galleryPIDs: true, // 为形成URL时使用的每个幻灯片对象启用自定义ID
        maxSpreadZoom: 2, // 图片放大倍数
        tapToClose: true, // 轻触屏幕关闭预览界面
        index: index, // 显示的第一张大图
        // 图片预览界面的按钮/元素
        closeEl: false, // 关闭按钮
        captionEl: false, // 标题元素
        fullscreenEl: false, // 全屏按钮
        zoomEl: true, // 缩放
        shareEl: false, // 分享按钮
        counterEl: true, // 计数器
        arrowEl: true, // 箭头
        preloaderEl: true // 预加载
      }
      // 初始化
      let gallery = new PhotoSwipe(pswpElement, PhotoSwipeUIDefault, items, options)
      gallery.init()
    }
  }
}

</script>
<style scoped>
  @import "./css/default-skin/default-skin.css";
  @import "./css/photoswipe.css";
  .fade-enter-active, .fade-leave-active {
    transition: opacity .5s
  }
  .fade-enter, .fade-leave-to /* .fade-leave-active in <2.1.8 */ {
    opacity: 0
  }
  #r-img-preview > .img-list img {
    width: 100%;
  }
</style>
