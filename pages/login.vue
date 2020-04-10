<template>
  <div>
    <h1>Login</h1>
    <button @click="login">Authorize</button>
    <p>url: {{ url }}</p>
    <p>spotifyClientId: {{ spotifyClientId }}</p>
    <p>spotifyClientSecret: {{ spotifyClientSecret }}</p>
    <p>spotifyRedirectUri: {{ spotifyRedirectUri }}</p>
    <p>spotifyScopes: {{ spotifyScopes }}</p>
  </div>
</template>
<script>
export default {
  data() {
    return {
      url: '',
      spotifyClientId: process.env.NUXT_ENV_SPOTIFY_CLIENT_ID,
      spotifyClientSecret: process.env.NUXT_ENV_SPOTIFY_CLIENT_SECRET,
      spotifyRedirectUri: process.env.NUXT_ENV_SPOTIFY_REDIRECT_URI,
      spotifyScopes: process.env.NUXT_ENV_SPOTIFY_SCOPES
    }
  },
  methods: {
    login() {
      const authorizeEndpoint = 'https://accounts.spotify.com/authorize'

      const url = `${authorizeEndpoint}?response_type=code&client_id=${
        this.spotifyClientId
      }&scope=${encodeURIComponent(
        this.spotifyScopes
      )}&redirect_uri=${encodeURIComponent(this.spotifyRedirectUri)}`

      this.url = url
      window.location = url
    }
  }
}
</script>
