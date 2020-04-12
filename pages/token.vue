<template>
  <div>
    <h1>Login</h1>
    <div>
      <h2>Access Token</h2>
      <code>{{ token }}</code>
    </div>
  </div>
</template>
<script>
// TODO move in function
import querystring from 'querystring'

const TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token'

export default {
  async fetch() {
    await this.getToken(this.$route.query.code)
  },
  data() {
    return {
      authorizationCode: '',
      token: ''
    }
  },
  methods: {
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
