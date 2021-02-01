<template>
  <v-container>
    <h1>Login</h1>
    <v-alert v-if="error" type="error">
      {{ error }}
    </v-alert>
    <form @submit.prevent="onLoginFormSubmit">
      <div>
        <v-text-field v-model="username" type="text" label="Username" />
      </div>
      <div>
        <v-text-field v-model="password" type="password" label="Password" />
      </div>
      <div>
        <v-btn type="submit">Login</v-btn>
      </div>
    </form>
  </v-container>
</template>

<script>
import AuthSDK from "rs-auth-js";

export default {
  name: "Login",
  data() {
    return {
      error: null,
      username: null,
      password: null,
    };
  },
  methods: {
    async onLoginFormSubmit() {
      this.error = null;

      const response = await AuthSDK.Auth.login({
        username: this.username,
        password: this.password,
      });

      if (response.error) {
        this.error = response.message;
      } else {
        this.$store.state.user = response.user;
        this.$router.push("/");
      }
    },
  },
};
</script>
