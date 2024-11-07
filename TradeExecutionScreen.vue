<template>
  <v-container
    fluid
    class="trade-bg fill-height"
    style="
      background-image: url('https://images.unsplash.com/photo-1611974789855-9c2a0a7236a3?auto=format&fit=crop&w=1920&q=80');
    "
  >
    <v-row align="center" justify="center">
      <v-col cols="12" sm="10" md="8" lg="6">
        <v-card class="mx-auto" elevation="12" shaped>
          <v-card-title
            class="text-h4 font-weight-bold white--text blue darken-2 py-4"
          >
            <v-icon large left color="white">mdi-chart-line</v-icon>
            Trade Execution
          </v-card-title>
          <v-card-text class="pa-6">
            <v-form ref="form" v-model="valid" lazy-validation>
              <v-select
                v-model="selectedStock"
                :items="stocks"
                item-text="name"
                item-value="name"
                label="Stock Name"
                required
                :rules="[v => !!v || 'Stock is required']"
                prepend-icon="mdi-alphabetical"
                class="mt-4"
              ></v-select>

              <v-text-field
                v-model="segment"
                label="Segment"
                readonly
                outlined
                prepend-icon="mdi-tag"
                class="mt-4"
              ></v-text-field>

              <v-text-field
                v-model="isin"
                label="ISIN"
                readonly
                outlined
                prepend-icon="mdi-barcode"
                class="mt-4"
              ></v-text-field>

              <v-text-field
                v-model="currentPrice"
                label="Current Price"
                readonly
                outlined
                prefix="$"
                prepend-icon="mdi-currency-usd"
                class="mt-4"
              ></v-text-field>

              <v-text-field
                v-model.number="quantity"
                label="Quantity"
                type="number"
                required
                :rules="[
                  v => !!v || 'Quantity is required',
                  v => v > 0 || 'Quantity must be greater than 0'
                ]"
                outlined
                prepend-icon="mdi-numeric"
                class="mt-4"
              ></v-text-field>

              <v-radio-group
                v-model="tradeType"
                row
                required
                :rules="[v => !!v || 'Trade type is required']"
                class="mt-4"
              >
                <template v-slot:label>
                  <div class="font-weight-bold">Trade Type</div>
                </template>
                <v-radio label="Buy" value="buy" color="green">
                  <template v-slot:label>
                    <v-icon color="green">mdi-arrow-up-bold-circle</v-icon> Buy
                  </template>
                </v-radio>
                <v-radio label="Sell" value="sell" color="red">
                  <template v-slot:label>
                    <v-icon color="red">mdi-arrow-down-bold-circle</v-icon> Sell
                  </template>
                </v-radio>
              </v-radio-group>

              <v-text-field
                v-model="estimatedTotal"
                label="Estimated Total"
                readonly
                outlined
                prefix="$"
                prepend-icon="mdi-calculator"
                class="mt-4"
              ></v-text-field>

              <v-btn
                @click="submitTrade"
                :disabled="!valid"
                color="blue darken-2"
                class="mt-6 white--text"
                x-large
                block
                :loading="loading"
              >
                <v-icon left>mdi-send</v-icon>
                Submit Trade
              </v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
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
    data: () => ({
      valid: true,
      selectedStock: null,
      quantity: null,
      tradeType: null,
      stocks: [
        {
          name: 'AAPL',
          segment: 'Technology',
          isin: 'US0378331005',
          price: 150.25,
        },
        {
          name: 'GOOGL',
          segment: 'Technology',
          isin: 'US02079K3059',
          price: 2800.75,
        },
        {
          name: 'MSFT',
          segment: 'Technology',
          isin: 'US5949181045',
          price: 300.5,
        },
      ],
      loading: false,
      snackbar: false,
      snackbarText: '',
      snackbarColor: 'success',
    }),
    computed: {
      segment() {
        const stock = this.stocks.find(s => s.name === this.selectedStock)
        return stock ? stock.segment : ''
      },
      isin() {
        const stock = this.stocks.find(s => s.name === this.selectedStock)
        return stock ? stock.isin : ''
      },
      currentPrice() {
        const stock = this.stocks.find(s => s.name === this.selectedStock)
        return stock ? stock.price.toFixed(2) : ''
      },
      estimatedTotal() {
        if (this.quantity && this.currentPrice) {
          return (this.quantity * parseFloat(this.currentPrice)).toFixed(2)
        }
        return ''
      },
    },
    methods: {
      submitTrade() {
        if (this.$refs.form.validate()) {
          this.loading = true
          // Simulate API call
          setTimeout(() => {
            this.loading = false
            this.snackbarText = 'Trade submitted successfully!'
            this.snackbarColor = 'success'
            this.snackbar = true
            console.log('Trade submitted:', {
              stock: this.selectedStock,
              segment: this.segment,
              isin: this.isin,
              currentPrice: this.currentPrice,
              quantity: this.quantity,
              tradeType: this.tradeType,
              estimatedTotal: this.estimatedTotal,
            })
            this.$refs.form.reset()
          }, 2000)
        }
      },
    },
  }
</script>
