<template>
  <ul>
    <li
      v-for="cogType of cogTypes"
      :key="cogType"
    >
      <button
        type="button"
        v-text="cogType"
        @click="switchActiveCogType(cogType)"
      />
    </li>
  </ul>

  <CogHQ
    :cog-type="activeCogType"
  />
</template>

<script>
import CogHQ from './components/cog-hq/cog-hq.vue'

export default {
  name: 'App',

  components: {
    CogHQ
  },

  data() {
    return {
      activeCogType: 'Sellbot',
      cogTypes: ['Sellbot', 'Cashbot', 'Lawbot', 'Bossbot'],
    }
  },

  methods: {
    /**
     * Switches active Cog type.
     * @param {String} cogType 
     */
    switchActiveCogType(cogType) {
      this.activeCogType = cogType
    },
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
