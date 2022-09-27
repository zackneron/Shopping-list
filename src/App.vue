<template>
  <v-app>
    <v-app-bar app color="white" dark>
      <div class="d-flex align-center">
        <v-img :src="require('@/assets/logo.svg')" />
      </div>
    </v-app-bar>

    <v-main>
      <Card />
    </v-main>
  </v-app>
</template>

<script>
import Card from "./components/Card";

export default {
  name: "App",

  components: {
    Card,
  },

  data: () => ({
    //
  }),
};
</script>
