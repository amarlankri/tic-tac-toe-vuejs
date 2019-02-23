<template>
  <div>
    <div
      v-for="i in 3"
      :key="i"  
      class='board-row'>

      <Square v-for="j in 3"
        :key="j"
        :value='squares[getSquareIndex(i, j)]'
        :highlight = "isHighlighted(i, j)"
        @click='$emit("square-click",getSquareIndex(i, j))' />

    </div>
  </div>
</template>

<script>
import Square from "./Square";

export default {
  name: "Board",
  props: {
    squares: {
      type: Array,
      required: true
    },
    winningCombination: {
      type: Array
    }
  },
  components: {
    Square
  },
  methods: {
    getSquareIndex: function(i, j) {
      return 3 * (i - 1) + (j - 1);
    },
    isHighlighted: function(i, j) {
      const index = this.getSquareIndex(i, j);

      return this.winningCombination
        ? this.winningCombination.some(e => e === index)
        : false
    }
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.board-row:after {
  clear: both;
  content: "";
  display: table;
}

.status {
  margin-bottom: 10px;
}
</style>
