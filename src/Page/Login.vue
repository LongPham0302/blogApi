<template>
  <div class="ass1-login">
    <div class="ass1-login__logo">
      <router-link to="/" class="ass1-logo">ZendVn Meme</router-link>
    </div>
    <div class="ass1-login__content">
      <p>Đăng nhập</p>
      <div class="ass1-login__form">
        <form v-on:submit.prevent="handelSumitLogin">
          <input
            v-model="email"
            type="text"
            class="form-control"
            placeholder="Email"
            required=""
          />
          <div class="ass1-input-copy">
            <input
              v-model="password"
              type="password"
              class="form-control"
              placeholder="Mật khẩu"
              required=""
            />
            <a href="#">Copy</a>
          </div>
          <div class="ass1-login__send">
            <router-link to="/register">Đăng ký một tài khoản</router-link>
            <button type="submit" class="ass1-btn">Đăng nhập</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { mapActions } from "vuex";
export default {
  name: "login",
  data() {
    return {
      email: "",
      password: "",
    };
  },
  methods: {
    ...mapActions({ login: "user/login" }),
    handelSumitLogin(e) {
      let data = {
        email: this.email,
        password: this.password,
      };
      console.log("data", data);
      this.login(data).then((res) => {
        if (!res.ok) {
          if (typeof res.error === "string") {
            alert(res.error);
          } else {
            alert(res.error.join(" "));
          }
        } else {
          this.$router.push("/");
        }
      });
    },
  },
};
</script>
