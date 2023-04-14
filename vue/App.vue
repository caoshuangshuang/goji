<!--
 * @Description: 
 * @version: 1.0.0
 * @Author: 曹双双
 * @Date: 2023-04-13 11:11:48
 * @LastEditors: 曹双双
 * @LastEditTime: 2023-04-14 17:50:52
-->

<template>
  <div class="App">
    <h3>触发方式</h3>
    <button :class="trigger==='click'?'active':''" @click="handleTriggerChange('click')">click</button>
    <button :class="trigger==='hover'?'active':''"  @click="handleTriggerChange('hover')">hover</button>

    <h3>方向</h3>
    <button :class="placement==='L'?'active':''" @click="handlePlacementChange('L')">L</button>
    <button :class="placement==='R'?'active':''"  @click="handlePlacementChange('R')">R</button>
    <button :class="placement==='T'?'active':''"  @click="handlePlacementChange('T')">T</button>
    <button :class="placement==='B'?'active':''"  @click="handlePlacementChange('B')">B</button>

    <div style="margin-top:30px"></div>
    <h3>效果展示</h3>
    <div style="text-align: left;">
      <Popover :trigger="trigger" :placement="placement" content="我是内容我是内容我是内容" title="我是title">
      <div class="popover-text">popover</div>
    </Popover>
    </div>
    

  </div>
</template>

<script>
import Popover from '@/components/Popover.vue'
export default {
  components: { Popover },
  name: 'App',

  data() {
    return {
      trigger:'click',
      placement:'R'
    };
  },
  methods: {
    handleTriggerChange(type) {
      this.trigger=type
    },

    handlePlacementChange(placement) {
      this.placement=placement
    }
  },
};
</script>
<style lang="less" scoped>
.App{
  padding:30px;

  .popover-text{
    border:1px solid #eee;
    padding:8px;
    border-radius: 4px;
  }
}
button{
  padding:7px;
  margin-right: 20px;
  background: #fff;
  border:1px solid #eee;
  border-radius: 4px;

  &.active{
    background: #3075ff;
    color:#fff;
  }
}
</style>
