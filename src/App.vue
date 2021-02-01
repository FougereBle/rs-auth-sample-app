<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <div class="d-flex align-center">
        <v-img
          alt="Vuetify Logo"
          class="shrink mr-2"
          contain
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-logo-dark.png"
          transition="scale-transition"
          width="40"
        />

        <v-img
          alt="Vuetify Name"
          class="shrink mt-1 hidden-sm-and-down"
          contain
          min-width="100"
          src="https://cdn.vuetifyjs.com/images/logos/vuetify-name-dark.png"
          width="100"
        />
      </div>

      <v-spacer></v-spacer>

      <v-btn to="/" text>
        Home
      </v-btn>

      <v-btn to="/login" text v-if="!$store.state.user">
        Login
      </v-btn>

      <v-btn to="/register" text v-if="!$store.state.user">
        Register
      </v-btn>

      <v-btn @click="onLogout" text v-if="$store.state.user">
        {{ $store.state.user.username }} (Logout)
      </v-btn>
    </v-app-bar>

    <v-main>
      <router-view />
    </v-main>
  </v-app>
</template>

<script>
import AuthSDK from "rs-auth-js";

export default {
  name: "App",
  async created() {
    // Initialize Auth SDK
    AuthSDK.initialize({
      apiURL: "http://localhost:3000",
    });

    // Login User
    const response = await AuthSDK.Auth.autoLogin();
    this.$store.state.user = response.user;
  },
  methods: {
    async onLogout() {
      // Logout User
      await AuthSDK.Auth.logout();
      this.$store.state.user = null;
    },
  },
};
</script>
