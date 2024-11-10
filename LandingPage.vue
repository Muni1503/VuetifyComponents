<template>
  <v-app>
    <v-app-bar app color="blue" dark>
      <v-toolbar-title class="text-h4 font-italic"
        ><v-icon icon="mdi-cash-multiple"></v-icon>TradePro</v-toolbar-title
      >
      <v-spacer></v-spacer>
      <v-btn @click="openDialog('login')" color="white">Login</v-btn>
      <v-btn outlined class="ml-2" @click="openDialog('signup')" color="white">
        Sign Up
      </v-btn>
    </v-app-bar>

    <v-main class="hero">
      <v-container fluid fill-height>
        <v-row align="center" justify="center">
          <v-col cols="12" md="6" class="text-center white--text">
            <h1 class="display-1 font-weight-bold mb-4">
              Trade Smarter, Faster
            </h1>
            <p class="subtitle-1 grey--text text--lighten-1 mb-8">
              Join thousands of traders and invest in your future today.
            </p>
            <v-btn
              x-large
              color="green"
              class="mr-4"
              @click="openDialog('signup')"
            >
              Get Started
            </v-btn>
            <v-btn x-large outlined color="cyan" @click="learnMore"
              >Learn More</v-btn
            >
          </v-col>
        </v-row>
      </v-container>
    </v-main>

    <v-container fluid pt-12 pb-12 class="grey lighten-3">
      <v-row justify="center">
        <v-col cols="12" md="4">
          <v-card class="elevation-4 rounded-lg mb-4">
            <v-card-text class="text-center py-8">
              <v-icon large color="primary">mdi-chart-line</v-icon>
              <h3 class="text-h6 font-weight-bold mt-4">Powerful Analytics</h3>
              <p class="text-subtitle-1">
                Gain insights and make informed decisions with our advanced
                charting tools.
              </p>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" md="4">
          <v-card class="elevation-4 rounded-lg mb-4">
            <v-card-text class="text-center py-8">
              <v-icon large color="primary">mdi-lock</v-icon>
              <h3 class="text-h6 font-weight-bold mt-4">Secure Transactions</h3>
              <p class="text-subtitle-1">
                Your data and funds are protected with the highest security
                standards.
              </p>
            </v-card-text>
          </v-card>
        </v-col>
        <v-col cols="12" md="4">
          <v-card class="elevation-4 rounded-lg mb-4">
            <v-card-text class="text-center py-8">
              <v-icon large color="primary">mdi-head-question-outline</v-icon>
              <h3 class="text-h6 font-weight-bold mt-4">Dedicated Support</h3>
              <p class="text-subtitle-1">
                Our friendly support team is always here to help you succeed.
              </p>
            </v-card-text>
          </v-card>
        </v-col>
      </v-row>
    </v-container>

    <v-dialog v-model="dialog" max-width="400">
      <v-card>
        <v-card-title class="text-h5 cyan lighten-2">
          {{ dialogAction }} to TradePro
        </v-card-title>
        <v-card-text>
          <v-form ref="form" v-model="valid" lazy-validation class="mt-4">
            <v-text-field
              v-model="email"
              :rules="emailRules"
              label="E-mail"
              required
              color="blue"
            ></v-text-field>
            <v-text-field
              v-model="password"
              :rules="passwordRules"
              label="Password"
              type="password"
              required
              color="blue"
            ></v-text-field>
          </v-form>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="grey darken-3" @click="dialog = false">Cancel</v-btn>
          <v-btn color="cyan" @click="submit">{{ dialogAction }}</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>

    <v-footer color="green" dark app>
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
    data() {
      return {
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
      }
    },
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
