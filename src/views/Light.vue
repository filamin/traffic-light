<template>
 <div class='light' :class='currentColor'/>
</template>

<script>
export default {
  name: 'Light',
  computed: {
    currentColor() {
      return this.$route.name;
    }
  }
}
</script>

<style scoped>
.light {
  width: 100%;
  height: 100%;
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
