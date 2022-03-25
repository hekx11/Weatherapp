<template>
  <div>
    <NavBar />
    <b-container>
      <b-row>
        <b-col>
          <h1>Home</h1>
        </b-col>
      </b-row>
    </b-container>
  </div>
</template>

<script lang="ts">
import { Component, Vue } from "vue-property-decorator";
import NavBar from "@/components/NavBar.vue";

@Component({ components: { NavBar } })
export default class HomeView extends Vue {}
</script>
