<template>
    <div class="login-container">

    <el-dialog title="" :visible.sync="detailVisible" width="35%">
      两次密码输入不同
    </el-dialog>

    <el-dialog title="" :visible.sync="detailVisible1" width="35%">
      用户名已存在
    </el-dialog>
    

      <el-form ref="loginForm" :model="loginForm" :rules="loginRules" class="login-form" auto-complete="on" label-position="left">

  
        <el-form-item prop="username">
          <span class="svg-container">
            <svg-icon icon-class="user" />
          </span>
          <el-input
            ref="username"
            v-model="loginForm.username"
            placeholder="请输入用户名"
            name="username"
            type="text"
            tabindex="1"
            auto-complete="on"
          />
        </el-form-item>
  
        <el-form-item prop="password">
          <span class="svg-container">
            <svg-icon icon-class="password" />
          </span>
          <el-input
            :key="passwordType"
            ref="password"
            v-model="loginForm.password"
            :type="passwordType"
            placeholder="请输入密码"
            name="password"
            tabindex="2"
            auto-complete="on"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>

        <el-form-item prop="password1">
          <span class="svg-container">
            <svg-icon icon-class="password" />
          </span>
          <el-input
            :key="passwordType"
            ref="password"
            v-model="password"
            :type="passwordType"
            placeholder="密码确认"
            name="password"
            tabindex="3"
            auto-complete="on"
          />
          <span class="show-pwd" @click="showPwd">
            <svg-icon :icon-class="passwordType === 'password' ? 'eye' : 'eye-open'" />
          </span>
        </el-form-item>
  
        <el-button type="primary" style="width:30%;margin-left:60px;" @click.native.prevent="submit">确定</el-button>
        <el-button type="info" style="width:30%;margin-left:50px;" @click.native.prevent="back">返回</el-button>
  
  
      </el-form>
    </div>
  </template>
  
  <script>
  import { register } from "@/api/user.js";


  export default {
    name: 'Login',
    data() {
      
      const validateUsername = (rule, value, callback) => {
      if (value.length == 0) {
        callback(new Error('The username can not be empty'))
      } else {
        callback()
      }
    }
    const validatePassword = (rule, value, callback) => {
      if (value.length < 6) {
        callback(new Error('The password can not be less than 6 digits'))
      } else {
        callback()
      }
    }

      return {
        detailVisible:false,
        detailVisible1:false,

        loginForm: {
          username: '',
          password: ''
        },

        loginRules: {
          username: [{ required: true, trigger: 'blur', validator: validateUsername }],
          password: [{ required: true, trigger: 'blur', validator: validatePassword }]
        },

        password:'',
        loading: false,
        passwordType: 'password',
        redirect: undefined
      }
    },
    watch: {
      $route: {
        handler: function(route) {
          this.redirect = route.query && route.query.redirect
        },
        immediate: true
      }
    },
    methods: {
      showPwd() {
        if (this.passwordType === 'password') {
          this.passwordType = ''
        } else {
          this.passwordType = 'password'
        }
        this.$nextTick(() => {
          this.$refs.password.focus()
        })
      },
  
      back(){
        this.$router.push("/");
      },
  
      submit() {
        this.$refs.loginForm.validate(valid => {
        if (valid) {
          
          if(this.loginForm.password==this.password){
            register(this.loginForm.username.trim(),this.loginForm.password).then((response) => {
              if(response.data.success=='false'){
                this.detailVisible1=true;
              }
              else{
                this.$alert('注册成功', '', {
                  confirmButtonText: '确定',
                  callback: action => {
                    this.$router.push("/");
                  }
                });
                // this.$router.push("/");
              }

            });
          }
          else{
            this.detailVisible=true;
          }

        } else {
          console.log('error submit!!')
          return false
        }
      })
      }
    }
  }
  </script>
  
  <style lang="scss">
  /* 修复input 背景不协调 和光标变色 */
  /* Detail see https://github.com/PanJiaChen/vue-element-admin/pull/927 */
  
  $bg:#283443;
  $light_gray:#fff;
  $cursor: #fff;
  
  @supports (-webkit-mask: none) and (not (cater-color: $cursor)) {
    .login-container .el-input input {
      color: $cursor;
    }
  }
  
  /* reset element-ui css */
  .login-container {
    .el-input {
      display: inline-block;
      height: 47px;
      width: 85%;
  
      input {
        background: transparent;
        border: 0px;
        -webkit-appearance: none;
        border-radius: 0px;
        padding: 12px 5px 12px 15px;
        color: $light_gray;
        height: 47px;
        caret-color: $cursor;
  
        &:-webkit-autofill {
          box-shadow: 0 0 0px 1000px $bg inset !important;
          -webkit-text-fill-color: $cursor !important;
        }
      }
    }
  
    .el-form-item {
      border: 1px solid rgba(255, 255, 255, 0.1);
      background: rgba(0, 0, 0, 0.1);
      border-radius: 5px;
      color: #454545;
    }
  }
  </style>
  
  <style lang="scss" scoped>
  $bg:#2d3a4b;
  $dark_gray:#889aa4;
  $light_gray:#eee;
  
  .login-container {
    min-height: 100%;
    width: 100%;
    background-color: $bg;
    overflow: hidden;
  
    .login-form {
      position: relative;
      width: 520px;
      max-width: 100%;
      padding: 160px 35px 0;
      margin: 0 auto;
      overflow: hidden;
    }
  
    .tips {
      font-size: 14px;
      color: #fff;
      margin-bottom: 10px;
  
      span {
        &:first-of-type {
          margin-right: 16px;
        }
      }
    }
  
    .svg-container {
      padding: 6px 5px 6px 15px;
      color: $dark_gray;
      vertical-align: middle;
      width: 30px;
      display: inline-block;
    }
  
    .title-container {
      position: relative;
  
      .title {
        font-size: 26px;
        color: $light_gray;
        margin: 0px auto 40px auto;
        text-align: center;
        font-weight: bold;
      }
    }
  
    .show-pwd {
      position: absolute;
      right: 10px;
      top: 7px;
      font-size: 16px;
      color: $dark_gray;
      cursor: pointer;
      user-select: none;
    }
  }
  </style>
  