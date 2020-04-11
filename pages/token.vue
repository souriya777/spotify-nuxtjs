<template>
  <div>
    <h1>Token</h1>
    <p>token: {{ token }}</p>
  </div>
</template>
<script>
export default {
  fetch() {
    this.token = this.$route.params
  },
  data() {
    return {
      token: ''
    }
  }
}
</script>
