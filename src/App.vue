<template>
  <div id="app">
    <div v-if="isLoading">
      loading...
    </div>
    <hmc-main :id="id" :all-data="allData" v-if="!isLoading"></hmc-main>
  </div>
</template>

<script>
import HmcMain from './components/hmc-main'
import { apiUrl } from './shared'
export default {
  name: 'app',
  components: {
    HmcMain
  },
  async created () {
    const headers = new Headers()
    headers.append('Content-Type', 'application/json')
    if (!this.id) {
      this.id = (await (await fetch(apiUrl, {
        method: 'POST',
        body: '{}',
        headers
      })).json()).uri.split('/').pop()
      history.replaceState('', '', this.id)
      this.isLoading = false
    } else {
      this.allData = await (await fetch(apiUrl + this.id)).json()
      this.isLoading = false
    }
  },
  data () {
    return {
      id: location.pathname.substr(1),
      allData: {},
      isLoading: true
    }
  }
}
</script>

<style>
#app {
}
</style>
