<template>
  <button class="square"
  :class="{highlight : highlight}"
    @click="$emit('click')">
    {{value}}
  </button>
</template>

<script>
export default {
  name: "Square",
  props: {
    value: String,
    highlight: Boolean
  }
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
.square {
  background: #fff;
  border: 1px solid #999;
  float: left;
  font-size: 24px;
  font-weight: bold;
  line-height: 34px;
  height: 34px;
  margin-right: -1px;
  margin-top: -1px;
  padding: 0;
  text-align: center;
  width: 34px;
}

.highlight {
  background-color: red;
}


.square:focus {
  outline: none;
}

.kbd-navigation .square:focus {
  background: #ddd;
}
</style>
