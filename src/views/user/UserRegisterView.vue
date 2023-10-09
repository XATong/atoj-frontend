<template>
  <div class="userRegisterView">
    <h2 style="margin-bottom: 16px">用户注册</h2>
    <a-form
      :model="form"
      @submit="handleSubmit"
      label-align="left"
      auto-label-width
      style="max-width: 480px; margin: 0 auto"
    >
      <a-form-item
        :rules="[
          { required: true, message: '账号不能为空' },
          { minLength: 4, message: '账号长度不能低于四位' },
        ]"
        field="userAccount"
        label="账号 :"
      >
        <a-input v-model="form.userAccount" placeholder="请输入账号" />
      </a-form-item>
      <a-form-item
        :rules="[
          { required: true, message: '密码不能为空' },
          { minLength: 8, message: '密码长度不能低于八位' },
        ]"
        field="userPassword"
        tooltip="密码长度不少于八位"
        label="密码 :"
      >
        <a-input-password
          v-model="form.userPassword"
          placeholder="请输入密码"
        />
      </a-form-item>
      <a-form-item
        :rules="[
          { required: true, message: '密码不能为空' },
          { minLength: 8, message: '密码长度不能低于八位' },
        ]"
        field="checkPassword"
        tooltip="密码长度不少于八位"
        label="重复密码 :"
      >
        <a-input-password
          v-model="form.checkPassword"
          placeholder="请再次输入密码"
        />
      </a-form-item>
      <a-form-item>
        <a-button
          type="primary"
          html-type="submit"
          style="width: 120px; margin: 0 auto"
        >
          注册
        </a-button>
      </a-form-item>
    </a-form>
  </div>
</template>
<script lang="ts" setup>
import { reactive } from "vue";
import { UserControllerService, UserRegisterRequest } from "../../../generated";
import { Message } from "@arco-design/web-vue";
import { useStore } from "vuex";
import { useRouter } from "vue-router";

const router = useRouter();
const store = useStore();
const form = reactive({
  userAccount: "",
  userPassword: "",
  checkPassword: "",
} as UserRegisterRequest);

const handleSubmit = async () => {
  if (
    (form.userAccount as string).length < 4 ||
    (form.userPassword as string).length < 6
  ) {
    return;
  }
  if (
    (form.checkPassword as string).length !==
      (form.userPassword as string).length ||
    form.checkPassword !== form.userPassword
  ) {
    Message.error("两次输入密码不一致");
    return;
  }
  const res = await UserControllerService.userRegisterUsingPost(form);
  if (res.code === 0) {
    Message.success("注册成功！");
    //await store.dispatch("user/getLoginUser");
    await router.push({
      path: "/user/login",
      replace: true,
    });
  } else {
    Message.error(res.msg);
  }
};
</script>
