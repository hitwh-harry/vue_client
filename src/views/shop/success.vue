<template>
  
  <div class="app-container">
      <h1>付款成功！</h1>
    <el-button type="primary" style="width:10%;margin-bottom:30px;" @click.native.prevent="return2Menu">返回</el-button>
    
  </div>
</template>

<script>
import { paySuccess } from "@/api/shop.js";

export default {

  created() {
    this.success()
  },

  methods: {

    success(){
      let trade_no = this.$route.query.trade_no;
      let out_trade_no = this.$route.query.out_trade_no;
      paySuccess(out_trade_no,trade_no);
    },

    return2Menu() {
          this.$router.push("/");
    },

    
  },
};
</script>
