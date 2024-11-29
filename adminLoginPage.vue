<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card elevation="6" class="pa-4">
          <v-card-title class="text-h4 justify-center mb-4">
            <v-icon large color="primary" class="mr-2">mdi-login</v-icon>
            Welcome Back
          </v-card-title>
          <v-card-text>
            <v-form @submit.prevent="login">
              <v-text-field
                v-model="email"
                label="Email"
                prepend-icon="mdi-email"
                type="email"
                required
                :rules="[rules.required, rules.email]"
              ></v-text-field>
              <v-text-field
                v-model="password"
                label="Password"
                prepend-icon="mdi-lock"
                :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                :type="showPassword ? 'text' : 'password'"
                required
                :rules="[rules.required, rules.min]"
                @click:append="showPassword = !showPassword"
              ></v-text-field>
              <v-btn
                type="submit"
                color="primary"
                block
                large
                class="mt-4"
                :loading="loading"
              >
                Login
                <v-icon right>mdi-arrow-right</v-icon>
              </v-btn>
            </v-form>
          </v-card-text>
          <v-card-actions class="justify-center">
            <v-btn text color="primary">Forgot Password?</v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    name: 'LoginPage',
    data() {
      return {
        email: '',
        password: '',
        showPassword: false,
        loading: false,
        rules: {
          required: v => !!v || 'This field is required',
          email: v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
          min: v => v.length >= 8 || 'Password must be at least 8 characters',
        },
      }
    },
    methods: {
      login() {
        if (this.$refs.form.validate()) {
          this.loading = true
          // Simulating login process
          setTimeout(() => {
            this.loading = false
            // Here you would typically make an API call to authenticate the user
            console.log('Login attempted with:', this.email, this.password)
            // After successful login, you might want to redirect or update app state
          }, 2000)
        }
      },
    },
  }
</script>

<style scoped>
  .v-card {
    border-radius: 16px;
  }
</style>
