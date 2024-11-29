<template>
  <v-container fluid class="subscribed-segments">
    <v-card class="mx-auto" max-width="600" elevation="8">
      <v-card-title class="text-h4 font-weight-bold primary white--text">
        <v-icon large left color="white">mdi-chart-box</v-icon>
        Subscribed Segments
      </v-card-title>
      <v-card-text>
        <v-list>
          <v-list-item v-for="segment in segments" :key="segment.name">
            <v-list-item-icon>
              <v-icon :color="segment.selected ? 'primary' : 'grey'"
                >{{ segment.icon }}</v-icon
              >
            </v-list-item-icon>
            <v-list-item-content>
              <v-list-item-title class="text-h6"
                >{{ segment.name }}</v-list-item-title
              >
              <v-list-item-subtitle
                >{{ segment.description }}</v-list-item-subtitle
              >
            </v-list-item-content>
            <v-list-item-action>
              <v-checkbox
                v-model="segment.selected"
                color="primary"
                @change="updateSelection(segment)"
              ></v-checkbox>
            </v-list-item-action>
          </v-list-item>
        </v-list>
      </v-card-text>
      <v-card-actions>
        <v-spacer></v-spacer>
        <v-btn
          color="primary"
          large
          @click="saveSubscriptions"
          :disabled="!hasSelectedSegments"
        >
          <v-icon left>mdi-content-save</v-icon>
          Save Subscriptions
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
    name: 'SubscribedSegments',
    data() {
      return {
        segments: [
          {
            name: 'NSE',
            icon: 'mdi-chart-line',
            description: 'National Stock Exchange',
            selected: false,
          },
          {
            name: 'BSE',
            icon: 'mdi-chart-bar',
            description: 'Bombay Stock Exchange',
            selected: false,
          },
          {
            name: 'MCX',
            icon: 'mdi-gold',
            description: 'Multi Commodity Exchange',
            selected: false,
          },
        ],
        snackbar: false,
        snackbarText: '',
        snackbarColor: '',
      }
    },
    computed: {
      hasSelectedSegments() {
        return this.segments.some(segment => segment.selected)
      },
      selectedSegments() {
        return this.segments
          .filter(segment => segment.selected)
          .map(segment => segment.name)
      },
    },
    methods: {
      updateSelection(segment) {
        console.log(
          `${segment.name} is now ${
            segment.selected ? 'selected' : 'unselected'
          }`
        )
      },
      saveSubscriptions() {
        if (this.hasSelectedSegments) {
          console.log('Selected segments:', this.selectedSegments)
          this.showSnackbar(
            `Subscribed to: ${this.selectedSegments.join(', ')}`,
            'success'
          )
        } else {
          this.showSnackbar('Please select at least one segment', 'warning')
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

<style scoped>
  .subscribed-segments {
    padding-top: 2rem;
    background-color: #f5f5f5;
    min-height: 100vh;
  }
</style>
