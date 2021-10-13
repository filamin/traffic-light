<template>
 <div class='light' :style="{background: currentColor, opacity: flash ? 0.3 : 1}"/>
</template>

<script>
export default {
  name: 'Light',
  props: {
    time: Number,
  },
  data() {
    return {
      flash: false,
    }
  },
  computed: {
    currentColor() {
      return this.$route.name;
    }
  },
  watch: {
    time: {
      handler(val) {
        if (val <= 3) {
          setTimeout(() => this.flash = true, 500)
          setTimeout(() => this.flash = false, 1000)
        }
      }
    }
  }
}
</script>

<style scoped>
.light {
  width: 100%;
  height: 100%;
  transition: all .3s ease-in-out;
}
.red {
  background-color: red;
}
.yellow {
  background-color: yellow;
}
.green {
  background: green;
}
</style>
