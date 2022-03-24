<template>
  <div id="app">
    <div id="nav">
      <router-link
        v-if="zalogowano"
        to="/login"
        v-on:click.native="logout()"
        replace
        >Logout</router-link
      >
    </div>
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
    return {
      zalogowano: false,
    };
  }
  mounted() {
    if (!this.$data.zalogowano) {
      this.$router.replace({ name: "login" });
    }
  }
  setLogin(status: boolean) {
    this.$data.zalogowano = status;
  }
  logout() {
    this.$data.zalogowano = false;
  }
}
</script>

<style lang="scss">
@import "~@/assets/scss/vendors/bootstrap-vue/index";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}

nav {
  padding: 30px;

  a {
    font-weight: bold;
    color: #2c3e50;

    &.router-link-exact-active {
      color: #42b983;
    }
  }
}
</style>
