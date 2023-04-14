<!--
 * @Description: 
 * @version: 1.0.0
 * @Author: 曹双双
 * @Date: 2023-04-13 11:11:48
 * @LastEditors: 曹双双
 * @LastEditTime: 2023-04-14 17:49:50
-->

<template>
  <div ref="popoverRef" class="popover-wrap" @click="handleClick" @mouseenter="handleMouseEnter" @mouseleave="handleMouseLeave">
    <slot></slot>
    <div v-show="isPopoverVisible" ref="popoverMainRef" class="popover-main" :style="popoverStyle">
      <h4 class="popover-title">{{ title }}</h4>
      <p class="popover-content">{{ content }}</p>
    </div>
  </div>
</template>
<style scoped lang="less">
.popover-wrap {
  display: inline-block;
  position: relative;
  box-sizing: border-box;

  .popover-main {
    position: absolute;
    padding: 12px;
    font-size: 14px;
    background: #fff;
    background-clip: padding-box;
    border-radius: 8px;
    box-shadow: 0 6px 16px 0 rgba(0, 0, 0, .08), 0 3px 6px -4px rgba(0, 0, 0, .12), 0 9px 28px 8px rgba(0, 0, 0, .05);
    z-index: 999;

    h4,
    p {
      margin: 0;
    }

    .popover-title {
      min-width: 177px;
      margin-bottom: 8px;
      color: rgba(0, 0, 0, 0.88);
      font-weight: 600;
      text-align: left;
    }

    .popover-content {
      color: rgba(0, 0, 0, 0.88);
      text-align: left;
    }
  }
}
</style>
<script>
export default {
  name: 'Popover',
  props: {
    trigger: {
      type: String,
      default: 'click'
    },
    placement: {
      type: String,
      default: 'L'
    },
    title: {
      type: String,
      default: ''
    },
    content: {
      type: String,
      required: true
    }
  },
  data() {
    return {
      gap: 15, //popover与元素的间距
      width: 0, //元素宽
      height: 0, //元素高
      left: 0, //元素左侧与视口左侧距离
      right: 0, //元素右侧与视口左侧距离
      bottom: 0, //元素低部与视口顶部距离
      top: 0, //元素顶部与视口顶部距离
      mainWidth: 0, //popover宽
      mainHeight: 0, //popover高
      clientWidth: 0, //视口宽
      clientHeight: 0, //视口高
      isPopoverVisible: false,
    }
  },
  computed:{
    popoverStyle() {
      let styleObj = {}
      switch (this.placement) {
        case 'L':
          styleObj.left = this.left > this.mainWidth + this.gap ? `-${this.mainWidth + this.gap}px` : `${this.width + this.gap}px`
          styleObj.top = this.clientHeight - this.top > this.mainHeight ? 0 : `${this.mainHeight - this.height}px`
          break

        case 'T':
          styleObj.left = this.clientWidth - this.left > this.mainWidth ? 0 : `${this.width - this.mainWidth}px`
          styleObj.top = this.top > this.mainHeight + this.gap ? `-${this.mainHeight + this.gap}px` : `${this.height + this.gap}px`;
          break;

        case 'R':
          styleObj.left = this.clientWidth - this.right < this.mainHeight + this.gap ? `-${this.mainWidth + this.gap}px` : `${this.width + this.gap}px`
          styleObj.top = this.clientHeight - this.top > this.mainHeight ? 0 : `${this.height - this.mainHeight}px`
          break
        case 'B':
          styleObj.left = this.clientWidth - this.left > this.mainWidth ? 0 : `${this.width - this.mainWidth}px`
          styleObj.top = this.clientHeight - this.bottom > this.mainHeight + this.gap ? `${this.height + this.gap}px` : `-${this.mainHeight + this.gap}px`;
          break;
      }
      return styleObj
    }
  },
  mounted() {
    const { clientWidth, clientHeight } = document.documentElement
    const rect = this.$refs.popoverRef.getBoundingClientRect()
    this.width = rect.width
    this.height = rect.height
    this.left = rect.left
    this.top = rect.top
    this.right = rect.right
    this.bottom = rect.bottom
    this.clientHeight = clientHeight
    this.clientWidth = clientWidth
  },
  methods: {
    initMainRect() {
      if (this.isPopoverVisible) {
        this.$nextTick(() => {
          const mainRect = this.$refs.popoverMainRef.getBoundingClientRect()
          this.mainHeight = mainRect.height
          this.mainWidth = mainRect.width
        })
      }
    },
    handleClick() {
      if(this.trigger!=='click'){
        return
      }
      this.isPopoverVisible = !this.isPopoverVisible
      this.initMainRect()
    },
    handleMouseEnter() {
      if(this.trigger!=='hover') {
        return
      }
      this.isPopoverVisible=true
      this.initMainRect()
    },
    handleMouseLeave() {
      if(this.trigger!=='hover') {
        return
      }
      this.isPopoverVisible=false
      this.initMainRect()
    }
  }
}
</script>
