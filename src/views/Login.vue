<template>
    <div  class="login">
        <div class="logo">
          <img src="../assets/logo.jpg">
        </div>
        <!-- 手机号 -->
        <InputGroup
            type="number"
            v-model="phone"
            placeholder="手机号"
            :btnTitle="btnTitle"
            :disabled="disabled"
            :error="errors.phone"
            @btnClick="getVerifyCode"
        />
        <!-- 验证码 -->
        <InputGroup type="number" v-model="verifyCode" placeholder="验证码" :error="errors.code"/>

        <div class="login_desc">
            新用户登录即自动注册，并表示已同意<a href="javascript:;">《用户服务协议》</a>
        </div>

        <button class="submitBtn" :disabled="isClick"  @click="login">登录</button>
    </div>
</template>

<script>
    import  InputGroup  from "../components/InputGroup"
    export  default {
        name: "login",
        data() {
           return {
                phone: "",
                verifyCode: "",
                errors: {},
                btnTitle: "获取验证码",
                disabled: false
           }
        },
        computed: {
            isClick() {
                if (!this.phone || !this.verifyCode) return true;
                else return false;
            }
        },
        methods: {

            //点击登录
            login() {
                this.errors = {};
                this.$axios.post("/api/posts/sms_back", {
                     phone: this.phone,
                     code: this.verifyCode
                })
                .then(res => {
                    // console.log(res);
                    // 检验成功 设置登录状态并且跳转到/
                    localStorage.setItem("ele_login", true);
                    this.$router.push("/");
                 })
                 .catch(err => {
                     // 返回错误信息
                     this.errors = {
                         code: err.response.data.msg
                     }
                 });
            },

            getVerifyCode(){
               // console.log(111)
                if(this.validatePhone()){
                    this.validateBtn()
                    //发送网络请求
                    this.$axios.post("/api/posts/sms_send", {
                        tpl_id:"136729",
                        key:"795be723dd9e88c3ea98e2b6713ab795",
                        phone:this.phone
                    })
                    .then(res => {
                        console.log(res);
                    });
                }
            },

            //倒计时
            validateBtn (){
                let  time =60;
                let timer = setInterval (() => {
                    if(time == 0) {
                        clearInterval(timer);
                        this.btnTitle="获取验证码";
                        this.disabled =false;
                    }else {
                        this.btnTitle = time + '秒后重试';
                        this.disabled = true;
                        time--;
                    }
                },1000)
            },
            //验证手机号码
            validatePhone() {                
                if(!this.phone){
                    this.errors= {
                        phone: "手机号码不能为空"
                    };
                    return false;
                }else if(!/^1[3455678]\d{9}$/.test(this.phone)){
                     this.errors= {
                        phone: "请添加正确的手机号码"
                    };
                    return false;
                }else {
                    this.errors ={};
                    return true;
                }
            }
        },
        components: {
            InputGroup 
        }
    }
</script>

<style>
    .login{
        width: 100%;
        height: 100%;
        padding: 30px;
        box-sizing:border-box;
        background: #fff;
    }
    .logo{
        text-align: center;
    }
    .logo  img{
        width: 150px
    }
    .login_desc{
        margin-top: 12px;
        color: #999;
        font-size: 14px;
        line-height: 20px;
    }
    .login_desc a{
        color: #2395ff;
        text-decoration: none;
        -webkit-tap-highlight-color: transparent
    }
    .submitBtn{
        display: block;
        width: 100%;
        height: 42px;
        margin-top: 30px;
        border-radius: 4px;
        background: #4cd96f;
        color: #fff;
        cursor: pointer;
        text-align: center;
        font-size: 16px;
        line-height: 42px;
        outline: none;
        border: none;
        font-family: inherit;
    }
    .submitBtn[disabled]{
        opacity: .6;
        cursor: no-drop
    }
    .submitBtn:focus, .submitBtn:hover{
        background: #4cd964;
    }
</style>
