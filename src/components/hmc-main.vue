<template>
  <div class="hmc-main">
    <div class="row">
      <div class="col">
        <input type="text" v-model="newPro" @keyup.enter="addPro">
        <button @click="addPro" :disabled="!newPro">add</button>
        <ul>
          <li v-for="(item, i) in pros" :key="i">
            <button @click="pros.splice(i, 1)">x</button>
            {{item.name}}
          </li>
        </ul>
      </div>
      <div class="col">
        <input type="text" v-model="newCon" @keyup.enter="addCon">
        <button @click="addCon" :disabled="!newCon">add</button>
        <ul>
          <li v-for="(item, i) in cons" :key="i">
            <button @click="pros.splice(i, 1)">x</button>
            {{item.name}}
          </li>
        </ul>
      </div>
    </div>
    <div class="results">
      <button :disabled="!pros.length || !cons.length"
              @click="calc">
        calculate
      </button>
      {{resultText}}
      <button @click="save" :disabled="isSaving">save</button>
    </div>
  </div>
</template>

<script>
import Vue from 'vue'
import { apiUrl } from '../shared'

const Component = Vue.extend({
  name: 'hmc-main',
  props: [
    'id',
    'allData'
  ],
  data () {
    return {
      pros: this.allData.pros || [],
      cons: this.allData.cons || [],
      newPro: '',
      newCon: '',
      resultText: '',
      isSaving: false
    }
  },
  methods: {
    addPro () {
      this.pros.push({ name: this.newPro })
      this.newPro = ''
    },
    addCon () {
      this.cons.push({ name: this.newCon })
      this.newCon = ''
    },
    calc () {
      if (this.cons.length > this.pros.length) {
        this.resultText = 'dont do it'
      } else if (this.pros.length > this.cons.length) {
        this.resultText = 'do IT!'
      } else {
        this.resultText = 'X3'
      }
    },
    async save () {
      const headers = new Headers()
      headers.append('Content-Type', 'application/json')
      this.isSaving = true
      await fetch(apiUrl + this.id, {
        method: 'PUT',
        body: JSON.stringify({
          pros: this.pros,
          cons: this.cons
        }),
        headers
      })
      this.isSaving = false
    }
  }
})

export default Component
</script>

<style scoped>
.row {
  display: flex;
  justify-content: space-around;
}
.results > * {
  display: block;
  margin: 0 auto;
}
</style>
