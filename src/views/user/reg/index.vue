<template>
  <div id="reg">
    <div class="reg-form__wrap">
      <h1>注册</h1>
      <el-form size="medium" :model="regForm" ref="regForm" :rules="regFormRules" label-width="65px">
        <el-form-item label="手机号" prop="phone">
          <el-input v-model="regForm.phone" type="text" autofocus clearable auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="密码" prop="password">
          <el-input v-model="regForm.password" type="password" clearable auto-complete="off"></el-input>
        </el-form-item>
        <el-form-item label="昵称" prop="nickname">
          <el-input v-model="regForm.nickname" type="text" autofocus clearable auto-complete="off"></el-input>
        </el-form-item>
      </el-form>
      <div class="reg-form__actions">
        <el-button type="primary" @click="submit">立即注册</el-button>
        <el-button @click="goLogin">登录已有账号</el-button>
      </div>
    </div>
  </div>
</template>

<script>
import { regexs } from '@/util'
import { mapState, mapActions } from 'vuex'

export default {
  data () {
    return {
      regForm: null,
      regFormRules: {
        phone: [
          {required: true, message: '请输入手机号码'},
          {pattern: regexs.phone, message: '手机号码格式有误，请检查'}
        ],
        password: [
          {required: true, message: '请输入密码'},
          {pattern: regexs.password, message: '请输入6-18位字母数字组合 (第一位必须为字母)'}
        ],
        nickname: [
          {required: true, message: '请输入昵称'},
          {pattern: regexs.nickname, message: '请输入2-10位的中英文组合 (不允许特殊符号)'}
        ]
      }
    }
  },
  computed: {
    ...mapState('user', ['defaultRegForm']),
    ...mapActions('user', ['submitRegForm'])
  },
  methods: {
    // 提交注册表单
    async submit () {
      this.$refs.regForm.validate(async valid => {
        if (valid) {
          this.$store.dispatch('user/submitRegForm', this.regForm)
        }
      })
    },
    // 跳转到登录页面
    goLogin () {
      this.$router.push('/user/login')
    }
  },
  created () {
    // 每次创建实例时清空表单
    this.regForm = Object.assign({}, this.defaultRegForm)
  }
}
</script>

<style lang="scss" scoped>
@import './style.scss';
</style>
