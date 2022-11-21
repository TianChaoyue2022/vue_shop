<template>
    <div class="login_container">
       <div class="login_box">
        <!-- 登录头像 -->
        <div class="avatar_box">
            <img src="../assets/logo.png" alt="">
        </div>
        <!-- 登录表单区域 -->
        <el-form ref="loginFormRef" :model="loginForm" :rules="loginFormRules" label-width="0px" class="login_form">
            <el-form-item prop="username">
                <el-input v-model="loginForm.username" prefix-icon="el-icon-search"></el-input>
            </el-form-item>
            <el-form-item prop="password">
                <el-input v-model="loginForm.password" prefix-icon="el-icon-search" type="password"></el-input>
            </el-form-item>
            <el-form-item class="btns">
                <el-button type="primary" @click="login">登录</el-button>
                <el-button type="info" @click="resetLoginForm">重置</el-button>
            </el-form-item>
        </el-form>
    
       </div>
    </div>
</template>

<script>
export default {
    name: 'VueShopLogin',

    data() {
        return {
            loginForm:{
                username:'admin',
                password:'123456'
            },
            loginFormRules:{
                username:[ 
                    { required: true, message: '请输入用户名', trigger: 'blur' },
                    { min: 3, max: 10, message: '长度在 3 到 10 个字符', trigger: 'blur' }
                ],
                password:[
                    { required: true, message: '请输入密码', trigger: 'blur' },
                    { min: 6, max: 10, message: '长度在 6 到 10 个字符', trigger: 'blur' }
                ]
            }
           
            
        };
    },

    methods: {
        login(){
            this.$refs.loginFormRef.validate(async valid => {
                // console.log(valid);
               if(!valid)return;
                const {data:res} = await this.$http.post("login",this.loginForm);
                if(res.meta.status !== 200) return this.$message.error("登录失败");
                this.$message.success("登录成功");
              
                // 1.登录成功后的token保存到seesionStorage
                //     1.1 项目中除了登录意外的其他api接口，必须在登录之后才能访问
                //     1.2 token只应在当前网站打开期间生效，所以将token保存在sessionStorage中
                window.sessionStorage.setItem("token",res.data.token);
                // 2.通过编程时导航跳转到后台主页，路由地址是/home
                this.$router.push("/home");
                // $route和$router的区别
                //     1、$router是用来操作路由，$route是用来获取路由信息

                //     2、$router是VueRouter的一个实例，他包含了所有的路由，包括路由的跳转方法，钩子函数等，也包含一些子对象（例如history）

                //     3、$route是一个跳转的路由对象（路由信息对象），每一个路由都会有一个$route对象，是一个局部的对象。

            })
        },
        resetLoginForm(){
            this.$refs.loginFormRef.resetFields();
        }

        
    },
};
</script>

<style lang="scss" scoped>
.login_container{
    background-color: #2b4b6b;
    height: 100%;
}

.login_box{
    width: 450px;
    height: 300px;
    background-color: #fff;
    border_radius:3px;
    position:absolute;  
    left: 50%;
    top:50%;
    transform: translate(-50%,-50%);

    .avatar_box{
        height: 130px;
        width:130px;
        border:1px solid #eee;
        border-radius:50%;
        padding:10px;
        box-shadow:0 0 10px #ddd;
        position: absolute;
        left: 50%;
        transform:translate(-50%,-50%);
        background-color: #fff;
        img{
            height: 100%;
            width:100%;
            border-radius: 50%;
            background-color: #eee;
        }

    }
}

.btns{
    display: flex;
    justify-content: flex-end;
}

.login_form{
    position:absolute;
    bottom:0;
    width: 100%;
    padding:0 10px;
    box-sizing:border-box;
    

}

</style>