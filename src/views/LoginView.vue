<template>
  <b-row class="justify-content-center vh-100" align-v="center">
    <b-col cols="12" lg="3" md="6" class="pt-4">
      <b-card
        bg-cyan
        class="p-4 text-light"
        align="center"
        header-bg-variant="secondary"
        border-variant="secondary"
        header-border-variant="secondary"
        bg-variant="success"
      >
        <template #header>
          <h4 class="mb-0">Zaloguj</h4>
        </template>
        <b-form-input
          type="text"
          placeholder="Login"
          name="username"
          v-model="username"
        ></b-form-input>
        <b-form-input
          type="password"
          placeholder="Haslo"
          name="password"
          v-model="password"
        ></b-form-input>
        <b-button class="mt-3" size="md" @click="login()">Zaloguj</b-button>
      </b-card>
    </b-col>
  </b-row>
</template>
<script lang="ts">
import { Component, Vue, Prop } from "vue-property-decorator";

@Component
export default class LoginView extends Vue {
  data() {
    return {
      username: "",
      password: "",
    };
  }
  login(): void {
    console.log(this.$store.state.parentUsername);
    if (
      this.$data.username == this.$store.state.parentUsername &&
      this.$data.password == this.$store.state.parentPassword
    ) {
      this.$emit("zalogowano", true);
      this.$router.replace({ name: "home" });
    } else {
      console.log("Login lub haslo nieprawidlowe");
    }
  }
}
</script>
<style scoped lang="scss">
@import "@/assets/scss/vendors/bootstrap-vue/_custom.scss";
@import "~bootstrap/scss/bootstrap.scss";
@import "~bootstrap-vue/dist/bootstrap-vue.css";

h2 {
  text-shadow: 2px 2px 5px rgba(0, 0, 0, 0.5);
}
</style>
