<template>
  <div class="circle">
    <svg class="timer" viewBox="0 0 200 200" preserveAspectRatio="xMinYMin meet" xmlns="http://www.w3.org/2000/svg">
      <circle class="bigCircle" r="50" cx="50" cy="50"></circle>
      <circle class="smallCircle" r="40" cx="50" cy="50"></circle>
      <path class="segment" :d="path"></path>
      <text v-if="text != ''" class="text" x="50" y="50">
        {{text}}
      </text>
    </svg>
  </div>
</template>
<script>
  /**
   * Calculates the x,y coordinates on the circumference for the given angle
   * @param {number} angle
   * @returns {object}
   */
  function calcEndPoint (angle) {
    let x, y

    x = 50 - 50 * Math.sin(Math.PI * angle / 180)
    y = 50 - 50 * Math.cos(Math.PI * angle / 180)

    return {
      x, y
    }
  }
  /**
   * Calculates the path attribute for the svg element to draw a circle sector for the given angle
   * @param {number} angle
   * @returns {string}
   */
  function calcPath (angle) {
    let d
    let {x, y} = calcEndPoint(angle)
    if (angle <= 180) {
      d = `M50,50 L50, 0 A50,50 0 0,0 ${x}, ${y} z`
    } else {
      d = `M50,50  L50, 0 A50,50 0 0,0 50, 100 A50,50 0 0,0 ${x}, ${y} z`
    }
    return d
  }
  export default {
    props: ['angle', 'text'],
    computed: {
      path () {
        return calcPath(this.angle)
      }
    }
  }
</script>
<style scoped lang="scss">
  $big-circle-color: gray;
  $small-circle-color: lightgray;
  $segment-color: darkgray;
  $text-color: black;
  .bigCircle {
    fill: $big-circle-color;
  }
  .smallCircle {
    fill: $small-circle-color;
  }
  .segment {
    fill: $segment-color;opacity: 0.6;
  }
  .text {
    font-size: 1em;
    stroke-width: 0;
    opacity: .9;
    fill: $text-color;
  }
</style>
