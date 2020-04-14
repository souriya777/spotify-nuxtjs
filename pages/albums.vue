<template>
  <div>
    <h1>Albums</h1>
    <div style="margin-bottom: 2rem">
      <h2>token</h2>
      <code>{{ accessToken }}</code>
      <div>
        <button @click="getAlbums">ALBUMS</button>
      </div>
      <div v-if="json">
        <h2>JSON</h2>
        <!-- eslint-disable-next-line vue/no-textarea-mustache -->
        <textarea id="myCode">{{ json }}</textarea>
        <button @click="copy">copy</button>
      </div>
    </div>
    <div class="album_list">
      <nuxt-link class="album_item" to="/album/55bxux5NGwsurOUlXfT2cv">
        <img
          class="album_img"
          src="https://i.scdn.co/image/ab67616d0000b27340da74331c45f848b2090f4d"
          alt="Pasaporte - Alexander Abreu, Havana D’Primera"
        />
        <div class="album_title">Pasaporte</div>
        <div class="album_interpreters">
          <nuxt-link
            class="album_interpreter"
            to="/artist/2YnskQkgb6kTSXh9YcNzgu"
            >Alexander Abreu</nuxt-link
          >,
          <nuxt-link
            class="album_interpreter"
            to="/artist/05qiwKzU1RgkyqkEH1ZFlA"
            >Havana D’Primera</nuxt-link
          >
        </div>
        <div class="album_play">
          <play-button />
        </div>
      </nuxt-link>
    </div>
  </div>
</template>
<script>
import gql from 'graphql-tag'
import PlayButton from '../components/PlayButton'

export default {
  apollo: {
    albums: gql`
      query getAlbums {
        albums {
          items
        }
      }
    `
  },
  components: { PlayButton },
  filters: {
    stringify(value) {
      return JSON.stringify(value)
    }
  },
  data() {
    return {
      accessToken: this.$apolloHelpers.getToken() + '->testa',
      json: ''
    }
  },
  methods: {
    copy(e) {
      const json = document.querySelector('#myCode')
      json.select()
      document.execCommand('copy')
    },
    async getAlbums() {
      this.json = await this.$axios.get(
        'https://api.spotify.com/v1/me/albums',
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
<style lang="scss">
$border-interpreter: 0.1rem solid;
.album {
  &_list {
    display: grid;
    column-gap: 1.6rem;
    grid-template-columns: repeat(auto-fill, minmax(16.4rem, 1fr));
    grid-auto-rows: 1fr;
  }
  &_item {
    position: relative;
    display: flex;
    flex-direction: column;
    width: 18.4rem;
    padding: 2rem;
    border-radius: 0.8rem;
    background-color: $colorButtonActive;
    text-decoration: none;
  }
  &_img {
    width: 100%;
    padding-bottom: 1.6rem;
  }
  &_title {
    height: 2rem;
    color: $colorText;
    font-weight: 700;
    letter-spacing: 0.024rem;
  }
  &_interpreters {
    color: $colorTextInactive;
  }
  &_interpreter {
    border-bottom: $border-interpreter transparent;
    color: $colorTextInactive;
    font-size: 1.2rem;
    font-weight: 400;
    letter-spacing: 0.021rem;
    line-height: 1.8rem;
    text-decoration: none;

    &:hover {
      border-bottom: $border-interpreter;
      transition: border 0.2s linear;
    }
  }
  &_play {
    position: absolute;
    bottom: 1.6rem;
    right: 1.6rem;
    display: none;
  }

  &_item:hover &_play {
    display: block;
  }
}
</style>
