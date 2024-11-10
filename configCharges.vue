<template>
  <v-container>
    <v-form ref="form" v-model="isValid" lazy-validation>
      <!-- Charge Type Dropdown (Autocomplete with option to add new) -->
      <v-row>
        <v-col cols="12" md="6">
          <v-select
            label="Charge Type"
            v-model="charge.chargeType"
            :items="chargeTypes"
            outlined
            prepend-icon="mdi-cash-multiple"
            color="blue darken-2"
            autocomplete
            item-text="name"
            item-value="name"
            @change="onChargeTypeChange"
            :rules="[v => !!v || 'Charge Type is required']"
            :menu-props="{ maxHeight: '400' }"
            multiple
            return-object
            chips
          >
            <template v-slot:append-item>
              <v-list-item @click="addNewChargeType">
                <v-list-item-content>
                  <v-list-item-title>Add New Charge Type</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </template>
          </v-select>
        </v-col>
      </v-row>

      <!-- Charge Percentage Field -->
      <v-row>
        <v-col cols="12" md="6">
          <v-text-field
            label="Charge Percentage"
            v-model="charge.chargePercentage"
            type="number"
            outlined
            prepend-icon="mdi-percent"
            color="green darken-2"
            :rules="[v => !!v || 'Charge Percentage is required']"
          ></v-text-field>
        </v-col>
      </v-row>

      <!-- Effective Date Date Picker (Popup) -->
      <v-row>
        <v-col cols="12" md="6">
          <v-menu
            v-model="menu"
            :close-on-content-click="false"
            transition="scale-transition"
            offset-y
          >
            <template v-slot:activator="{ on, attrs }">
              <v-text-field
                v-bind="attrs"
                v-on="on"
                label="Effective Date"
                v-model="charge.effectiveDate"
                readonly
                prepend-icon="mdi-calendar"
                color="cyan darken-2"
                :rules="[v => !!v || 'Effective Date is required']"
              ></v-text-field>
            </template>
            <v-date-picker
              v-model="charge.effectiveDate"
              @input="menu = false"
            ></v-date-picker>
          </v-menu>
        </v-col>
      </v-row>

      <!-- Action Buttons -->
      <v-row class="mt-4" justify="space-between">
        <!-- Add/Update Charge Button -->
        <v-col cols="12" md="6">
          <v-btn
            color="blue darken-2"
            @click="addUpdateCharge"
            :disabled="!isValid"
            outlined
          >
            <v-icon left>mdi-plus-circle</v-icon>
            Add/Update Charge
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
        menu: false, // To control the date picker popup visibility
        charge: {
          chargeType: null,
          chargePercentage: null,
          effectiveDate: null,
        },
        chargeTypes: [
          { name: 'STT' },
          { name: 'Brokerage' },
          { name: 'Demat' },
        ], // Predefined charge types
      }
    },
    methods: {
      // Method to handle the addition of a new charge type
      addNewChargeType() {
        const newChargeType = prompt('Enter new charge type name:')
        if (newChargeType) {
          this.chargeTypes.push({ name: newChargeType })
          this.charge.chargeType = newChargeType // Set new charge type to the selected charge type
        }
      },

      // Method for when charge type changes
      onChargeTypeChange() {
        console.log('Charge Type changed:', this.charge.chargeType)
      },

      // Method for adding or updating a charge
      addUpdateCharge() {
        console.log('Charge Added/Updated:', this.charge)
        // Logic to handle charge addition/updating
      },
    },
  }
</script>
