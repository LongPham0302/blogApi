<template>
  <div class="ass1-login" v-if="currentUser">
    <div class="ass1-login__content">
      <p>Profile</p>

      <div class="ass1-login__form">
        <div class="avatar">
          <img v-bind:src="getAvatar" alt="" />
        </div>
        <form action="#" v-on:submit.prevent="handelEditProfile">
          <input
            v-bind:value="currentUser.fullname"
            v-on:input="fullname = $event.target.value"
            type="text"
            class="form-control"
            required=""
          />

          <select
            class="form-control"
            v-bind:value="currentUser.gender"
            v-on:change="gender = $event.target.value"
          >
            <option value="" disabled>Giới tính</option>
            <option value="nam">Nam</option>
            <option value="nu">Nữ</option>
          </select>
          <input
            v-on:change="uploadAvatar"
            type="file"
            name="avatar"
            placeholder="Ảnh đại
          diện"
            class="form-control"
          />
          <textarea
            v-bind:value="currentUser.description"
            v-on:input="description = $event.target.value"
            class="form-control"
            cols="30"
            rows="5"
            placeholder="Mô tả ngắn ..."
          ></textarea>
          <div class="ass1-login__send justify-content-center">
            <button type="submit" class="ass1-btn">Cập nhật</button>
          </div>
        </form>
      </div>
    </div>
  </div>
</template>
<script>
import { mapActions, mapGetters } from "vuex";
export default {
  name: "user-profile",
  data() {
    return {
      userid: this.$route.params.id,
      fullname: "",
      description: "",
      gender: "",
      avatar: { objFile: null, base64URL: "" },
    };
  },
  watch: {
    $route(to, from) {
      this.userid = to.params.id;
      this.checkIsCurrentUser();
    },
  },
  created() {
    this.checkIsCurrentUser();
  },
  computed: {
    ...mapGetters({ currentUser: "user/currentUser" }),
    getAvatar() {
      if (this.avatar.base64URL === "" && this.currentUser) {
        return this.currentUser.profilepicture;
      } else {
        return this.avatar.base64URL;
      }
    },
  },

  methods: {
    ...mapActions({ updateProfile: "user/updateProfile" }),
    checkIsCurrentUser() {
      if (this.userid && this.currentUser) {
        if (this.userid != this.currentUser.USERID) {
          this.$router.push("/");
        }
      }
      return false;
    },
    handelEditProfile() {
      if (!this.gender) this.gender = this.currentUser.gender;
      if (!this.fullname) this.fullname = this.currentUser.fullname;
      if (!this.description) this.description = this.currentUser.description;
      if (this.fullname && this.description && this.gender) {
        let data = {
          fullname: this.fullname,
          description: this.description,
          gender: this.gender,
        };

        if (this.avatar.objFile) {
          data.objFile = this.avatar.objFile;
        }

        this.updateProfile(data).then((res) => {
          if (res.ok) {
            alert("Update thông tin Profile thành công!");
          } else {
            alert(res.error);
          }
        });
      }
    },
    uploadAvatar(e) {
      if (e.target.files && e.target.files.length) {
        const fileAvatar = e.target.files[0];
        let reader = new FileReader();

        reader.addEventListener(
          "load",
          () => {
            let PreviewSrc = reader.result;
            this.avatar.base64URL = PreviewSrc;
            this.avatar.objFile = fileAvatar;
          },
          false
        );

        if (fileAvatar) {
          reader.readAsDataURL(fileAvatar);
        }
      }
    },
  },
};
</script>
<style scoped>
.avatar {
  border-radius: 50%;
  overflow: hidden;
}
</style>
