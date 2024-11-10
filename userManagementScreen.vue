<template>
  <v-container>
    <v-form ref="form" v-model="isValid" lazy-validation>
      <!-- User ID Field (Read-only) -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="User ID"
            v-model="user.userID"
            readonly
            outlined
            prepend-icon="mdi-account"
            color="cyan darken-2"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- Username Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="Username"
            v-model="user.username"
            outlined
            prepend-icon="mdi-account-circle"
            color="cyan darken-2"
            :rules="[v => !!v || 'Username is required']"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- Role Dropdown with Autocomplete -->
      <v-row>
        <v-col cols="12" md="6">
          <v-select
            label="Role"
            v-model="user.role"
            :items="roles"
            outlined
            prepend-icon="mdi-shield-account"
            color="blue darken-2"
            autocomplete
            :rules="[v => !!v || 'Role is required']"
          ></v-select>
        </v-col>
      </v-row>

      <!-- Status Dropdown -->
      <v-row>
        <v-col cols="12" md="6">
          <v-select
            label="Status"
            v-model="user.status"
            :items="statuses"
            outlined
            prepend-icon="mdi-toggle-switch"
            color="green darken-2"
            :rules="[v => !!v || 'Status is required']"
          ></v-select>
        </v-col>
      </v-row>

      <!-- Action Buttons in Same Row -->
      <v-row class="mt-4" justify="space-between">
        <!-- Create User Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="blue darken-2"
            @click="createUser"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-account-plus</v-icon>
            Create User
          </v-btn>
        </v-col>

        <!-- Update User Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="cyan darken-2"
            @click="updateUser"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-account-edit</v-icon>
            Update User
          </v-btn>
        </v-col>

        <!-- Delete User Button -->
        <v-col cols="12" md="4">
          <v-btn
            color="red darken-2"
            @click="deleteUser"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-account-remove</v-icon>
            Delete User
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
        user: {
          userID: 'USER12345', // Sample read-only ID
          username: '',
          role: null,
          status: null,
        },
        roles: ['Client', 'BackOfficeAdmin', 'BillingApprover'], // Role options
        statuses: ['Active', 'Inactive'], // Status options
      }
    },
    methods: {
      createUser() {
        // Logic for creating a user
        console.log('User Created:', this.user)
      },
      updateUser() {
        // Logic for updating a user
        console.log('User Updated:', this.user)
      },
      deleteUser() {
        // Logic for deleting a user
        console.log('User Deleted:', this.user.userID)
      },
    },
  }
</script>
