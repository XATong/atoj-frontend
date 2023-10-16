<template>
  <a-row id="globalHader" align="center" :wrap="false">
    <a-col flex="auto">
      <a-menu
        mode="horizontal"
        :selected-keys="selectedKeys"
        @menu-item-click="doMenuClick"
      >
        <a-menu-item
          key="0"
          :style="{ padding: 0, marginRight: '38px' }"
          disabled
        >
          <div class="title-bar">
            <img class="logo" src="../assets/R-C.png" />
            <div class="title">领航 OJ</div>
          </div>
        </a-menu-item>
        <a-menu-item v-for="item in visibleRoutes" :key="item.path">
          {{ item.name }}
        </a-menu-item>
      </a-menu>
    </a-col>
    <a-col flex="100px">
      <!-- 用户头像展示， 补充个人信息、登出、注册、登录功能-->
      <div class="userAvatar">
        <a-dropdown trigger="hover">
          <a-avatar shape="circle">
            <template
              v-if="loginUser && loginUser.userRole !== ACCESS_ENUM.NOT_LOGIN"
            >
              <template v-if="loginUser.userAvatar">
                <img
                  class="avatarImg"
                  alt="avatar"
                  :src="loginUser.userAvatar"
                />
              </template>
              <template v-else>
                <a-avatar>
                  <IconUser />
                </a-avatar>
              </template>
            </template>
            <template v-else>
              <a-avatar>未登录</a-avatar>
            </template>
          </a-avatar>
          <template #content>
            <template
              v-if="loginUser && loginUser.userRole !== ACCESS_ENUM.NOT_LOGIN"
            >
              <a-doption>
                <template #icon>
                  <icon-idcard />
                </template>
                <template #default>
                  <a-anchor-link
                    @click="
                      router.push({
                        path: '/about',
                      })
                    "
                    >个人信息
                  </a-anchor-link>
                </template>
              </a-doption>
              <a-doption>
                <template #icon>
                  <icon-poweroff />
                </template>
                <template #default>
                  <a-anchor-link @click="logout">退出登录</a-anchor-link>
                </template>
              </a-doption>
            </template>
            <template v-else>
              <a-doption>
                <template #icon>
                  <icon-user />
                </template>
                <template #default>
                  <a-anchor-link
                    @click="
                      router.push({
                        path: '/user/login',
                      })
                    "
                    >用户登录
                  </a-anchor-link>
                </template>
              </a-doption>
              <a-doption>
                <template #icon>
                  <icon-user-add />
                </template>
                <template #default>
                  <a-anchor-link
                    @click="
                      router.push({
                        path: '/user/register',
                      })
                    "
                    >用户注册
                  </a-anchor-link>
                </template>
              </a-doption>
            </template>
          </template>
        </a-dropdown>
      </div>
    </a-col>
  </a-row>
</template>

<script setup lang="ts">
import { computed, ref } from "vue";
import { useRouter } from "vue-router";
import { useStore } from "vuex";
import { routes } from "../router/routes";
import checkAccess from "@/acccess/checkAccess";
import ACCESS_ENUM from "@/acccess/accessEnum";
import { LoginUserVO, UserControllerService } from "../../generated";

const router = useRouter();
const store = useStore();

const loginUser: LoginUserVO = computed(
  () => store.state.user?.loginUser
) as LoginUserVO;

const logout = () => {
  UserControllerService.userLogoutUsingPost();
  location.reload();
};

// 展示在菜单的路由数组
const visibleRoutes = computed(() => {
  return routes.filter((item, index) => {
    if (item.meta?.hideInMenu) {
      return false;
    }
    // 根据权限过滤菜单
    if (
      !checkAccess(store.state.user.loginUser, item?.meta?.access as string)
    ) {
      return false;
    }
    return true;
  });
});

// 默认主页
const selectedKeys = ref(["/"]);

// 路由跳转后，更新选中的菜单项
router.afterEach((to, from, failure) => {
  selectedKeys.value = [to.path];
});

const doMenuClick = (key: string) => {
  router.push({
    path: key,
  });
};

// setTimeout(() => {
//   store.dispatch("user/getLoginUser", {
//     userName: "阿通",
//     userRole: ACCESS_ENUM.ADMIN,
//   });
// }, 3000);
</script>

<style scoped>
.title-bar {
  display: flex;
  align-items: center;
}

.title {
  color: #444;
  margin-left: 16px;
}

.logo {
  height: 48px;
}
</style>
