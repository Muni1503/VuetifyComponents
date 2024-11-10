<template>
  <v-container fluid>
    <v-card class="elevation-2 mb-6">
      <v-card-title class="headline blue white--text">
        Billing Approver
      </v-card-title>
      <v-card-text>
        <v-data-table
          :headers="headers"
          :items="bills"
          item-key="billingId"
          show-select
          v-model="selectedBills"
          :single-select="false"
          class="elevation-1"
        >
          <template v-slot:top>
            <v-switch
              v-model="selectAll"
              label="Select All"
              class="mt-2"
              @change="toggleSelectAll"
            ></v-switch>
          </template>
        </v-data-table>
      </v-card-text>
    </v-card>

    <v-card v-if="selectedBills.length > 0" class="elevation-2">
      <v-card-title class="headline black--text">
        Approve Selected Bills
      </v-card-title>
      <v-card-text>
        <v-form ref="form" v-model="valid">
          <v-select
            v-model="approvalStatus"
            :items="approvalOptions"
            label="Approval Status"
            outlined
            dense
            required
          ></v-select>
          <v-textarea
            v-model="comments"
            label="Comments (Optional)"
            outlined
            dense
          ></v-textarea>
        </v-form>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="green"
          dark
          :disabled="!valid || !approvalStatus"
          @click="submitApproval"
        >
          Submit Approval
        </v-btn>
      </v-card-actions>
    </v-card>

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
    name: 'BillingApprover',
    data: () => ({
      valid: false,
      selectedBills: [],
      selectAll: false,
      headers: [
        { text: 'Billing ID', value: 'billingId' },
        { text: 'Client ID', value: 'clientId' },
        { text: 'Trade ID', value: 'tradeId' },
        { text: 'Total Amount', value: 'totalAmount' },
        { text: 'Brokerage', value: 'brokerage' },
        { text: 'STT', value: 'stt' },
        { text: 'Demat Charges', value: 'dematCharges' },
      ],
      bills: [
        {
          billingId: 'BIL001',
          clientId: 'CLI001',
          tradeId: 'TRD001',
          totalAmount: 1000.0,
          brokerage: 50.0,
          stt: 10.0,
          dematCharges: 5.0,
        },
        {
          billingId: 'BIL002',
          clientId: 'CLI002',
          tradeId: 'TRD002',
          totalAmount: 1500.0,
          brokerage: 75.0,
          stt: 15.0,
          dematCharges: 5.0,
        },
        {
          billingId: 'BIL003',
          clientId: 'CLI001',
          tradeId: 'TRD003',
          totalAmount: 2000.0,
          brokerage: 100.0,
          stt: 20.0,
          dematCharges: 5.0,
        },
      ],
      approvalStatus: '',
      comments: '',
      approvalOptions: [
        { text: 'Approved', value: 'approved' },
        { text: 'Rejected', value: 'rejected' },
        { text: 'Pending', value: 'pending' },
      ],
      snackbar: false,
      snackbarText: '',
      snackbarColor: '',
    }),
    watch: {
      selectedBills: {
        handler(newVal) {
          this.selectAll = newVal.length === this.bills.length
        },
        deep: true,
      },
    },
    methods: {
      toggleSelectAll() {
        this.selectedBills = this.selectAll ? [...this.bills] : []
      },
      submitApproval() {
        if (this.selectedBills.length === 0) {
          this.showSnackbar(
            'Please select at least one bill to approve',
            'error'
          )
          return
        }
        // Here you would typically make an API call to submit the approval
        console.log('Submitting approval:', {
          bills: this.selectedBills.map(bill => bill.billingId),
          approvalStatus: this.approvalStatus,
          comments: this.comments,
        })
        this.showSnackbar(
          `Approval submitted for ${this.selectedBills.length} bills: ${this.approvalStatus}`,
          'success'
        )
        // Reset selection after submission
        this.selectedBills = []
        this.selectAll = false
        this.approvalStatus = ''
        this.comments = ''
      },
      showSnackbar(text, color) {
        this.snackbarText = text
        this.snackbarColor = color
        this.snackbar = true
      },
    },
  }
</script>
