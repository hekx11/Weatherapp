<template>
  <b-container>
    <b-row class="justify-content-center vh-100" align-v="center">
      <b-col cols="3" class="pt-4">
        <b-card title="Login" bg-cyan class="p-4">
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
      </b-col></b-row
    >
  </b-container>
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
