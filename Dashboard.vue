<template>
  <v-container
    fluid
    class="pa-6"
    :class="$vuetify.theme.dark ? 'grey darken-4' : 'grey lighten-4'"
  >
    <v-row>
      <!-- Client Information -->
      <v-col cols="12" md="4">
        <v-card
          class="mb-4"
          elevation="4"
          :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'"
        >
          <v-card-title
            class="headline blue darken-2 white--text d-flex align-center"
          >
            <v-icon left color="white" large>mdi-account-circle</v-icon>
            Client Information
          </v-card-title>
          <v-card-text class="pa-4">
            <v-list>
              <v-list-item
                v-for="(value, key) in clientInfo"
                :key="key"
                two-line
              >
                <v-list-item-icon>
                  <v-icon :color="getIconColor(key)">{{ getIcon(key) }}</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title class="subtitle-1 font-weight-medium"
                    >{{ formatLabel(key) }}</v-list-item-title
                  >
                  <v-list-item-subtitle class="text--primary"
                    >{{ value }}</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Bank Information and Subscribed Segments -->
      <v-col cols="12" md="4">
        <v-card
          class="mb-4"
          elevation="4"
          :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'"
        >
          <v-card-title
            class="headline green darken-3 white--text d-flex align-center"
          >
            <v-icon left color="white" large>mdi-bank</v-icon>
            Bank Information
          </v-card-title>
          <v-card-text class="pa-4">
            <v-list>
              <v-list-item v-for="(value, key) in bankInfo" :key="key" two-line>
                <v-list-item-icon>
                  <v-icon :color="getBankIconColor(key)"
                    >{{ getBankIcon(key) }}</v-icon
                  >
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title class="subtitle-1 font-weight-medium"
                    >{{ formatLabel(key) }}</v-list-item-title
                  >
                  <v-list-item-subtitle class="text--primary"
                    >{{ value }}</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card-text>
        </v-card>

        <v-card
          elevation="4"
          :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'"
        >
          <v-card-title
            class="headline teal darken-4 white--text d-flex align-center"
          >
            <v-icon left color="white" large
              >mdi-checkbox-multiple-marked-circle</v-icon
            >
            Subscribed Segments
          </v-card-title>
          <v-card-text class="pa-4">
            <v-chip-group column>
              <v-chip
                v-for="segment in segments"
                :key="segment"
                color="teal"
                text-color="white"
                label
                large
                class="ma-2"
              >
                <v-icon left>mdi-check-circle</v-icon>
                {{ segment }}
              </v-chip>
            </v-chip-group>
          </v-card-text>
        </v-card>
      </v-col>

      <!-- Trade Data Table -->
      <v-col cols="12" md="4">
        <v-card
          elevation="4"
          :color="$vuetify.theme.dark ? 'grey darken-3' : 'white'"
        >
          <v-card-title class="headline teal darken-4 white--text">
            <v-icon left color="white">mdi-table</v-icon>
            Trade Data
            <v-spacer></v-spacer>
            <v-text-field
              v-model="search"
              append-icon="mdi-magnify"
              label="Search"
              single-line
              hide-details
              dark
              class="mt-2"
            ></v-text-field>
          </v-card-title>
          <v-data-table
            :headers="headers"
            :items="trades"
            :search="search"
            :items-per-page="5"
            class="elevation-1"
          >
            <template v-slot:item.tradeType="{ item }">
              <v-chip
                :color="item.tradeType === 'Buy' ? 'green' : 'red'"
                text-color="white"
                small
              >
                {{ item.tradeType }}
              </v-chip>
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
      search: '',
      clientInfo: {
        clientName: 'John Doe',
        email: 'john.doe@example.com',
        phoneNumber: '+1 (555) 123-4567',
        panCardNumber: 'ABCDE1234F',
        kycStatus: 'Completed',
        nomineeName: 'Jane Doe',
      },
      bankInfo: {
        bankName: 'Global Bank',
        branchName: 'Main Street Branch',
        ifscCode: 'GLOB0001234',
        bankAccountNumber: '1234567890',
      },
      segments: ['NSE', 'BSE', 'MCX'],
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
    methods: {
      formatLabel(key) {
        return key
          .split(/(?=[A-Z])/)
          .join(' ')
          .replace(/\b\w/g, l => l.toUpperCase())
      },
      getIcon(key) {
        const icons = {
          clientName: 'mdi-account',
          email: 'mdi-email',
          phoneNumber: 'mdi-phone',
          panCardNumber: 'mdi-card-account-details',
          kycStatus: 'mdi-shield-check',
          nomineeName: 'mdi-account-multiple',
        }
        return icons[key] || 'mdi-information'
      },
      getIconColor(key) {
        const colors = {
          clientName: 'blue',
          email: 'green',
          phoneNumber: 'purple',
          panCardNumber: 'orange',
          kycStatus: 'teal',
          nomineeName: 'pink',
        }
        return colors[key] || 'grey'
      },
      getBankIcon(key) {
        const icons = {
          bankName: 'mdi-bank',
          branchName: 'mdi-map-marker',
          ifscCode: 'mdi-barcode',
          bankAccountNumber: 'mdi-credit-card',
        }
        return icons[key] || 'mdi-information'
      },
      getBankIconColor(key) {
        const colors = {
          bankName: 'green',
          branchName: 'red',
          ifscCode: 'blue',
          bankAccountNumber: 'purple',
        }
        return colors[key] || 'grey'
      },
    },
  }
</script>
