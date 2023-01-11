<template>
  <div class="number-container d-flex justify-content-center align-items-center">
    <!-- 减 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="sub">-</button>
    <!-- 购买的数量 -->
    <span class="number-box">{{cnt}}</span>
    <!-- 加 1 的按钮 -->
    <button type="button" class="btn btn-light btn-sm" @click="add">+</button>
  </div>
</template>

<script>
// 导入跨父组件传值vue对象
import bus from '@/components/eventBus.js'

export default {
  props: {
    id: {
      type: Number,
      required: true
    },
    cnt: {
      default: 0,
      type: Number
    }
  },
  methods: {
    // 点击按钮，数值加一
    add() {
      const obj = {id:this.id, value:this.cnt+1}
      bus.$emit('share', obj)
    },
    sub() {
      if(this.cnt > 1){
        const obj = {id:this.id, value:this.cnt-1}
        bus.$emit('share', obj)
      }
    }
  }

}
</script>

<style lang="less" scoped>
.number-box {
  min-width: 30px;
  text-align: center;
  margin: 0 5px;
  font-size: 12px;
}

.btn-sm {
  width: 30px;
}
</style>
