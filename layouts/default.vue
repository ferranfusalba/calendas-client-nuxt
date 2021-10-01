<template>
  <v-app>
    <v-main class="mainApp">
      <v-container>
        <Nuxt />
      </v-container>
    </v-main>
    <v-footer id="bgColor" :absolute="!fixed" app>
      <!--
      <span>&copy; {{ new Date().getFullYear() }}</span> -->
    </v-footer>
  </v-app>
</template>

<script>
export default {
  data() {
    return {
      clipped: false,
      drawer: false,
      fixed: false,
      items: [
        {
          icon: "mdi-apps",
          title: "Welcome",
          to: "/",
        },
        {
          icon: "mdi-chart-bubble",
          title: "Inspire",
          to: "/inspire",
        },
      ],
      miniVariant: false,
      right: true,
      rightDrawer: false,
      title: "Notorious Jazz Café",
    };
  },
};
</script>

<style>
.mainApp,
#bgColor {
  background-color: #f9ecdb;
}
</style>