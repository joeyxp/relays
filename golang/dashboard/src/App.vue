<template>
  <v-app class="app">
    <v-app-bar app>
      <v-layout
        justify-space-between
        align-center
        class="nav__content"
      >
        <v-toolbar-title class="headline text-uppercase">
          <v-img
            alt="Summa"
            src="./assets/Summa-Logo.png"
            height="42"
            width="35"
          ></v-img>
        </v-toolbar-title>

        <v-spacer/>

        <div class="nav__title">
          <h1>Cosmos - Bitcoin Relay</h1>
        </div>

        <v-spacer/>
      </v-layout>
    </v-app-bar>

    <Relay-Connection />

    <Relay-Info />
    <v-btn class="app__content__update" @click="updateAll">REFRESH</v-btn>

  </v-app>
</template>

<script>
export default {
  name: 'CosmosRelayDashboard',

  metaInfo: {
    title: 'Cosmos-Bitcoin Relay Dashboard',
    meta: [
      { 'http-equiv': 'Content-Type', content: 'text/html; charset=utf-8' },
      { name: 'viewport', content: 'width=device-width, initial-scale=1' },
      { name: 'description', content: 'Cosmos-Bitcoin Relay Dashboard' }
    ]
  },

  components: {
    RelayInfo: () => import(/* webpackChunkName: 'Relay-Info' */ './components/Relay-Info/Info-Main'),
    RelayConnection: () => import(/* webpackChunkName: 'Relay-Connection' */ './components/Relay-Connection')
  },

  mounted () {
    // Get relay info - best know digest(BKD), last common ancestor(LCA)
    this.getRelayInfo()
    // Get external info and set it in the store, start polling
    this.getExternalInfo()
    setInterval(this.getExternalInfo, 180000) // 3 mins
    setInterval(this.getRelayInfo, 120000) // 2 mins
  },

  methods: {
    getRelayInfo () {
      console.log('Getting relay info')
      this.$store.dispatch('relay/getBKD')
      this.$store.dispatch('relay/getLCA')
    },

    getExternalInfo () {
      this.$store.dispatch('external/getExternalInfo')
    },

    updateAll () {
      this.getRelayInfo()
      this.getExternalInfo()
    }
  }
}
</script>

<style>
.app__content__update {
  width: 100px;
  margin: 10px auto;
}

.nav__content {
  max-width: 1200px;
  margin: auto;
}
.nav__title {
  font-weight: 500;
  font-size: 0.8em;
}
</style>
