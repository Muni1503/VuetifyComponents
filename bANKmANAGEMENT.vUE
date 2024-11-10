<template>
  <v-container>
    <!-- Title for the Screen -->
    <v-row>
      <v-col cols="12" class="text-center">
        <v-typography variant="h4" class="font-weight-bold purple--text">
          Bank Management
        </v-typography>
      </v-col>
    </v-row>

    <!-- Form -->
    <v-form ref="form" v-model="isValid" lazy-validation>
      <!-- Bank Name Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="Bank Name"
            v-model="bank.bankName"
            outlined
            prepend-icon="mdi-bank"
            color="blue darken-2"
            :rules="[v => !!v || 'Bank Name is required']"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- Branch Name Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="Branch Name"
            v-model="bank.branchName"
            outlined
            prepend-icon="mdi-branch"
            color="blue darken-2"
            :rules="[v => !!v || 'Branch Name is required']"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- IFSC Code Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="IFSC Code"
            v-model="bank.ifscCode"
            outlined
            prepend-icon="mdi-code-braces"
            color="green darken-2"
            :rules="[v => !!v || 'IFSC Code is required']"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- Address Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-textarea
            label="Address"
            v-model="bank.address"
            outlined
            prepend-icon="mdi-map-marker"
            color="cyan darken-2"
            :rules="[v => !!v || 'Address is required']"
            auto-grow
          ></v-textarea>
        </v-col>
      </v-row>

      <!-- Action Buttons -->
      <v-row class="mt-4" justify="space-between">
        <!-- Add Bank Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="blue darken-2"
            @click="addBank"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-bank-plus</v-icon>
            Add Bank
          </v-btn>
        </v-col>

        <!-- Update Bank Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="green darken-2"
            @click="updateBank"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-bank-edit</v-icon>
            Update Bank
          </v-btn>
        </v-col>

        <!-- Delete Bank Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="red darken-2"
            @click="deleteBank"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-bank-remove</v-icon>
            Delete Bank
          </v-btn>
        </v-col>
      </v-row>
    </v-form>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        isValid: false, // Form validity state
        bank: {
          bankName: '',
          branchName: '',
          ifscCode: '',
          address: '',
        },
      }
    },
    methods: {
      // Method for adding a new bank
      addBank() {
        console.log('Bank Added:', this.bank)
        // Logic to handle bank addition
      },
      // Method for updating an existing bank
      updateBank() {
        console.log('Bank Updated:', this.bank)
        // Logic to handle bank update
      },
      // Method for deleting a bank
      deleteBank() {
        console.log('Bank Deleted:', this.bank)
        // Logic to handle bank deletion
      },
    },
  }
</script>
