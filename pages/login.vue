<template>
  <div>
    <h1>Login</h1>
    <div>
      <h2>Authorization code</h2>
      <code>{{ authorizationCode }}</code>
    </div>
    <div>
      <h2>Access Token</h2>
      <code>{{ token }}</code>
    </div>
  </div>
</template>
<script>
// TODO move in function
import querystring from 'querystring'

const AUTHORIZE_ENDPOINT = 'https://accounts.spotify.com/authorize'
const TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token'

export default {
  async fetch() {
    this.authorizationCode = this.$route.query.code

    if (this.authorizationCode) {
      await this.getToken(this.authorizationCode)
    } else {
      this.login()
    }
  },
  data() {
    return {
      authorizationCode: '',
      token: ''
    }
  },
  methods: {
    login() {
      const url = `${AUTHORIZE_ENDPOINT}?response_type=code&client_id=${
        process.env.NUXT_ENV_SPOTIFY_CLIENT_ID
      }&scope=${encodeURIComponent(
        process.env.NUXT_ENV_SPOTIFY_SCOPES
      )}&redirect_uri=${encodeURIComponent(
        process.env.NUXT_ENV_SPOTIFY_REDIRECT_URI
      )}`

      window.location = url
    },
    async getToken(authorizationCode) {
      const data = {
        code: authorizationCode,
        redirect_uri: process.env.NUXT_ENV_SPOTIFY_REDIRECT_URI,
        grant_type: 'authorization_code',
        client_id: process.env.NUXT_ENV_SPOTIFY_CLIENT_ID,
        client_secret: process.env.NUXT_ENV_SPOTIFY_CLIENT_SECRET
      }
      const options = {
        method: 'POST',
        headers: {
          Accept: 'application/json',
          'Content-Type': 'application/x-www-form-urlencoded',
          Authorization:
            'Basic ' +
            Buffer.from(
              process.env.NUXT_ENV_SPOTIFY_CLIENT_ID +
                ':' +
                process.env.NUXT_ENV_SPOTIFY_CLIENT_SECRET
            ).toString('base64')
        },
        data: querystring.stringify(data),
        url: TOKEN_ENDPOINT
      }

      const instance = this
      await this.$axios(options)
        .then(function(response) {
          instance.token = {
            access_token: response.data.access_token,
            token_type: response.data.token_type,
            scope: response.data.scope,
            expires_in: response.data.expires_in,
            refresh_token: response.data.refresh_token
          }
        })
        .catch(function(error) {
          instance.token = error.toJSON()
        })
    }
  }
}
</script>
