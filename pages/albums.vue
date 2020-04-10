<template>
  <div>
    <h1>album</h1>
    <p>code: {{ spotifyCode }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      spotifyCode: ''
    }
  },
  mounted() {
    this.spotifyCode = this.$route.query.code
  }
}
</script>
