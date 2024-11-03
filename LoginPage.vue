<template>
  <v-container class="fill-height" fluid>
    <v-row align="center" justify="center">
      <v-col cols="12" sm="8" md="6" lg="4">
        <v-card class="elevation-12 rounded-lg">
          <v-card-title class="white--text">
            <v-icon large left color="white">mdi-login</v-icon>
            Welcome Back
          </v-card-title>

          <v-card-text class="pt-6">
            <v-form>
              <v-text-field
                v-model="email"
                label="Email"
                prepend-icon="mdi-email"
                type="email"
                outlined
                dense
              ></v-text-field>
              <v-text-field
                v-model="password"
                label="Password"
                prepend-icon="mdi-lock"
                :append-icon="showPassword ? 'mdi-eye' : 'mdi-eye-off'"
                :type="showPassword ? 'text' : 'password'"
                @click:append="showPassword = !showPassword"
                outlined
                dense
              ></v-text-field>
              <v-checkbox
                v-model="rememberMe"
                label="Remember me"
                color="primary"
              ></v-checkbox>
            </v-form>
          </v-card-text>
          <v-card-actions>
            <v-btn text color="primary" small> Forgot Password? </v-btn>
            <v-spacer></v-spacer>
            <v-btn color="primary" large>
              Sign In
              <v-icon right>mdi-arrow-right</v-icon>
            </v-btn>
          </v-card-actions>
          <v-divider></v-divider>
          <v-card-actions class="justify-center">
            <v-btn text color="primary"> Don't have an account? Sign up </v-btn>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
export default {
  name: "LoginPage",
  data: () => ({
    email: "",
    password: "",
    showPassword: false,
    rememberMe: false,
  }),
};
</script>
