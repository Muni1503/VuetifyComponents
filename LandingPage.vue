<template>
  <v-app>
    <v-app-bar app color="primary" dark>
      <v-toolbar-title class="text-h4 font-weight-bold"
        >TradePro</v-toolbar-title
      >
      <v-spacer></v-spacer>
      <v-btn text @click="openDialog('login')">Login</v-btn>
      <v-btn outlined class="ml-2" @click="openDialog('signup')">Sign Up</v-btn>
    </v-app-bar>

    <v-main class="black">
      <v-container fluid fill-height>
        <v-row align="center" justify="center">
          <v-col cols="12" md="6" class="text-center">
            <h1 class="text-h2 font-weight-bold white--text mb-4">
              Trade Smarter, Faster
            </h1>
            <p class="text-h5 grey--text text--lighten-1 mb-8">
              Join thousands of traders and invest in your future today.
            </p>
            <v-btn
              x-large
              color="primary"
              class="mr-4"
              @click="openDialog('signup')"
            >
              Get Started
            </v-btn>
            <v-btn x-large outlined color="white" @click="learnMore">
              Learn More
            </v-btn>
          </v-col>
          <v-col cols="12" md="6" class="d-none d-md-flex justify-center">
            <v-img
              src="/placeholder.svg?height=400&width=600"
              alt="Trading Chart"
              max-width="600"
              contain
            ></v-img>
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-dialog v-model="dialog" max-width="400">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          {{ dialogAction }} to TradePro
        </v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation class="mt-4">
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
            ></v-text-field>
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="Password"
              type="password"
              required
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="dialog = false">Cancel</v-btn>
          <v-btn color="primary" @click="submit">{{ dialogAction }}</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-footer color="primary" dark app>
      <v-row justify="center" no-gutters>
        <v-col class="py-4 text-center white--text" cols="12">
          {{ new Date().getFullYear() }} — <strong>TradePro</strong>
        </v-col>
      </v-row>
    </v-footer>
  </v-app>
</template>

<script>
  export default {
    data: () => ({
      dialog: false,
      dialogAction: 'Login',
      valid: true,
      email: '',
      emailRules: [
        v => !!v || 'E-mail is required',
        v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
      ],
      password: '',
      passwordRules: [
        v => !!v || 'Password is required',
        v => (v && v.length >= 8) || 'Password must be at least 8 characters',
      ],
    }),
    methods: {
      openDialog(action) {
        this.dialogAction = action === 'login' ? 'Login' : 'Sign Up'
        this.dialog = true
      },
      learnMore() {
        // Implement learn more functionality
        console.log('Learn More clicked')
      },
      submit() {
        if (this.$refs.form.validate()) {
          // Implement login/signup logic here
          console.log(
            `${this.dialogAction} submitted:`,
            this.email,
            this.password
          )
          this.dialog = false
        }
      },
    },
  }
</script>
