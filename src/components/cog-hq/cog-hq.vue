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
          v-for="(level, index) in cogLevelOptions"
          :value="level"
          :selected="index === 0"
          :key="level"
          v-text="level.level"
        />
      </select>
    </div>

    <!-- Checkbox if v2.0 -->
    <div
      v-if="activeCogSuit && isTopCog"
      class="form-field form-field--checkbox"
    >
      <input
        v-model="isV2"
        type="checkbox"
        id="isV2"
      />

      <label
        for="isV2"
      >v2.0</label>
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
      @click="handleSubmit"
    >
  </form>

  <div v-if="instancesNeeded.length > 0">
    <span>For a promotion from {{ activeCogSuit.name }} Level {{ activeCogLevel.level }}, you need:</span>

    <div v-if="hasMinAndMax">
      <span>At least:</span>

      <ul>
        <li
          v-for="instance in instancesNeeded[0]"
          :key="instance.name"
        >
          {{ instance.quantity }} {{ instance.name }}
        </li>
      </ul>

      <span>At most:</span>

      <ul>
        <li
          v-for="instance in instancesNeeded[1]"
          :key="instance.name"
        >
          {{ instance.quantity }} {{ instance.name }}
        </li>
      </ul>
    </div>

    <ul v-else>
      <li
        v-for="instance in instancesNeeded"
        :key="instance.name"
      >
        {{ instance.quantity }} {{ instance.name }}
      </li>
    </ul>
  </div>
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
      instancesNeeded: [],
      isV2: false,
    }
  },

  computed: {
    /**
     * Compute cog level options to render.
     * @returns {Array}
     */
    cogLevelOptions() {
      if (this.isTopCog) {
        let levels = []

        this.activeCogSuit.levels.forEach(levelObject => {
          const levelsInObject = levelObject.levels.map(levelNumber => {
            return {
              level: levelNumber,
              xp: levelObject.xp,
              xpV2: levelObject.xpV2
            }
          })

          levels.push(...levelsInObject)
        })

        return levels.sort((a, b) => a.level - b.level);
      }

      return this.activeCogSuit.levels
    },

    /**
     * Compute if both a min and max array of instances have been calculated.
     * @returns {Boolean}
     */
    hasMinAndMax() {
      if (!this.instancesNeeded.length) {
        return false
      }

      return Array.isArray(this.instancesNeeded[0])
    },

    /**
     * Compute if current Cog suit is the highest for that Cog type.
     * @returns {Boolean}
     */
    isTopCog() {
      if (!this.activeCogSuit) {
        return false
      }

      const cogsArray = this.hqData.cogs
      const topCog = cogsArray[cogsArray.length - 1];

      return topCog === this.activeCogSuit
    },

    /**
     * Compute XP needed based on current Cog suit and user input for current XP.
     * @returns {Number}
     */
    xpNeeded() {
      return this.activeCogLevel.xp - this.currentXp
    }
  },

  watch: {
    /**
     * On changes to Cog type, update HQ data.
     * Also reset active suit, active level, and instances needed.
     */
    cogType: function() {
      this.setHqData()
      this.activeCogSuit = null
      this.activeCogLevel = null
      this.instancesNeeded = []
    },

    /**
     * On change to Cog suit, clear level and instances needed.
     */
    activeCogSuit: function() {
      this.activeCogLevel = null
      this.instancesNeeded = []
    },

    /**
     * On change to level, clear instances needed.
     */
    activeCogLevel: function() {
      this.instancesNeeded = []
    }
  },

  props: {
    cogType: String,
  },

  mounted() {
    this.setHqData()  
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
     * Set HQ JSON data that matches the active Cog type.
     */
    setHqData() {
      this.hqData = cogHqJson.find(hq => hq.cogType === this.cogType)
    },

    /**
     * Handle submit button click.
     */
    handleSubmit() {
      event.preventDefault();
      this.setInstancesNeeded()
    },

    /**
     * Set an array of instance types and quantities needed.
     */
    setInstancesNeeded() {
      const instances = this.hqData.instances

      if (this.cogType === 'Sellbot' && this.xpNeeded <= instances[0].xp) {
        this.instancesNeeded = [
          {
            name: instances[0].name,
            quantity: 1,
          }
        ]
        return
      }

      const min = this.calculateInstances('min')
      const max = this.calculateInstances('max')

      if (this.minEqualsMax(min, max)) {
        this.instancesNeeded = min
        return
      }

      this.instancesNeeded = [[...min], [...max]]
    },

    /**
     * Calculate instances needed to return an array of instance objects with name and quantity.
     * @param {String} minOrMax - Whether we're calculating the min or max needed.
     * @returns {Array}
     */
    calculateInstances(minOrMax) {
      let xpNeededCopy = this.xpNeeded
      let instances = []

      this.hqData.instances.reverse().forEach(instance => {
        const isConsistentXp = typeof instance.xp === 'number'
        let quantity = 0

        if (isConsistentXp) {
          while (xpNeededCopy >= instance.xp) {
            quantity += 1
            xpNeededCopy -= instance.xp
          }

          if (quantity > 0) {
            instances.push({
              name: instance.name,
              quantity,
            })
          }
        } else {
          while (xpNeededCopy >= instance.xp[minOrMax]) {
            quantity += 1
            xpNeededCopy -= instance.xp[minOrMax]
          }

          if (quantity > 0) {
            instances.push({
              name: instance.name,
              quantity,
            })
          }
        }
      })

      return instances
    },

    /**
     * Check if min and max instances needed arrays are equal.
     * @param {Array} min - Minimum instances needed
     * @param {Array} max - Maximum instances needed
     * @returns {Boolean}
     */
    minEqualsMax(min, max) {
      if (min.length !== max.length) return false;

      return min.every((obj1, index) => {
        const obj2 = max[index]
        return JSON.stringify(obj1) === JSON.stringify(obj2)
      })
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
