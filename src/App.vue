<template>
  <div id='app'>
    <router-view :time="displayedTime" />
    <div class="timer">
      {{displayedTime}}
    </div>
  </div>
</template>

<script>
export default {
  mounted() {
    const saveStorage = this.saveStorage;
    const savedData = this.openStorage();
    window.addEventListener('beforeunload', function () {
      saveStorage()
    })
    if (savedData) {
      this.lastWasGreen = savedData.lwg
      this.setNextColor(savedData.color, savedData.time)
    } else {
      this.setNextColor(this.currentColor)
    }
  },
  data() {
    return {
      lastWasGreen: false,
      displayedTime: 0,
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
    async setNextColor(color, sec) {
      switch (color) {
        case 'red':
          this.lastWasGreen = false
          await this.timer(sec || 10,'yellow')
          break
        case 'yellow':
          await this.timer(sec || 3, this.lastWasGreen ? 'red' : 'green')
          break
        case 'green':
          this.lastWasGreen = true
          await this.timer(sec || 15,'yellow')
          break
      }
    },
    async timer(sec, nextColor) {
      const router = this.$router
      const setTime = (time) => this.setDisplayedTime(time);
      let secToNextColor = sec
      setTime(secToNextColor)
      let timer = setInterval(function() {
        secToNextColor--
        setTime(secToNextColor)
        if (secToNextColor === 0) {
          clearInterval(timer)
          router.push(`/${nextColor}`)
        }
      }, 1000)
    },
    setDisplayedTime(time) {
      this.displayedTime = time
    },
    openStorage() {
      return JSON.parse(localStorage.getItem('appStatus'))
    },
    saveStorage() {
      localStorage.setItem('appStatus', JSON.stringify({time: this.displayedTime, color: this.currentColor, lwg: this.lastWasGreen}))
    },
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
  position: relative;
}
.timer {
  position: absolute;
  top: 0;
  width: 150px;
  padding: 20px 0;
  text-align: center;
  color: white;
  font-family: monospace;
  font-weight: bold;
  font-size: 24px;
  background: rgba(0, 0, 0, 0.4);
}
</style>
