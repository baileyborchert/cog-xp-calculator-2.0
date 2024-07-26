<template>
  <h1 v-text="cogType" />

  <form>
    <label for="suit">Choose your suit:</label>

    <select
      id="suit"
    >

    </select>
  </form>
</template>

<script>
import cogHqJson from './cog-hq.json'

export default {
  name: 'CogHQ',

  data() {
    return {
      hqData: null,
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
