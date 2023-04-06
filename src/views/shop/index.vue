<template>
  
  <div class="app-container">

    

    <el-button type="primary" style="width:12%;margin-bottom:30px;"
        @click.native.prevent="submit(1*unit_price)">
          充值1次 ({{ 1*unit_price }}元)</el-button>
    <el-button type="primary" style="width:12%;margin-bottom:30px;" 
        @click.native.prevent="submit(5*unit_price)">
          充值5次 ({{ 5*unit_price }}元)</el-button>
    <el-button type="primary" style="width:12%;margin-bottom:30px;" 
        @click.native.prevent="submit(10*unit_price)">
          充值10次 ({{ 10*unit_price }}元)</el-button>

    <div class="app-text1">注意：付款后请等待页面自动跳转，不要关闭页面</div>

  </div>
</template>

<script>
import { pay } from "@/api/shop.js";

export default {
  data() {
    
    return {
      unit_price:2,
      form:{

        //订单详情
        subject: '商品支付',
        //商品信息
        body: '支付宝支付',
        //订单金额
        total_amount: '',
      },

    };
  },
  
  methods: {
    submit(money) {
      this.form.total_amount=money;
      pay(this.form).then((resp) => {
        // 添加之前先删除一下，如果单页面，页面不刷新，添加进去的内容会一直保留在页面中，二次调用form表单会出错
  
        // const divForm = document.getElementsByTagName("div");
        // if (divForm.length) {
        //   document.body.removeChild(divForm[0]);
        // }
        // const div = document.createElement("div");
        // div.innerHTML = resp.data.form; // data就是接口返回的form 表单字符串
        // document.body.appendChild(div);
        // document.forms[0].setAttribute("target", "_blank"); // 新开窗口跳转
        // document.forms[0].submit();

        
        document.write(resp.data.form)
      });

    },
  },
};
</script>

<style lang="scss" scoped>
.app {
  &-container {
    margin: 20px;
    padding: 20px 35px 0;
  }
  &-text {
    font-size: 20px;
    line-height: 46px;
  }
  &-text1 {
    font-size: 20px;
    color: #f43905;
  }
}
</style>