<template>
  <v-container>
    <!-- Scrollable Cards Container -->
    <v-card max-height="600px" class="overflow-y-auto pa-4">
      <v-row>
        <!-- Each Bill Card -->
        <v-col cols="12" sm="6" md="4" v-for="bill in bills" :key="bill.billingId">
          <v-card outlined elevation="2">
            <!-- Card Header -->
            <v-card-title class="blue lighten-4">
              <v-icon color="blue darken-2" left>mdi-file-document</v-icon>
              <div>
                <h3 class="text-h6 font-weight-bold mb-1">Billing ID: {{ bill.billingId }}</h3>
                <p class="text-subtitle-1 grey--text">{{ bill.clientName }}</p>
              </div>
            </v-card-title>

            <!-- Trade Details Table -->
            <v-card-subtitle class="pb-0">
              <v-data-table
                :headers="tradeHeaders"
                :items="bill.tradeDetails"
                class="elevation-1"
                dense
                hide-default-footer
              >
                <template v-slot:item.totalCharges="{ item }">
                  <v-chip color="green darken-2" dark>{{ item.quantity * item.price }}</v-chip>
                </template>
              </v-data-table>
            </v-card-subtitle>

            <!-- Total Charges Display -->
            <v-card-subtitle>
              <v-chip color="green lighten-1" dark>Total Charges: {{ calculateTotal(bill.tradeDetails) }}</v-chip>
            </v-card-subtitle>

            <!-- Action Buttons -->
            <v-card-actions>
              <v-btn color="blue darken-2" @click="generatePdf(bill)" class="mr-2" outlined>
                <v-icon left>mdi-file-pdf-box</v-icon> Generate PDF
              </v-btn>
              <v-btn color="green darken-2" @click="downloadPdf(bill)" outlined>
                <v-icon left>mdi-download</v-icon> Download PDF
              </v-btn>
            </v-card-actions>
          </v-card>
        </v-col>
      </v-row>
    </v-card>
  </v-container>
</template>

<script>
import jsPDF from "jspdf";

export default {
  data() {
    return {
      bills: [
        {
          billingId: "BILL12345",
          clientName: "John Doe",
          tradeDetails: [
            { tradeId: "T123", stockName: "AAPL", quantity: 10, price: 150, tradeDate: "2024-01-01" },
            { tradeId: "T124", stockName: "GOOGL", quantity: 5, price: 200, tradeDate: "2024-01-02" },
          ],
        },
        {
          billingId: "BILL67890",
          clientName: "Jane Smith",
          tradeDetails: [
            { tradeId: "T125", stockName: "MSFT", quantity: 8, price: 250, tradeDate: "2024-02-01" },
            { tradeId: "T126", stockName: "AMZN", quantity: 3, price: 400, tradeDate: "2024-02-02" },
          ],
        },
        // Additional bills can go here
      ],
      tradeHeaders: [
        { text: "Trade ID", value: "tradeId" },
        { text: "Stock Name", value: "stockName" },
        { text: "Quantity", value: "quantity" },
        { text: "Price", value: "price" },
        { text: "Trade Date", value: "tradeDate" },
        { text: "Total Charges", value: "totalCharges" },
      ],
    };
  },
  methods: {
    calculateTotal(tradeDetails) {
      return tradeDetails.reduce((acc, trade) => acc + trade.quantity * trade.price, 0);
    },
    generatePdf(bill) {
      const doc = new jsPDF();
      doc.text("Billing ID: " + bill.billingId, 10, 10);
      doc.text("Client Name: " + bill.clientName, 10, 20);
      doc.text("Trade Details:", 10, 30);

      bill.tradeDetails.forEach((trade, index) => {
        const y = 40 + index * 10;
        doc.text(
          `Trade ID: ${trade.tradeId}, Stock Name: ${trade.stockName}, Quantity: ${trade.quantity}, Price: ${trade.price}, Date: ${trade.tradeDate}`,
          10,
          y
        );
      });

      doc.text("Total Charges: " + this.calculateTotal(bill.tradeDetails), 10, 40 + bill.tradeDetails.length * 10);
      doc.save(`Bill_${bill.billingId}.pdf`);
    },
    downloadPdf(bill) {
      this.generatePdf(bill);
    },
  },
};
</script>
