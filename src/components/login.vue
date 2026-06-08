<template>
  <div class="login-wrapper">
    <div class="login-card">
      <h2 class="login-title">系统登录</h2>
      <a-form
        :model="formState"
        name="basic"
        :label-col="{ span: 6 }"
        :wrapper-col="{ span: 18 }"
        autocomplete="off"
        @finish="onFinish"
        @finishFailed="onFinishFailed"
      >
        <a-form-item
          label="用户名"
          name="username"
          :rules="[{ required: true, message: '请输入用户名!' }]"
        >
          <a-input v-model:value="formState.username" placeholder="请输入用户名" />
        </a-form-item>

        <a-form-item
          label="密码"
          name="password"
          :rules="[{ required: true, message: '请输入密码!' }]"
        >
          <a-input-password v-model:value="formState.password" placeholder="请输入密码" />
        </a-form-item>

        <a-form-item name="remember" :wrapper-col="{ offset: 6, span: 18 }">
          <a-checkbox v-model:checked="formState.remember">记住密码</a-checkbox>
        </a-form-item>

        <a-form-item :wrapper-col="{ offset: 6, span: 18 }">
          <a-button type="primary" html-type="submit" block>登录</a-button>
        </a-form-item>
      </a-form>
    </div>
  </div>
</template>
<script lang="ts" setup>

import { useRouter } from 'vue-router';
import { reactive } from 'vue';
const router = useRouter();


interface FormState {
  username: string;
  password: string;
  remember: boolean;
}

const formState = reactive<FormState>({
  username: '',
  password: '',
  remember: true,
});
const onFinish = (values: any) => {
  console.log('Success:', values);
  // 登录成功后，跳转到首页
  router.push('/home');
  // 登录成功后，将用户名存储到本地存储
  localStorage.setItem('username', values.username);
};

const onFinishFailed = (errorInfo: any) => {
  console.log('Failed:', errorInfo);
};
</script>

<style scoped lang="less">
.login-wrapper {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 100vh;
  background: #f5f5f5;
}

.login-card {
  width: 380px;
  padding: 32px;
  background: #fff;
  border-radius: 6px;
  box-shadow: 0 2px 12px rgba(0, 0, 0, 0.06);
}

.login-title {
  text-align: center;
  margin-bottom: 28px;
  font-size: 20px;
  font-weight: 500;
  color: #333;
}
</style>
