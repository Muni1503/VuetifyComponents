<template>
  <v-container fluid>
    <v-card class="elevation-2">
      <v-card-title class="headline blue white--text">
        Billing Generation Screen
      </v-card-title>
      <v-card-text>
        <v-row>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              v-model="clientID"
              label="Client ID"
              outlined
              dense
              @input="filterTrades"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
            <v-text-field
              v-model="tradeID"
              label="Trade ID"
              outlined
              dense
              @input="filterTrades"
            ></v-text-field>
          </v-col>
          <v-col cols="12" sm="6" md="4">
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
                  v-model="tradeDate"
                  label="Trade Date"
                  prepend-icon="mdi-calendar"
                  readonly
                  v-bind="attrs"
                  v-on="on"
                  outlined
                  dense
                ></v-text-field>
              </template>
              <v-date-picker
                v-model="tradeDate"
                @input="dateMenu = false"
                @change="filterTrades"
              ></v-date-picker>
            </v-menu>
          </v-col>
        </v-row>
        <v-data-table
          :headers="headers"
          :items="filteredTrades"
          :items-per-page="5"
          class="elevation-1"
        >
          <template v-slot:item.tradeID="{ item }">
            <v-btn text color="blue" @click="showTradeDetails(item)">
              {{ item.tradeID }}
            </v-btn>
          </template>
          <template v-slot:item.actions="{ item }">
            <v-btn small color="green" dark @click="generateBill(item)">
              Generate Bill
            </v-btn>
          </template>
        </v-data-table>
      </v-card-text>
    </v-card>
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title class="headline blue white--text">
          Trade Details
        </v-card-title>
        <v-card-text>
          <v-list>
            <v-list-item v-for="(value, key) in selectedTrade" :key="key">
              <v-list-item-content>
                <v-list-item-title>{{ key }}</v-list-item-title>
                <v-list-item-subtitle>{{ value }}</v-list-item-subtitle>
              </v-list-item-content>
            </v-list-item>
          </v-list>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="blue darken-1" text @click="dialog = false"
            >Close</v-btn
          >
        </v-card-actions>
      </v-card>
    </v-dialog>
    <v-snackbar v-model="snackbar" :color="snackbarColor" top>
      {{ snackbarText }}
      <template v-slot:action="{ attrs }">
        <v-btn text v-bind="attrs" @click="snackbar = false">Close</v-btn>
      </template>
    </v-snackbar>
  </v-container>
</template>

<script>
  export default {
    name: 'BillingGenerationScreen',
    data: () => ({
      clientID: '',
      tradeID: '',
      tradeDate: '',
      dateMenu: false,
      headers: [
        { text: 'Trade ID', value: 'tradeID' },
        { text: 'Trade Date', value: 'tradeDate' },
        { text: 'Client ID', value: 'clientID' },
        { text: 'Stock Name', value: 'stockName' },
        { text: 'Quantity', value: 'quantity' },
        { text: 'Brokerage Percentage', value: 'brokeragePercentage' },
        { text: 'STT Percentage', value: 'sttPercentage' },
        { text: 'Demat Charges', value: 'dematCharges' },
        { text: 'Total Charges', value: 'totalCharges' },
        { text: 'Actions', value: 'actions', sortable: false },
      ],
      trades: [
        {
          id: 1,
          clientID: 'C001',
          tradeID: 'T001',
          tradeDate: '2023-05-15',
          stockName: 'AAPL',
          quantity: 100,
          brokeragePercentage: 0.5,
          sttPercentage: 0.1,
          dematCharges: 15,
          totalCharges: 65,
        },
        {
          id: 2,
          clientID: 'C002',
          tradeID: 'T002',
          tradeDate: '2023-05-16',
          stockName: 'GOOGL',
          quantity: 50,
          brokeragePercentage: 0.5,
          sttPercentage: 0.1,
          dematCharges: 15,
          totalCharges: 40,
        },
        {
          id: 3,
          clientID: 'C001',
          tradeID: 'T003',
          tradeDate: '2023-05-17',
          stockName: 'MSFT',
          quantity: 75,
          brokeragePercentage: 0.5,
          sttPercentage: 0.1,
          dematCharges: 15,
          totalCharges: 52.5,
        },
      ],
      dialog: false,
      selectedTrade: null,
      snackbar: false,
      snackbarText: '',
      snackbarColor: '',
    }),
    computed: {
      filteredTrades() {
        return this.trades.filter(
          trade =>
            trade.clientID
              .toLowerCase()
              .includes(this.clientID.toLowerCase()) &&
            trade.tradeID.toLowerCase().includes(this.tradeID.toLowerCase()) &&
            (this.tradeDate ? trade.tradeDate === this.tradeDate : true)
        )
      },
    },
    methods: {
      filterTrades() {
        // The filtering is handled by the computed property
      },
      showTradeDetails(item) {
        this.selectedTrade = { ...item }
        this.dialog = true
      },
      generateBill(item) {
        // Implement bill generation logic here
        console.log('Generating bill for:', item)
        // You would typically call an API or perform some action here
        this.showSnackbar(`Bill generated for ${item.stockName}`, 'success')
      },
      showSnackbar(text, color) {
        this.snackbarText = text
        this.snackbarColor = color
        this.snackbar = true
      },
    },
  }
</script>

