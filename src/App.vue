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

  <CogHeadquarters
    v-if="activeCogType === 'Sellbot'"
    cog-type="Sellbot"
    xp-name="Merit"
  />

  <CogHeadquarters
    v-if="activeCogType === 'Cashbot'"
    cog-type="Cashbot"
    xp-name="Cogbuck"
  />

  <CogHeadquarters
    v-if="activeCogType === 'Lawbot'"
    cog-type="Lawbot"
    xp-name="Jury Notice"
  />

  <CogHeadquarters
    v-if="activeCogType === 'Bossbot'"
    cog-type="Bossbot"
    xp-name="Stock Option"
  />
</template>

<script>
import CogHeadquarters from './components/CogHeadquarters.vue'

export default {
  name: 'App',

  components: {
    CogHeadquarters
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
