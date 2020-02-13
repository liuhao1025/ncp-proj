<template>
    <div class="register">
        <!-- 注册表单 -->
        <el-form ref="dataForm" :data="formData">
            <el-form-item prop="phoneNumber">
                <el-input type="text" placeholder="手机号码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input type="password" placeholder="密码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input type="password" placeholder="重复密码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input type="text" placeholder="验证码"></el-input>
            </el-form-item>
            <el-form-item>
                <el-input type="text" placeholder="手机验证码"></el-input>
            </el-form-item>
        </el-form>
        <div class="register-form"></div>
    </div>
</template>
<script>
const MAX_COUNT_SECOND = 60
const LAST_COUNTDOWN_START_TIME_KEY = 'last_countdown_start_time'

export default {
    data () {
        return {
            formData: {
                // 提交数据
            }
        }
    },
    methods: {
        // 获取图片验证码
        getImgCode () {},
        // 获取短信验证码
        // TODO 增加延迟限制
        // TODO 刷新后要保持延迟限制（本地保存时间戳）
        getSMSCode () {
            const {
                phoneNumber,
                imgCode
            } = this.formData

            getSMSCode({
                phoneNumber,
                imgCode
            })
            .then(({ data }) => {
                if (data.code === 0) {
                    // 进入倒计时
                    this.startCountdown()
                } else {
                    this.$message.error(data.message)
                }
            })
            this.$http({
                url: this.$http.adornUrl('/getSMSCode'),
                data: this.$http.adornParams({
                    
                })
            })
        },
        startCountdown (remainSecond = MAX_COUNT_SECOND) {
            this.countdown = remainSecond
            this.countdownInterval && clearInterval(this.countdownInterval)
            const update = () => {
                this.countdownInterval = setInterval(() => {
                    if (this.countdown > 1) {
                        this.countdown = this,countdown - 1
                        update()
                    } else {
                        this.endCountdown()
                    }
                }, 1000)
            }
        },
        endCountdown () {
            this.countdownInterval && clearInterval(this.countdownInterval)
            this.countdown = 0
            localStorage.setItem(LAST_COUNTDOWN_START_TIME_KEY, null)
        },
        resetCountdown () {
            // 检查是否存在未结束的倒计时
            const lastCountdownStartTime = Number(localStorage.getItem(LAST_COUNTDOWN_START_TIME_KEY))
            if (!isNaN(lastCountdownStartTime) && lastCountdownStartTime) {
                this.startCountdown(Math.ceil((now - lastCountdownStartTime / 1000)))
            }
        },
        // 提交注册
        dataSubmit () {}
    },
    // 检查是否有未结束的倒计时
    activated () {
        this.resetCountdown()
    }
}
</script>