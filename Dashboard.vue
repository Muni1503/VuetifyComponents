<template>
  <v-container fluid class="pa-6" style="background-color: #f0f8ff">
    <v-row>
      <!-- Client Information -->
      <v-col cols="12" md="4">
        <v-card class="mb-4" elevation="4" hover>
          <v-card-title class="headline blue darken-3 white--text">
            <v-icon left color="white">mdi-account</v-icon>
            Client Information
          </v-card-title>
          <v-card-text class="pa-4" style="background-color: #e6f2ff">
            <v-text-field
              v-model="clientName"
              label="Client Name"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="email"
              label="Email"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="phoneNumber"
              label="Phone Number"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="panCardNumber"
              label="PAN Card Number"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="kycStatus"
              label="KYC Status"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="nomineeName"
              label="Nominee Name"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Bank Information and Subscribed Segments -->
      <v-col cols="12" md="4">
        <v-card class="mb-4" elevation="4" hover>
          <v-card-title class="headline blue darken-3 white--text">
            <v-icon left color="white">mdi-bank</v-icon>
            Bank Information
          </v-card-title>
          <v-card-text class="pa-4" style="background-color: #e6f2ff">
            <v-text-field
              v-model="bankName"
              label="Bank Name"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="branchName"
              label="Branch Name"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="ifscCode"
              label="IFSC Code"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
            <v-text-field
              v-model="bankAccountNumber"
              label="Bank Account Number"
              outlined
              dense
              color="blue darken-3"
              readonly
              background-color="white"
              class="mb-2"
            ></v-text-field>
          </v-card-text>
        </v-card>

        <v-card elevation="4" hover>
          <v-card-title class="headline blue darken-3 white--text">
            <v-icon left color="white">mdi-checkbox-multiple-marked</v-icon>
            Subscribed Segments
          </v-card-title>
          <v-card-text class="pa-4" style="background-color: #e6f2ff">
            <v-checkbox
              v-model="segments"
              label="NSE"
              value="NSE"
              color="blue darken-3"
              readonly
              disabled
              class="mb-1"
            ></v-checkbox>
            <v-checkbox
              v-model="segments"
              label="BSE"
              value="BSE"
              color="blue darken-3"
              readonly
              disabled
              class="mb-1"
            ></v-checkbox>
            <v-checkbox
              v-model="segments"
              label="MCX"
              value="MCX"
              color="blue darken-3"
              readonly
              disabled
              class="mb-1"
            ></v-checkbox>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Trade Data Table -->
      <v-col cols="12" md="4">
        <v-card elevation="4" hover>
          <v-card-title class="headline blue darken-3 white--text">
            <v-icon left color="white">mdi-table</v-icon>
            Trade Data
            <v-spacer></v-spacer>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
              color="blue darken-3"
              background-color="white"
              class="mt-2"
              dense
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="trades"
            :search="search"
            :items-per-page="5"
            class="elevation-1"
          >
            <template v-slot:header="{ props: { headers } }">
              <thead class="blue darken-1">
                <tr>
                  <th
                    v-for="header in headers"
                    :key="header.text"
                    class="white--text font-weight-bold text-subtitle-1"
                  >
                    {{ header.text }}
                  </th>
                </tr>
              </thead>
            </template>
            <template v-slot:item="{ item }">
              <tr
                :style="{ backgroundColor: item.index % 2 === 0 ? '#f5f9ff' : 'white' }"
              >
                <td
                  v-for="(value, key) in item"
                  :key="key"
                  class="text-subtitle-2"
                >
                  <span
                    :class="{'green--text': item.tradeType === 'Buy' && key === 'tradeType', 'red--text': item.tradeType === 'Sell' && key === 'tradeType'}"
                  >
                    {{ value }}
                  </span>
                </td>
              </tr>
            </template>
          </v-data-table>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
  export default {
    data: () => ({
      clientName: 'John Doe',
      email: 'john.doe@example.com',
      phoneNumber: '+1 (555) 123-4567',
      panCardNumber: 'ABCDE1234F',
      kycStatus: 'Completed',
      nomineeName: 'Jane Doe',
      bankName: 'Global Bank',
      branchName: 'Main Street Branch',
      ifscCode: 'GLOB0001234',
      bankAccountNumber: '1234567890',
      segments: ['NSE', 'BSE'],
      search: '',
      headers: [
        { text: 'Trade ID', value: 'tradeId' },
        { text: 'Stock Name', value: 'stockName' },
        { text: 'Quantity', value: 'quantity' },
        { text: 'Trade Type', value: 'tradeType' },
        { text: 'Price', value: 'price' },
        { text: 'Trade Date', value: 'tradeDate' },
      ],
      trades: [
        {
          tradeId: '001',
          stockName: 'AAPL',
          quantity: 100,
          tradeType: 'Buy',
          price: 150.5,
          tradeDate: '2023-04-15',
        },
        {
          tradeId: '002',
          stockName: 'GOOGL',
          quantity: 50,
          tradeType: 'Sell',
          price: 2800.75,
          tradeDate: '2023-04-16',
        },
        {
          tradeId: '003',
          stockName: 'MSFT',
          quantity: 75,
          tradeType: 'Buy',
          price: 280.25,
          tradeDate: '2023-04-17',
        },
        {
          tradeId: '004',
          stockName: 'AMZN',
          quantity: 30,
          tradeType: 'Buy',
          price: 3200.0,
          tradeDate: '2023-04-18',
        },
        {
          tradeId: '005',
          stockName: 'TSLA',
          quantity: 20,
          tradeType: 'Sell',
          price: 750.8,
          tradeDate: '2023-04-19',
        },
      ],
    }),
  }
</script>
