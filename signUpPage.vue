<template>
  <v-app>
    <v-main class="black">
      <v-container fluid fill-height>
        <v-row align="center" justify="center">
          <v-col cols="12" sm="10" md="8" lg="6">
            <v-card class="elevation-12">
              <v-toolbar color="blue darken-3" dark flat>
                <v-toolbar-title class="text-h5 font-weight-bold">Sign Up for TradePro</v-toolbar-title>
              </v-toolbar>
              <v-card-text class="mt-5">
                <v-form ref="form" v-model="valid" lazy-validation>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="firstName"
                        :rules="nameRules"
                        label="First Name"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="lastName"
                        :rules="nameRules"
                        label="Last Name"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="email"
                        :rules="emailRules"
                        label="Email"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="phoneNumber"
                        :rules="phoneRules"
                        label="Phone Number"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="panNumber"
                        :rules="panRules"
                        label="PAN Number"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="nomineeName"
                        :rules="nameRules"
                        label="Nominee Name"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="bankName"
                        :rules="requiredRules"
                        label="Bank Name"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="branchName"
                        :rules="requiredRules"
                        label="Branch Name"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-row>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="ifscCode"
                        :rules="ifscRules"
                        label="IFSC Code"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                    <v-col cols="12" md="6">
                      <v-text-field
                        v-model="bankAccountNumber"
                        :rules="accountRules"
                        label="Bank Account Number"
                        required
                        outlined
                        dense
                      ></v-text-field>
                    </v-col>
                  </v-row>
                  <v-text-field
                    v-model="address"
                    :rules="requiredRules"
                    label="Address"
                    required
                    outlined
                    dense
                  ></v-text-field>
                  <v-select
                    v-model="kycStatus"
                    :items="kycStatusOptions"
                    :rules="requiredRules"
                    label="KYC Status"
                    required
                    outlined
                    dense
                  ></v-select>
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
                <v-btn color="blue darken-1" text @click="reset">
                  Cancel
                </v-btn>
                <v-btn
                  color="blue darken-3"
                  class="white--text"
                  @click="submit"
                  :disabled="!valid"
                >
                  Sign Up
                </v-btn>
              </v-card-actions>
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
    phoneNumber: '',
    panNumber: '',
    nomineeName: '',
    bankName: '',
    branchName: '',
    ifscCode: '',
    bankAccountNumber: '',
    address: '',
    kycStatus: '',
    agreeTerms: false,
    kycStatusOptions: ['Pending', 'Completed', 'Rejected'],
    nameRules: [
      v => !!v || 'Name is required',
      v => (v && v.length <= 50) || 'Name must be less than 50 characters',
    ],
    emailRules: [
      v => !!v || 'E-mail is required',
      v => /.+@.+\..+/.test(v) || 'E-mail must be valid',
    ],
    phoneRules: [
      v => !!v || 'Phone number is required',
      v => /^[0-9]{10}$/.test(v) || 'Phone number must be 10 digits',
    ],
    panRules: [
      v => !!v || 'PAN number is required',
      v => /^[A-Z]{5}[0-9]{4}[A-Z]{1}$/.test(v) || 'Invalid PAN number format',
    ],
    ifscRules: [
      v => !!v || 'IFSC code is required',
      v => /^[A-Z]{4}0[A-Z0-9]{6}$/.test(v) || 'Invalid IFSC code format',
    ],
    accountRules: [
      v => !!v || 'Bank account number is required',
      v => (v && v.length >= 9 && v.length <= 18) || 'Account number must be between 9 and 18 digits',
    ],
    requiredRules: [
      v => !!v || 'This field is required',
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
          phoneNumber: this.phoneNumber,
          panNumber: this.panNumber,
          nomineeName: this.nomineeName,
          bankName: this.bankName,
          branchName: this.branchName,
          ifscCode: this.ifscCode,
          bankAccountNumber: this.bankAccountNumber,
          address: this.address,
          kycStatus: this.kycStatus,
        })
        // You would typically make an API call here to register the user
        // For demonstration purposes, we'll just log the data
        alert('Signup successful! (Demo only)')
      }
    },
    reset() {
      this.$refs.form.reset()
    },
  },
}
</script>
