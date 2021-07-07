<template>
  <div class="order-box">
    <div>
      <div class="status"><span>進行中</span></div>
      <div v-for="(item,index) in processOrder" :key="index" class="order">
          <img :src="item.logo" alt="">
          <div class="order-text">
            <div class="order-text-process"> 
              <span class="process-status">{{item.status.type}}</span>
              <span>預計出貨：{{item.date}}</span>
            </div>
            <div class="order-name">
              {{item.name}}
            </div>
          </div>
          <img src="../assets/right.svg" class="arrow-right-icon">
      </div>
    </div>
    <div>
      <div class="status">
        <span>已完成</span>
      </div>
      <div v-for="(item,index) in successOrder" :key="index" class="order">
          <img :src="item.logo" alt="" class="success-order-img">
          <div class="order-text">
            <div class="order-status-success">
              <span>{{item.status.type}}</span>
            </div>
            <div class="order-name">
              {{item.name}}
            </div>
          </div>
          <img src="../assets/right.svg" class="arrow-right-icon">
      </div>
    </div>
  </div>
</template>

<script>
import api from '../data.js';

export default {
  data() {
    return {
    orderData:[],
    successOrder:[],
    processOrder:[],
    }
  },

  created() {
    const vm = this
    vm.getApi()
  },
  methods:{
    getApi() {
      const vm = this
      vm.apiData = api.orders
      vm.apiData = this.apiData.sort(function (a,b) {
        return new Date(b.date) - new Date(a.date);
      })
      vm.apiData.forEach(e=> {
        if(e.status.code <= 2) {
          vm.processOrder.push(e)
        } else {
          vm.successOrder.push(e)
        }
      })
      console.log(vm.successOrder , vm.processOrder);
    }
  }
}
</script>

<style  lang="scss" scoped>
.order-box {
  border-bottom: 1px solid #aaa;
  width: 300px;
  .status {
    position: relative;
    border: 1px solid #aaa;
    border-bottom:none;
    height: 30px;
    background: rgb(243, 241, 241);
    padding: 0 30px;
    display: flex;
    justify-content: flex-start;
    align-items: center;
    font-weight: bold ;
    font-size: 12px;

    span::before {
    text-align: start;
    position: absolute;
    top: 50%;
    left: 7%;
    width: 5px;
    height: 17px;
    background: rgb(26, 148, 26);
    display: inline-block;
    vertical-align: middle;
    content: '';
    transform: translateX(-50%) translateY(-50%);
    }
  }
  .order {
    height: 80px;
    padding: 10px 15px;
    border: 1px solid #aaa;
    border-bottom:none;
    display: flex;
    align-items: center;
    justify-content: flex-start;

    img {
      width: 50px;
      height: 50px;
    }

    .arrow-right-icon {
      width: 12.5px;
      height: 12.5px;
    }

    .success-order-img {
      filter:grayscale(100%);
    }
    .order-text {
      font-size: 14px;
      text-align: start;
      width: 75%;
      margin-left:15px;
      .order-status-success {
        margin-bottom:5px ;
      }
      .order-text-process {
        display: flex;
        justify-content: space-between;
        margin-bottom:5px ;
        .process-status {
          color: rgb(59, 161, 59);
        }
      }
      .order-name {
        padding-right: 10px;
      }
    }
  }
  
}
</style>
