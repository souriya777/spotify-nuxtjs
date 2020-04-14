<template>
  <div>
    <h1>Login</h1>
    <div>
      <h2>Authorization code</h2>
      <code>{{ authorizationCode }}</code>
    </div>
    <div>
      <h2>Access token</h2>
      <code>{{ accessToken }}</code>
      <h2>Refresh token</h2>
      <code>{{ refreshToken }}</code>
    </div>
    <div>
      <h2>User ID</h2>
      <code>{{ userId }}</code>
    </div>
    <div>
      <h2>Spotify</h2>
      <button @click="getRefreshToken">REFRESH TOKEN</button>
      <button @click="spotifyMe">SPOTIFY ME</button>
      <button @click="getAlbums">ALBUMS</button>
      <button @click="getPlaylists">{{ userId }} PLAYLIST</button>
      <div>
        <code>{{ spotifyResult }}</code>
      </div>
    </div>
  </div>
</template>
<script>
import querystring from 'querystring'

const AUTHORIZE_ENDPOINT = 'https://accounts.spotify.com/authorize'
const TOKEN_ENDPOINT = 'https://accounts.spotify.com/api/token'

export default {
  async fetch() {
    this.authorizationCode = this.$route.query.code

    if (this.authorizationCode) {
      await this.getTokens(this.authorizationCode)
    } else {
      this.authorize()
    }
  },
  data() {
    return {
      authorizationCode: '',
      accessToken: '',
      refreshToken: '',
      spotifyResult: ''
    }
  },
  computed: {
    userId() {
      return this.spotifyResult?.data?.id
    }
  },
  methods: {
    authorize() {
      const url = `${AUTHORIZE_ENDPOINT}?response_type=code&client_id=${
        process.env.NUXT_ENV_SPOTIFY_CLIENT_ID
      }&scope=${encodeURIComponent(
        process.env.NUXT_ENV_SPOTIFY_SCOPES
      )}&redirect_uri=${encodeURIComponent(
        process.env.NUXT_ENV_SPOTIFY_REDIRECT_URI
      )}`
      window.location = url
    },
    async getTokens(authorizationCode) {
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
          instance.accessToken = response.data.access_token
          instance.refreshToken = response.data.refresh_token
        })
        .catch(function(error) {
          instance.token = error.toJSON()
        })

      // FIXME apollo
      await this.$apolloHelpers.onLogin(instance.accessToken, undefined, {
        expires: 7,
        path: '/'
      })
    },
    async getRefreshToken() {
      const data = {
        grant_type: 'refresh_token',
        refresh_token: this.refreshToken
      }
      const options = {
        method: 'POST',
        headers: {
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
          instance.accessToken = response.data.access_token
          instance.refreshToken = response.data.refresh_token
        })
        .catch(function(error) {
          instance.token = error.toJSON()
        })
    },
    async spotifyMe() {
      this.spotifyResult = await this.$axios.get(
        'https://api.spotify.com/v1/me',
        {
          headers: {
            Authorization: 'Bearer ' + this.accessToken
          }
        }
      )
    },
    async getAlbums() {
      this.spotifyResult = await this.$axios.get(
        'https://api.spotify.com/v1/me/albums',
        {
          headers: {
            Authorization: 'Bearer ' + this.accessToken
          }
        }
      )
    },
    async getPlaylists() {
      this.spotifyResult = await this.$axios.get(
        `https://api.spotify.com/v1/users/${this.userId}/playlists`,
        {
          headers: {
            Accept: 'application/json',
            'Content-Type': 'application/json',
            Authorization: 'Bearer ' + this.accessToken
          }
        }
      )
    }
  }
}
</script>
