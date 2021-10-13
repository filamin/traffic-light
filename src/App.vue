<template>
  <div id='app'>
    <router-view/>
  </div>
</template>

<script>
export default {
  mounted() {
    this.setNextColor(this.currentColor)
  },
  data() {
    return {
      lastWasGreen: false,
    }
  },
  computed: {
    currentColor() {
      return this.$route.name;
    }
  },
  watch: {
    currentColor: {
      handler(val) {
        this.setNextColor(val)
      }
    }
  },
  methods: {
    async setNextColor(color) {
      switch (color) {
        case 'red':
          this.lastWasGreen = false
          await this.timer(10,'yellow')
          break
        case 'yellow':
          await this.timer(3, this.lastWasGreen ? 'red' : 'green')
          break
        case 'green':
          this.lastWasGreen = true
          await this.timer(15,'yellow')
          break
      }
    },
    async timer(sec, nextColor) {
      const router = this.$router
      let secToNextColor = sec

      let timer = setInterval(function() {
        secToNextColor--
        if (secToNextColor < 0) {
          clearInterval(timer)
          router.push(`/${nextColor}`)
        }
      }, 1000)
    }
  }
}
</script>

<style>
* {
  margin: 0
}
#app {
  height: 100vh;
  width: 100vw;
}
</style>
