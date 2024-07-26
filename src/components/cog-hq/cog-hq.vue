<template>
  <h1 v-text="cogType" />

  <form v-if="hqData">
    <label for="suit">Choose your suit:</label>

    <select
      id="suit"
      v-model="activeCogSuit"
    >
      <option disabled value="">Please select one</option>

      <option
        v-for="(cog, index) in hqData.cogs"
        :value="cog"
        :selected="index === 0"
        :key="cog.name"
        v-text="cog.name"
      />
    </select>
    
    <template v-if="activeCogSuit && activeCogSuit?.levels">
      <label for="level">Choose your level:</label>
      
      <select
        id="level"
      >
        <option
          v-for="(level, index) in activeCogSuit.levels"
          :selected="index === 0"
          :key="level"
          v-text="level.level"
        />
      </select>
    </template>
  </form>
</template>

<script>
import cogHqJson from './cog-hq.json'

export default {
  name: 'CogHQ',

  data() {
    return {
      activeCogSuit: null,
      hqData: null,
    }
  },

  watch: {
    /**
     * On changes to Cog type, update HQ data.
     */
    cogType: function() {
      this.getHqData()
    }
  },

  props: {
    cogType: String,
  },

  mounted() {
    this.getHqData()  
  },

  methods: {
    /**
     * Get HQ JSON data that matches the active Cog type.
     */
    getHqData() {
      this.hqData = cogHqJson.find(hq => hq.cogType === this.cogType)
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
