<template>
  <v-container>
    <h1>Register</h1>
    <v-alert v-if="error" type="error">
      {{ error }}
    </v-alert>
    <form @submit.prevent="onRegisterFormSubmit">
      <div>
        <v-text-field v-model="username" type="text" label="Username" />
      </div>
      <div>
        <v-text-field v-model="password" type="password" label="Password" />
      </div>
      <div>
        <v-btn type="submit">Register</v-btn>
      </div>
    </form>
  </v-container>
</template>

<script>
import AuthSDK from "rs-auth-js";

export default {
  name: "Register",
  data() {
    return {
      error: null,
      username: null,
      password: null,
    };
  },
  methods: {
    async onRegisterFormSubmit() {
      this.error = null;

      const response = await AuthSDK.Auth.register({
        username: this.username,
        password: this.password,
      });

      if (response.error) {
        this.error = response.message;
      } else {
        this.$router.push("/login");
      }
    },
  },
};
</script>
