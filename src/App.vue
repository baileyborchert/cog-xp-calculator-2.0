<template>
  <CogHeadquarters
    cog-type="Sellbot"
    xp-name="Merit"
  />
  <CogHeadquarters
    cog-type="Cashbot"
    xp-name="Cogbuck"
  />
  <CogHeadquarters
    cog-type="Lawbot"
    xp-name="Jury Notice"
  />
  <CogHeadquarters
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
