<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title class="text-h4 font-weight-bold"
        >TradePro</v-toolbar-title
      >
      <v-spacer></v-spacer>
      <v-btn text to="/">Home</v-btn>
      <v-btn outlined class="ml-2" to="/login">Login</v-btn>
    </v-app-bar>

    <v-main class="black">
      <v-container fluid fill-height>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="8" md="6" lg="4">
            <v-card class="elevation-12">
              <v-toolbar color="primary" dark flat>
                <v-toolbar-title>Sign Up for TradePro</v-toolbar-title>
              </v-toolbar>
              <v-card-text>
                <v-form ref="form" v-model="valid" lazy-validation>
                  <v-text-field
                    v-model="firstName"
                    :rules="nameRules"
                    label="First Name"
                    required
                    prepend-icon="mdi-account"
                  ></v-text-field>
                  <v-text-field
                    v-model="lastName"
                    :rules="nameRules"
                    label="Last Name"
                    required
                    prepend-icon="mdi-account"
                  ></v-text-field>
                  <v-text-field
                    v-model="email"
                    :rules="emailRules"
                    label="Email"
                    required
                    prepend-icon="mdi-email"
                  ></v-text-field>
                  <v-text-field
                    v-model="password"
                    :rules="passwordRules"
                    label="Password"
                    type="password"
                    required
                    prepend-icon="mdi-lock"
                  ></v-text-field>
                  <v-text-field
                    v-model="confirmPassword"
                    :rules="confirmPasswordRules"
                    label="Confirm Password"
                    type="password"
                    required
                    prepend-icon="mdi-lock-check"
                  ></v-text-field>
                  <v-checkbox
                    v-model="agreeTerms"
                    :rules="[v => !!v || 'You must agree to continue!']"
                    label="I agree to the Terms and Conditions"
                    required
                  ></v-checkbox>
                </v-form>
              </v-card-text>
              <v-card-actions>
                <v-spacer></v-spacer>
                <v-btn color="primary" @click="submit" :disabled="!valid">
                  Sign Up
                </v-btn>
              </v-card-actions>
              <v-card-text class="text-center">
                <v-divider class="my-3"></v-divider>
                <p class="mb-0">Already an existing user?</p>
                <v-btn text color="primary" to="/login"> Please Login </v-btn>
              </v-card-text>
            </v-card>
          </v-col>
        </v-row>
      </v-container>
    </v-main>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      valid: true,
      firstName: '',
      lastName: '',
      email: '',
      password: '',
      confirmPassword: '',
      agreeTerms: false,
      nameRules: [
        v => !!v || 'Name is required',
        v => (v && v.length <= 50) || 'Name must be less than 50 characters',
      ],
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 8) || 'Password must be at least 8 characters',
      ],
      confirmPasswordRules: [
        v => !!v || 'Password confirmation is required',
        v => v === this.password || 'Passwords must match',
      ],
    }),
    methods: {
      submit() {
        if (this.$refs.form.validate()) {
          // Implement signup logic here
          console.log('Form submitted:', {
            firstName: this.firstName,
            lastName: this.lastName,
            email: this.email,
            password: this.password,
          })
          // You would typically make an API call here to register the user
          // For demonstration purposes, we'll just log the data
          alert('Signup successful! (Demo only)')
        }
      },
    },
  }
</script>
