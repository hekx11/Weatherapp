<template>
  <div id="app">
    <router-view @zalogowano="setLogin" />
  </div>
</template>
<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import LoginView from "@/views/LoginView.vue";

@Component({
  components: { LoginView },
})
export default class App extends Vue {
  data() {
    return {};
  }
  mounted() {
    if (!this.$store.state.zalogowano) {
      this.$router.replace({ name: "login" });
    }
  }
  setLogin(status: boolean) {
    this.$store.state.zalogowano = status;
  }
}
</script>

<style lang="scss">
@import "~@/assets/scss/vendors/bootstrap-vue/index";
@import "~@/assets/scss/vendors/bootstrap-vue/index";
@import url("https://fonts.googleapis.com/css2?family=Montserrat:wght@500&display=swap");

#app {
  font-family: "Montserrat", sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}
</style>
