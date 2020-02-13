<template>
    <div class="login">
        <el-form class="login-form">
            <el-form-item>
                <el-input type="password" placeholder="输入手机号" :maxlength="16"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input type="password" placeholder="密码" :maxlength="16"></el-input>
            </el-form-item>
        </el-form>
        <el-button>登录</el-button>
        <div>
            <el-button type="text">忘记密码</el-button>
            <el-button type="text">注册账号</el-button>
        </div>
    </div>
</template>