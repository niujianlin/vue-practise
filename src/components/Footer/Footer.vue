<template>
  <div class="footer-container">
    <!-- 左侧的全选 -->
    <div class="custom-control custom-checkbox">
      <input type="checkbox" class="custom-control-input" id="cbFull" :checked="isfull" @change="fullChange" />
      <label class="custom-control-label" for="cbFull" >全选</label>
    </div>

    <!-- 中间的合计 -->
    <div>
      <span>合计：</span>
      <span class="total-price">￥{{ totalprice.toFixed(2) }}</span>
    </div>

    <!-- 结算按钮 -->
    <button type="button" class="btn btn-primary btn-settle">结算（{{ totalnum }}）</button>
  </div>
</template>

<script>
export default {
  props: {
    // 从父组件接收是否全选
    isfull: {
      type: Boolean,
      default: true
    },
    // 从父组件接收总价格
    totalprice: {
      type: Number,
      default: 0
    },
    // 从父组件接收商品总个数
    totalnum: {
      type: Number,
      default: 0
    }
  },
  methods: {
    fullChange(e) {
      console.log(e)
      const val = e.target.checked
      this.$emit('footFullChange', {value: val})
    },
  },
}
</script>

<style lang="less" scoped>
.footer-container {
  font-size: 12px;
  height: 50px;
  width: 100%;
  border-top: 1px solid #efefef;
  position: fixed;
  bottom: 0;
  background-color: #fff;
  display: flex;
  justify-content: space-between;
  align-items: center;
  padding: 0 10px;
}

.custom-checkbox {
  display: flex;
  align-items: center;
}

#cbFull {
  margin-right: 5px;
}

.btn-settle {
  height: 80%;
  min-width: 110px;
  border-radius: 25px;
  font-size: 12px;
}

.total-price {
  font-weight: bold;
  font-size: 14px;
  color: red;
}
</style>
