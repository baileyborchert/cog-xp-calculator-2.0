<template>
  <h1 v-text="cogType" />

  <form v-if="hqData">
    <!-- Cog suit selection-->
    <div class="form-field form-field--select">
      <label for="suit">Choose your suit:</label>

      <select
        id="suit"
        v-model="activeCogSuit"
      >
        <option disabled value="">Please select a Cog</option>

        <option
          v-for="(cog, index) in hqData.cogs"
          :value="cog"
          :selected="index === 0"
          :key="cog.name"
          v-text="cog.name"
        />
      </select>
    </div>

    <!-- Level selection -->
    <div
      v-if="activeCogSuit && activeCogSuit?.levels"
      class="form-field form-field--select"
    >
      <label for="level">Choose your level:</label> 

      <select
        id="level"
        v-model="activeCogLevel"
      >
        <option
          disabled
          value=""
        />

        <option
          v-for="(level, index) in activeCogSuit.levels"
          :value="level"
          :selected="index === 0"
          :key="level"
          v-text="level.level"
        />
      </select>
    </div>
    
    <!-- Checkbox if has XP -->
    <div
      v-if="activeCogSuit && activeCogLevel"
      class="form-field form-field--checkbox"
    >
      <input
        v-model="hasXp"
        type="checkbox"
        id="hasXp"
      />

      <label
        for="hasXp"
        v-text="getHasXpLabel()"
      />
    </div>
   
    
    <!-- Number input for current XP -->
    <div
      v-if="hasXp"
      class="form-field form-field--number"
    >
    <label
      for="currentXp"
      v-text="getCurrentXpLabel()"
    />

      <input
        v-model="currentXp"
        type="number"
        id="currentXp"
        min="0"
        :max="activeCogLevel.xp"
      />

    </div>

    <!-- Submit button-->
    <input
      v-if="activeCogSuit && activeCogLevel"
      type="submit"
      value="Submit"
    >
  </form>
</template>

<script>
import cogHqJson from './cog-hq.json'

export default {
  name: 'CogHQ',

  data() {
    return {
      activeCogSuit: null,
      activeCogLevel: null,
      currentXp: 0,
      hasXp: false,
      hqData: null,
    }
  },

  watch: {
    /**
     * On changes to Cog type, update HQ data, and reset active suit and level.
     */
    cogType: function() {
      this.getHqData()
      this.activeCogSuit = null
      this.activeCogLevel = null
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
     * Get label for checkbox input for if user has XP.
     * Include XP name string based on current Cog HQ.
     * @returns {String}
     */
    getHasXpLabel() {
      return `I already have some ${this.hqData.xpName}s`
    },

    /**
     * Get label for number input for current XP.
     * Include XP name string based on current Cog HQ.
     * @returns {String}
     */
    getCurrentXpLabel() {
      return `How many ${this.hqData.xpName}s do you have?`
    },

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
  form {
    align-items: center;
    display: flex;
    flex-flow: column nowrap;
    gap: 16px;
    margin: auto;
    width: fit-content;
  }

  .form-field--checkbox {
    display: flex;
    flex-flow: row nowrap;
    gap: 4px;
  }

  .form-field--select,
  .form-field--number {
    display: flex;
    flex-flow: column nowrap;
    gap: 4px;
  }
</style>
