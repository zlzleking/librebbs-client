<template lang="pug">
.page-auth {{ error }}
</template>
<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import { getToken } from "@/auth";
import store from "@/store";

@Component({ name: "Auth" })
export default class Auth extends Vue {
  error = "";

  async beforeMount(): Promise<void> {
    if (typeof this.$route.query.code !== "string") {
      this.error = "로그인 중 에러가 발생했습니다.";
      return;
    }
    try {
      await getToken(this.$route.query.code);
    } catch {
      this.error = "로그인 중 에러가 발생했습니다.";
      return;
    }
    store.dispatch("fetchUserInfo");
    if (typeof this.$route.query.state === "string") {
      this.$router.replace(this.$route.query.state);
    } else {
      this.$router.replace("/");
    }
  }
}
</script>
