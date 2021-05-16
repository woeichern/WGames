<template>
  <div class="container">
    <div v-for="lights, index1 in light_array">
      <span v-for="light, index2 in lights">
        <b-button squared variant="warning" class="light" v-if="light === 1" @click="clicklight(index1, index2)"></b-button>
        <b-button squared variant="dark" class="light" v-else @click="clicklight(index1, index2)"></b-button>
      </span>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      config: {
        STEPS_INITIAL: 12,
        SIZE: 7,
      },
      light_array: [],
    }
  },
  beforeMount: function() {
    let light_array = [];

    for (let i = 0; i < this.config.SIZE; i++) {
      let lights = [];

      for (let k = 0; k < this.config.SIZE; k++) {
        lights.push(0);
      }

      light_array.push(lights);
    }

    this.light_array = light_array.slice();

    for (let i = 0; i < this.config.STEPS_INITIAL; i++) {
      let y = this.get_random_int(this.config.SIZE);
      let x = this.get_random_int(this.config.SIZE);

      this.clicklight(y, x);
    }
  },
  methods: {
    get_random_int(max) {
      return Math.floor(Math.random() * max);
    },

    clicklight (y, x) {
      // Center
      this.light_array[y][x] = +(!this.light_array[y][x]);

      // Up
      if (y > 0) {
        this.light_array[y-1][x] = +(!this.light_array[y-1][x]);
      }

      // Down
      if (y < this.config.SIZE -1) {
        this.light_array[y+1][x] = +(!this.light_array[y+1][x]);
      }

      // Right
      if (x < this.config.SIZE -1) {
        this.light_array[y][x+1] = +(!this.light_array[y][x+1]);
      }

      // Left
      if (x > 0) {
        this.light_array[y][x-1] = +(!this.light_array[y][x-1]);
      }

      this.$forceUpdate();
    }
  },
}
</script>

<style scoped>
.light {
  width: 8vw;
  height: 8vw;

  min-height: 30px;
  min-width: 30px;
}
</style>
