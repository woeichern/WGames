<template>
  <div class="container">
    <div v-for="lights, index1 in light_array">
      <span v-for="light, index2 in lights">
        <b-button squared variant="warning" class="light" v-if="light === 1" @click="clicklight(index1, index2)"></b-button>
        <b-button squared variant="dark" class="light" v-else @click="clicklight(index1, index2)"></b-button>
      </span>
    </div>
    <div class="control-bar">
      <b-button variant="success" @click="new_game">New Game</b-button>
      <b-button variant="light">Steps: {{ steps }}</b-button>
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
      steps: 0,
      light_array: [],
    }
  },
  beforeMount: function() {
    this.new_game();
  },
  methods: {
    get_random_int(max) {
      return Math.floor(Math.random() * max);
    },

    new_game(){
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

        this.steps = 0;
      }
    },

    clicklight (y, x) {
      this.steps++;

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
.control-bar {
  margin-top: 40px;
  display: flex;
  justify-content: space-around;

  text-align: center;
}

.container{
  margin-top: 40px;
  max-width: 70vw;
}

.light {
  width: 8vw;
  height: 8vw;

  min-height: 30px;
  min-width: 30px;
}
</style>
