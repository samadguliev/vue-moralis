<template>
  <button @click="click()">AASSSD</button>
</template>

<script>
export default {
  name: "Button",
  methods: {
    click() {
      ethereum.request({ method: 'eth_requestAccounts' });
    }
  },
  created() {

  }
}
</script>

<style scoped>

</style>