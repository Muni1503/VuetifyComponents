<template>
  <v-container fluid class="pa-0">
    <v-card class="mx-auto" max-width="900" elevation="5">
      <v-app-bar
        dark
        color="primary"
        prominent
        src="https://picsum.photos/1920/1080?random"
      >
        <template v-slot:img="{ props }">
          <v-img
            v-bind="props"
            gradient="to top right, rgba(19,84,122,.8), rgba(128,208,199,.8)"
          ></v-img>
        </template>

        <v-app-bar-title class="text-h3 font-weight-bold">
          Trade History
        </v-app-bar-title>

        <v-spacer></v-spacer>

        <v-btn icon>
          <v-icon>mdi-magnify</v-icon>
        </v-btn>

        <v-btn icon>
          <v-icon>mdi-filter</v-icon>
        </v-btn>
      </v-app-bar>

      <v-card-text class="pa-0">
        <v-list height="500" class="overflow-y-auto">
          <v-list-item
            v-for="trade in trades"
            :key="trade.tradeId"
            class="mb-4"
          >
            <v-card width="100%" outlined>
              <v-card-title
                :class="trade.tradeType === 'Buy' ? 'success lighten-4' : 'error lighten-4'"
              >
                <v-icon
                  left
                  :color="trade.tradeType === 'Buy' ? 'success' : 'error'"
                >
                  {{ trade.tradeType === 'Buy' ? 'mdi-trending-up' :
                  'mdi-trending-down' }}
                </v-icon>
                {{ trade.stockName }}
                <v-spacer></v-spacer>
                <v-chip
                  :color="trade.tradeType === 'Buy' ? 'success' : 'error'"
                  text-color="white"
                  small
                >
                  {{ trade.tradeType }}
                </v-chip>
              </v-card-title>
              <v-card-text>
                <v-row dense>
                  <v-col cols="6" sm="4">
                    <v-list-item-subtitle class="text-caption"
                      >Quantity</v-list-item-subtitle
                    >
                    <v-list-item-title>{{ trade.quantity }}</v-list-item-title>
                  </v-col>
                  <v-col cols="6" sm="4">
                    <v-list-item-subtitle class="text-caption"
                      >Price</v-list-item-subtitle
                    >
                    <v-list-item-title
                      >${{ trade.price.toFixed(2) }}</v-list-item-title
                    >
                  </v-col>
                  <v-col cols="6" sm="4">
                    <v-list-item-subtitle class="text-caption"
                      >Trade Date</v-list-item-subtitle
                    >
                    <v-list-item-title
                      >{{ new Date(trade.tradeDate).toLocaleDateString()
                      }}</v-list-item-title
                    >
                  </v-col>
                  <v-col cols="6" sm="4">
                    <v-list-item-subtitle class="text-caption"
                      >Total Amount</v-list-item-subtitle
                    >
                    <v-list-item-title class="font-weight-bold"
                      >${{ trade.totalAmount.toFixed(2) }}</v-list-item-title
                    >
                  </v-col>
                </v-row>
              </v-card-text>
              <v-card-actions>
                <v-btn color="primary" text @click="viewBill(trade)">
                  <v-icon left>mdi-file-document-outline</v-icon>
                  View Bill
                </v-btn>
              </v-card-actions>
            </v-card>
          </v-list-item>
        </v-list>
      </v-card-text>
    </v-card>

    <v-dialog v-model="dialog" max-width="500">
      <v-card>
        <v-card-title class="text-h5 grey lighten-2">
          Bill Details
        </v-card-title>
        <v-card-text>
          <v-simple-table>
            <template v-slot:default>
              <tbody>
                <tr v-for="(value, key) in selectedBill" :key="key">
                  <td class="font-weight-medium">{{ formatKey(key) }}</td>
                  <td>{{ formatValue(key, value) }}</td>
                </tr>
              </tbody>
            </template>
          </v-simple-table>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn color="primary" text @click="downloadPdf">
            <v-icon left>mdi-file-pdf-box</v-icon>
            Download PDF
          </v-btn>
          <v-btn color="grey darken-1" text @click="dialog = false">
            Close
          </v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>

<script>
  export default {
    data() {
      return {
        trades: [
          {
            tradeId: 1,
            stockName: 'AAPL',
            tradeType: 'Buy',
            quantity: 100,
            price: 150.5,
            tradeDate: '2023-05-15',
            totalAmount: 15050,
            billingId: 'B001',
            brokerage: 15.05,
            stt: 15.05,
            dematCharges: 5.0,
            totalCharges: 35.1,
            billingDate: '2023-05-15',
          },
          {
            tradeId: 2,
            stockName: 'GOOGL',
            tradeType: 'Sell',
            quantity: 50,
            price: 2500.75,
            tradeDate: '2023-05-14',
            totalAmount: 125037.5,
            billingId: 'B002',
            brokerage: 125.04,
            stt: 125.04,
            dematCharges: 5.0,
            totalCharges: 255.08,
            billingDate: '2023-05-14',
          },
          // Add more trade objects as needed
        ],
        dialog: false,
        selectedBill: null,
      }
    },
    methods: {
      viewBill(trade) {
        this.selectedBill = {
          billingId: trade.billingId,
          tradeId: trade.tradeId,
          brokerage: trade.brokerage,
          stt: trade.stt,
          dematCharges: trade.dematCharges,
          totalCharges: trade.totalCharges,
          billingDate: trade.billingDate,
        }
        this.dialog = true
      },
      downloadPdf() {
        // Implement PDF generation and download logic here
        console.log(
          'Downloading PDF for billing ID:',
          this.selectedBill.billingId
        )
        // You would typically make an API call here to generate and download the PDF
      },
      formatKey(key) {
        return key.replace(/([A-Z])/g, ' $1').replace(/^./, function (str) {
          return str.toUpperCase()
        })
      },
      formatValue(key, value) {
        if (
          ['brokerage', 'stt', 'dematCharges', 'totalCharges'].includes(key)
        ) {
          return `$${value.toFixed(2)}`
        }
        return value
      },
    },
  }
</script>
