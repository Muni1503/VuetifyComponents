<template>
  <v-container fluid>
    <v-row>
      <v-col cols="12">
        <h1 class="text-h4 primary--text mb-4">
          <v-icon large color="primary" class="mr-2">mdi-account-group</v-icon>
          Client Approval
        </h1>
      </v-col>
    </v-row>
    <v-row>
      <v-col
        v-for="(client, index) in clients"
        :key="client.id"
        cols="12"
        sm="6"
        md="4"
        lg="3"
      >
        <v-card
          elevation="2"
          :class="{ 'primary lighten-5': approvalStatus[index] }"
        >
          <v-card-title class="text-h6">
            <v-icon left color="primary">mdi-account</v-icon>
            {{ client.name }}
          </v-card-title>
          <v-card-text>
            <v-list dense>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon color="primary">mdi-email</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>{{ client.email }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon color="primary">mdi-phone</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>{{ client.phone }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon color="primary">mdi-office-building</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>{{ client.company }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
              <v-list-item>
                <v-list-item-icon>
                  <v-icon color="primary">mdi-card-account-details</v-icon>
                </v-list-item-icon>
                <v-list-item-content>
                  <v-list-item-title>PAN: {{ client.pan }}</v-list-item-title>
                </v-list-item-content>
              </v-list-item>
            </v-list>
          </v-card-text>
          <v-card-actions>
            <v-spacer></v-spacer>
            <v-checkbox
              v-model="selectedClients[index]"
              :disabled="approvalStatus[index]"
              :label="approvalStatus[index] ? 'Approved' : 'Select for Approval'"
              color="primary"
            ></v-checkbox>
          </v-card-actions>
        </v-card>
      </v-col>
    </v-row>
    <v-row class="mt-4">
      <v-col cols="12" class="text-center">
        <v-btn
          color="primary"
          large
          @click="approveSelectedClients"
          :disabled="!hasSelectedClients"
        >
          <v-icon left>mdi-check-all</v-icon>
          Approve Selected Clients
        </v-btn>
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
    name: 'ClientApprovalPage',
    data() {
      return {
        clients: [
          {
            id: 1,
            name: 'John Doe',
            email: 'john@example.com',
            phone: '(123) 456-7890',
            company: 'ABC Corp',
            pan: 'ABCDE1234F',
          },
          {
            id: 2,
            name: 'Jane Smith',
            email: 'jane@example.com',
            phone: '(234) 567-8901',
            company: 'XYZ Inc',
            pan: 'FGHIJ5678K',
          },
          {
            id: 3,
            name: 'Bob Johnson',
            email: 'bob@example.com',
            phone: '(345) 678-9012',
            company: '123 LLC',
            pan: 'KLMNO9012P',
          },
          {
            id: 4,
            name: 'Alice Brown',
            email: 'alice@example.com',
            phone: '(456) 789-0123',
            company: '456 Co',
            pan: 'QRSTU3456V',
          },
        ],
        approvalStatus: [],
        selectedClients: [],
        snackbar: false,
        snackbarText: '',
        snackbarColor: '',
      }
    },
    computed: {
      hasSelectedClients() {
        return this.selectedClients.some(selected => selected)
      },
    },
    created() {
      this.approvalStatus = new Array(this.clients.length).fill(false)
      this.selectedClients = new Array(this.clients.length).fill(false)
    },
    methods: {
      approveSelectedClients() {
        let approvedCount = 0
        this.selectedClients.forEach((selected, index) => {
          if (selected && !this.approvalStatus[index]) {
            this.approvalStatus[index] = true
            this.selectedClients[index] = false
            approvedCount++
          }
        })
        if (approvedCount > 0) {
          this.showSnackbar(
            `${approvedCount} client(s) approved successfully`,
            'success'
          )
        } else {
          this.showSnackbar('No new clients selected for approval', 'info')
        }
      },
      showSnackbar(text, color) {
        this.snackbarText = text
        this.snackbarColor = color
        this.snackbar = true
      },
    },
  }
</script>
