<template>
  <div class="rate">
    <span>☆☆☆☆☆</span>
    <div class="hollow" :style="RateValue">★★★★★</div>
  </div>
</template>

<script>
  export default {
    props: {
      value: {
        type: [Number, String],
        default: '0'
      }
    },
    computed: {
      RateValue() {
        return `width: ${this.value/2 - 1}em`
      }
    }
  }
</script>

<style scoped="scoped">
  .rate {
    position: relative;
    display: inline-block;
  }
  .rate .hollow {
    position: absolute;
    display: inline-block;
    top: 0;
    left: 0;
    width: 0;
    overflow: hidden;
  }
</style>