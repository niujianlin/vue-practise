<template>
  <div class="app-container">
    <!-- Header 头部区域 -->
    <Header></Header>
    <!-- 循环渲染每一个商品的信息（父向子传值子用props，子向父传值子挂载方法传值父绑定@方法收值） -->
    <Goods v-for="item in list" :key="item.id" 
    :title="item.goods_name" :pic="item.goods_img" 
    :price="item.goods_price" :state="item.goods_state" 
    :id="item.id" @state-change="getNewState"
    :count="item.goods_count"></Goods>
    <!-- Footer底部区域 -->
    <!-- <p>{{ fullState }}</p> -->
    <!-- <p>{{ amt }}</p> -->
    <Footer :isfull="fullState" @footFullChange="getFullChange"
    :totalprice="amtprice" :totalnum="amtnum"></Footer>
  </div>
</template>

<script>
// 导入需要的组件
import Header from '@/components/Header/Header.vue'
import Goods from '@/components/Goods/Goods.vue'
import Footer from '@/components/Footer/Footer.vue'
// 导入跨父组件传值vue对象
import bus from '@/components/eventBus.js'
// 导入axios请求
import axios from 'axios'
// import e from 'express'

export default {
  data() {
    return {
      // 存储购物车列表数据
      list: [],

    }
  },
  // 计算属性，动态的，应用：购物车是否全选
  computed: {
    // 动态计算出是否全选，every()常用判断全部列表是否符合条件，合规的返回true
    fullState(){
      return this.list.every(item => item.goods_state === true)
    },
    // 计算勾选商品总价格 reduce作用是累加，返回total值。reduce((累加数，当前项)=>{计算过程},初始值)
    amtprice() {
      return this.list
      .filter(item => item.goods_state)
      .reduce((total, item) => {
        // console.log(total)
        return total += item.goods_price * item.goods_count
      }, 0)
    },
    // 计算勾选商品的个数
    amtnum() {
      return this.list.filter(item => item.goods_state)
      .reduce((total, item) => {
        return total += item.goods_count
      }, 0)
    }
  },
  created() {
    // 调用请求数据的方法
    this.initCartList()

    // 接收Counter中修改的cont商品数量（跨过父组件直接到祖父）
    // 他是个异步函数，但要求从开始就执行，所以需要先初始化声明好了，后面会触发事件
    bus.$on('share', val => {
      this.list.some(item => {
        if(item.id===val.id){
          item.goods_count = val.value
        }
      })
    })

  },
  methods: {
    //封装请求列表数据的方法
    async initCartList() {
      const {data:res} = await axios.get('https://www.escook.cn/api/cart')
      console.log(res)
      // 存数据
      if(res.status === 200){
        this.list = res.list
      }
    },
    // 接受子组件传递过来的数据
    getNewState(e) {
      console.log('子组件勾选状态触发改变，父组件接收到啦！！')
      // console.log(e)
      // some用作查找列表中的一项，找到就停止
      this.list.some(item => {
        if(item.id === e.id){
          item.goods_state = e.value
          return true
        }
      })
    },
    getFullChange(e){
      if(e.value === true){
        this.list.forEach(item => item.goods_state = true);
      }else{
        this.list.forEach(item => item.goods_state = false);
      }
    }
  },
  components: {
    Header,
    Goods,
    Footer
  }
}
</script>

<style lang="less" scoped>
.app-container {
  padding-top: 45px;
  padding-bottom: 50px;
}
</style>
