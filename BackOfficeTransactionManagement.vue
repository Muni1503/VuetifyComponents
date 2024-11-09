<template>
  <v-container fluid class="pa-0">
    <v-card class="mx-auto" max-width="1200" elevation="5">
      <v-app-bar dark color="primary" prominent>
        <v-app-bar-title class="text-h4 font-weight-bold">
          Back Office Transaction Management
        </v-app-bar-title>
      </v-app-bar>

      <v-card-text class="pa-6 grey lighten-5">
        <v-row>
          <v-col cols="12" md="4">
            <v-menu
              v-model="dateMenu"
              :close-on-content-click="false"
              :nudge-right="40"
              transition="scale-transition"
              offset-y
              min-width="auto"
            >
              <template v-slot:activator="{ on, attrs }">
                <v-text-field
                  v-model="selectedDate"
                  label="Select Date"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                  outlined
                  dense
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="selectedDate"
                @input="dateMenu = false"
              ></v-date-picker>
            </v-menu>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="clientId"
              label="Client ID"
              prepend-icon="mdi-account"
              outlined
              dense
            ></v-text-field>
          </v-col>
          <v-col cols="12" md="4">
            <v-text-field
              v-model="tradeId"
              label="Trade ID"
              prepend-icon="mdi-identifier"
              outlined
              dense
            ></v-text-field>
          </v-col>
        </v-row>
        <v-btn color="primary" @click="applyFilters" class="mt-4">
          <v-icon left>mdi-filter</v-icon>
          Apply Filters
        </v-btn>
        <v-card class="mt-4">
          <v-card-title>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="filteredTransactions"
            :items-per-page="10"
            :search="search"
            :loading="loading"
            class="elevation-1"
          >
            <template v-slot:item.tradeType="{ item }">
              <v-chip
                :color="item.tradeType === 'Buy' ? 'success' : 'error'"
                text-color="white"
                small
              >
                {{ item.tradeType }}
              </v-chip>
            </template>
            <template v-slot:item.settlementStatus="{ item }">
              <v-select
                v-model="item.settlementStatus"
                :items="settlementStatusOptions"
                dense
                outlined
                :color="getStatusColor(item.settlementStatus)"
              ></v-select>
            </template>
            <template v-slot:item.actions="{ item }">
              <v-btn
                color="primary"
                small
                @click="updateSettlementStatus(item)"
                :loading="item.updating"
              >
                Update Status
              </v-btn>
            </template>
          </v-data-table>
        </v-card>
      </v-card-text>
    </v-card>
    <v-snackbar v-model="snackbar" :color="snackbarColor" top>
      {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn text v-bind="attrs" @click="snackbar = false"> Close </v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        dateMenu: false,
        selectedDate: new Date().toISOString().substr(0, 10),
        clientId: '',
        tradeId: '',
        search: '',
        headers: [
          { text: 'Stock Name', value: 'stockName' },
          { text: 'Quantity', value: 'quantity' },
          { text: 'Trade Type', value: 'tradeType' },
          { text: 'Settlement Status', value: 'settlementStatus' },
          { text: 'Actions', value: 'actions', sortable: false },
        ],
        transactions: [
          {
            id: 1,
            stockName: 'AAPL',
            quantity: 100,
            tradeType: 'Buy',
            settlementStatus: 'Pending',
            clientId: 'C001',
            tradeId: 'T001',
            date: '2023-05-15',
            updating: false,
          },
          {
            id: 2,
            stockName: 'GOOGL',
            quantity: 50,
            tradeType: 'Sell',
            settlementStatus: 'Completed',
            clientId: 'C002',
            tradeId: 'T002',
            date: '2023-05-15',
            updating: false,
          },
          {
            id: 3,
            stockName: 'MSFT',
            quantity: 75,
            tradeType: 'Buy',
            settlementStatus: 'Failed',
            clientId: 'C001',
            tradeId: 'T003',
            date: '2023-05-16',
            updating: false,
          },
          {
            id: 4,
            stockName: 'AMZN',
            quantity: 25,
            tradeType: 'Sell',
            settlementStatus: 'Pending',
            clientId: 'C003',
            tradeId: 'T004',
            date: '2024-11-09',
            updating: false,
          },
          {
            id: 5,
            stockName: 'TSLA',
            quantity: 10,
            tradeType: 'Buy',
            settlementStatus: 'Completed',
            clientId: 'C002',
            tradeId: 'T005',
            date: '2023-05-17',
            updating: false,
          },
        ],
        filteredTransactions: [],
        settlementStatusOptions: ['Pending', 'Completed', 'Failed'],
        loading: false,
        snackbar: false,
        snackbarText: '',
        snackbarColor: '',
      }
    },
    methods: {
      applyFilters() {
        this.loading = true
        setTimeout(() => {
          this.filteredTransactions = this.transactions.filter(transaction => {
            return (
              transaction.date === this.selectedDate &&
              (this.clientId === '' ||
                transaction.clientId === this.clientId) &&
              (this.tradeId === '' || transaction.tradeId === this.tradeId)
            )
          })
          this.loading = false
          this.showSnackbar('Filters applied successfully', 'success')
        }, 1000)
      },
      updateSettlementStatus(item) {
        this.$set(item, 'updating', true)
        setTimeout(() => {
          console.log(
            `Updated settlement status for transaction ${item.id} to ${item.settlementStatus}`
          )
          this.$set(item, 'updating', false)
          this.showSnackbar(
            `Settlement status updated for transaction ${item.id}`,
            'info'
          )
        }, 1000)
      },
      getStatusColor(status) {
        switch (status) {
          case 'Completed':
            return 'success'
          case 'Pending':
            return 'warning'
          case 'Failed':
            return 'error'
          default:
            return 'primary'
        }
      },
      showSnackbar(text, color) {
        this.snackbarText = text
        this.snackbarColor = color
        this.snackbar = true
      },
    },
    mounted() {
      this.applyFilters()
    },
  }
</script>
