<template>
    <div class="login">
        <img class="left" src="../assets/img/login_back.jpg" alt="left poster">
        <div class="box">
            <div class="logo-container">
                <img class="logo" src="../assets/img/logo.png" alt="left poster">
                <span class="logo-text">内部餐饮系统</span>
            </div>
            <div style="display: flex; text-align: center;">
                <p class="title">登录</p>
                <span class="regPart">没有账号？
                    <el-link :underline="false" type="primary" href="#register">前往注册</el-link>
                </span>
            </div>
            <el-form ref="ruleFormRef" :model="form" :rules="rules" label-width="20%" class="demo-form" status-icon>
                <el-form-item label="登录账号" prop="username">
                    <el-input v-model="form.username" />
                </el-form-item>
                <el-form-item label="登录密码" prop="password" style="margin-bottom: 2%;">
                    <el-input type="password" v-model="form.password" />
                </el-form-item>
                <el-form-item style="margin-bottom: 1%;">
                    <el-link style="margin-left: 82%;" :underline="false" type="danger" href="#/resetPass">忘记密码</el-link>
                </el-form-item>
                <el-form-item>
                    <el-button type="success" @click="onSubmit(ruleFormRef)">
                        登录
                    </el-button>
                    <el-button @click="onReset(ruleFormRef)">重置</el-button>
                </el-form-item>
            </el-form>
            <div class="info">
                <p>Copyright © 2024 lains. <span class="contact">Contact me ：<el-link :underline="false" type="success"
                            href="https://github.com/lainso" target="_blank">Github</el-link></span>
                </p>
            </div>
        </div>
    </div>
</template>

<script setup>
import { reactive, ref } from 'vue'
import { $Login } from '@/api/customer';
import { useUserStore } from '@/stores/userStore'
const userStore = useUserStore()
const ruleFormRef = ref()

const form = reactive({
    username: '',
    password: '',
})

const rules = reactive({
    username: [
        { required: true, message: '登录账号不能为空', trigger: 'blur' },
        // { min: 3, max: 5, message: 'Length should be 3 to 5', trigger: 'blur' },
    ],
    password: [
        { required: true, message: '登录密码不能为空', trigger: 'blur' },
    ],
})

const onSubmit = (formEl) => {
    if (!formEl) return
    formEl.validate((valid) => {
        if (valid) {
            $Login(form.username, form.password)
                .then(function (data) {
                    userStore.setUsername(form.username)
                    console.log(data);
                })
                .catch(function (error) {
                    console.log(error);
                });
        } else {
            return false
        }
    })
}

const onReset = (formEl) => {
    if (!formEl) return
    formEl.resetFields()
}
</script>

<style scoped lang="scss">
.login {
    display: flex;
    align-items: center;
    height: 100vh;
    background-color: #c3cbd6;

    .left {
        width: 63%;
        height: 100vh;
        object-fit: cover;
    }

    .box {
        min-height: 755px;
        max-height: 100vh;
        width: 37%;
        padding: 4%;
        background: #f6f8fb;
        box-shadow: 0 0 10px 0 rgba(0, 0, 0, 0.1);
        border-radius: 4px;
        display: flex;
        justify-content: center;
        flex-direction: column;
        position: relative;

        .title {
            margin-bottom: 4%;
            padding-left: 21%;
            color: #303133;
            font-size: 3.5vh;
        }

        .regPart {
            margin-left: auto;
        }

        .logo-container {
            max-height: 20%;
            display: flex;
            align-items: center;
            margin-bottom: 5%;

            .logo {
                display: inline-block;
                max-height: 65px;
                border-radius: 50%;
            }

            .logo-text {
                display: inline-block;
                vertical-align: middle;
                margin-left: 5%;
                font-size: 5.5vh;
                font-family: Kai;
            }
        }

        .info {
            position: absolute;
            bottom: 1%;
            width: 80%;

            .contact {
                margin-left: auto;
            }
        }
    }
}

::v-deep .el-form-item__label {
    font-size: 16px;
}

::v-deep .el-form-item .el-input {
    height: 5.5vh;

    .el-input__inner {
        font-size: 15px;
    }
}

::v-deep .el-form-item {
    margin-bottom: 7%;
}

::v-deep .el-button {
    height: 5vh;
    width: 19vh;
}

.login .box span,
.login .box p {
    display: flex;
    align-items: center;
}
</style>