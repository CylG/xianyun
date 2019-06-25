<template>
    <el-form 
        :model="form" 
        ref="form"
        :rules="rules" 
        class="form">

        <!-- 新增了prop属性 -->
        <el-form-item class="form-item" prop="userphone">
            <el-input 
                    placeholder="用户名手机"
                    v-model="form.userphone">
            </el-input>
        </el-form-item>
        <!-- 新增了prop属性 -->
        <el-form-item class="form-item" prop="num">
            <el-input 
                    placeholder="验证码"
                    v-model="form.num">
                    <template slot="append">
                        <el-button @click="handleSendNum">
                            发送验证码
                        </el-button>
                    </template>
            </el-input>
        </el-form-item>
        <!-- 新增了prop属性 -->
        <el-form-item class="form-item" prop="username">
            <el-input 
                    placeholder="你的名字"
                    v-model="form.username">
            </el-input>
        </el-form-item>
        <!-- 新增了prop属性 -->
        <el-form-item class="form-item" prop="password">
            <el-input 
                    placeholder="密码" 
                    type="password"
                    v-model="form.num">
            </el-input>
        </el-form-item>
        <!-- 新增了prop属性 -->
        <el-form-item class="form-item" prop="confirmpassword">
            <el-input 
                    placeholder="确认密码" 
                    type="password"
                    v-model="form.confirmpassword">
            </el-input>
        </el-form-item>

        <el-button 
        class="submit"
        type="primary"
        @click="handleRegisterSubmit">
            注册
        </el-button>
    </el-form>

</template>

<script>
export default {
        data(){
             // 确认密码
            const validatePass = (rule, value, callback) => {
                if (value === '') {
                    callback(new Error('请再次输入密码'));
                } else if (value !== this.form.password) {
                    callback(new Error('两次输入密码不一致!'));
                } else {
                    callback();
                }
            }
            return {
                // 表单数据
                form: {
                    userphone: "",          // 注册用户名手机
                    num: "",                // 注册验证码
                    username: "",           // 注册用户名字
                    password: "",           // 注册密码
                    confirmpassword: ""     // 注册再次确认密码
                },
                // 表单规则
                rules: {
                    userphone: [
                        { 
                            required: true, 
                            message: '请输入用户名手机', 
                            trigger: 'blur' 
                        },
                    ],
                    num: [
                        { 
                            required: true, 
                            message: '请输入验证码', 
                            trigger: 'blur' 
                        },
                    ],
                    username: [
                        { 
                            required: true, 
                            message: '请输入名字', 
                            trigger: 'blur' 
                        },
                    ],
                    password: [
                        { 
                            required: true, 
                            message: '请输入密码', 
                            trigger: 'blur' 
                        },
                    ],
                    confirmpassword: [
                        { 
                            required: true, 
                            message: '请再次输入密码', 
                            trigger: 'blur' 
                        },
                    ],
                },
            }
        },
    // 提交注册
    methods: {
        // 发送验证码
        handleSendNum(){
            if(!this.form.username){
                this.$confirm('手机号码不能为空', '提示', {
                    confirmButtonText: '确定',
                    showCancelButton: false,
                    type: 'warning'
                })
                return;
            }
            if(this.form.username.length !== 11){
                this.$confirm('手机号码格式错误', '提示', {
                    confirmButtonText: '确定',
                    showCancelButton: false,
                    type: 'warning'
                })
                return;
            }
            this.$axios({
                url: `captchas`,
                method: "POST",
                data: {
                    tel: this.form.username
                }
            }).then(res => {
                const {code} = res.data;
                this.$confirm(`模拟手机验证码为：${code}`, '提示', {
                    confirmButtonText: '确定',
                    showCancelButton: false,
                    type: 'warning'
                })
            })
        },

        handleRegisterSubmit(){
            // 验证表单
            this.$refs['form'].validate((valid) => {
                // 为true表示没有错误
                if (valid) {
                    // 注册提交
                    const {confirmpassword, ...props} = this.regForm;
                    this.$axios({
                        url: "/accounts/register",
                        method: "POST",
                        data: props
                    }).then(res => {
                        console.log(res.data);
                    })
                }
            })
        }
    }
}
</script>

<style scoped lang="less">
    .form{
        padding:25px;
    }

    .form-item{
        margin-bottom:20px;
    }

    .form-text{
        font-size:12px;
        color:#409EFF;
        text-align: right;
        line-height: 1;
    }

    .submit{
        width:100%;
        margin-top:10px;
    }
</style>