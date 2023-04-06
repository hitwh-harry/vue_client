<template>
    <div class="img-container">
        <h3> 如遇问题，请添加客服咨询 </h3>
          <img :src="imgUrl" style="width: 20% ;height: 300px"  />
        </div>
  </template>
  <script>
  import imgPath from "@/assets/a.jpg"  //先导入
  export default {
    data() {
      return {
      //   imgUrl: require('@/assets/logo.png'),   方法二 利用require直接写
        imgUrl: imgPath,  //再引用 
      };
    },
    methods: {},
  };
  </script>

<style lang="scss" scoped>
.img {
  &-container {
    margin: 40px;
  }
}
</style>
