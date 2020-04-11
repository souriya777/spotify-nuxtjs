<template>
  <div>
    <h1>Login</h1>
    <button @click="login">Authorize</button>
    <!-- <button v-if="spotifyCode" @click="getToken">Get token</button> -->
    <p>code: {{ spotifyCode }}</p>
    <p>token: {{ spotifyToken }}</p>
    <div v-if="spotifyCode" class="code">
      <code>{{ curl }}</code>
    </div>
  </div>
</template>
<script>
// import axios from 'axios'

const AUTHORIZE_ENDPOINT = 'https://accounts.spotify.com/authorize'
const TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token'

export default {
  // 1st
  fetch() {
    // this.login()
    this.spotifyToken = this.$route.query.code
    // this.spotifyToken = 'fetch - token OK'
    // this.spotifyToken = 'fetch - token KO'
  },
  // 2nd
  // asyncData() {
  //   return {
  //     spotifyToken: 'asyncData'
  //   }
  // },
  data() {
    return {
      spotifyCode: '',
      // spotifyToken: 'data',
      spotifyClientId: process.env.NUXT_ENV_SPOTIFY_CLIENT_ID,
      spotifyClientSecret: process.env.NUXT_ENV_SPOTIFY_CLIENT_SECRET,
      spotifyRedirectUri: process.env.NUXT_ENV_SPOTIFY_REDIRECT_URI,
      spotifyScopes: process.env.NUXT_ENV_SPOTIFY_SCOPES
    }
  },
  computed: {
    curl() {
      return `
          curl -d grant_type=authorization_code -d code=${this.spotifyCode} -d redirect_uri=${this.spotifyRedirectUri} -d client_id=${this.spotifyClientId} -d client_secret=${this.spotifyClientSecret} ${TOKEN_ENDPOINT}
        `
    }
  },
  mounted() {
    this.spotifyCode = this.$route.query.code
    this.login()
    // this.getToken()
  },
  methods: {
    login() {
      const url = `${AUTHORIZE_ENDPOINT}?response_type=code&client_id=${
        this.spotifyClientId
      }&scope=${encodeURIComponent(
        this.spotifyScopes
      )}&redirect_uri=${encodeURIComponent(this.spotifyRedirectUri)}`

      window.location = url
    },
    getToken() {
      const authOptions = {
        url: TOKEN_ENDPOINT,
        form: {
          code: this.spotifyCode,
          redirect_uri: this.spotifyRedirectUri,
          grant_type: 'authorization_code'
        },
        headers: {
          Authorization:
            'Basic ' +
            Buffer.from(
              this.spotifyClientId + ':' + this.spotifyClientSecret
            ).toString('base64')
        },
        json: true
      }
      this.$axios(authOptions, (error, response, body) => {
        if (error) {
          this.spotifyToken = 'errorrrr'
        }
        this.spotifyToken = response
      })
      // const bearer = Buffer.from(
      //   this.spotifyClientId + ':' + this.spotifyClientSecret
      // ).toString('base64')
      // const response = await this.$axios.$post(
      //   TOKEN_ENDPOINT,
      //   {
      //     grant_type: 'authorization_code',
      //     code: this.spotifyCode,
      //     redirect_uri: this.spotifyRedirectUri
      //   },
      //   {
      //     headers: {
      //       Accept: 'application/json',
      //       Authorization: `Basic ${bearer}`,
      //       'Content-Type': 'application/x-www-form-urlencoded'
      //     }
      //   }
      // )
      // this.spotifyToken = response
    }
  }
}
</script>
<style>
.code {
  margin: 2rem;
  color: #222;
  font-size: 1.6rem;
  background-color: #fff;
}
</style>
