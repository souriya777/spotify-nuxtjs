<template>
  <div class="app">
    <Nav />
    <div class="browser">
      <nuxt />
    </div>
    <Player />
  </div>
</template>

<script>
import Nav from '../components/Nav'
import Player from '../components/Player'

export default {
  components: { Nav, Player }
}
</script>

<style lang="scss">
html {
  font-family: 'Source Sans Pro', -apple-system, BlinkMacSystemFont, 'Segoe UI',
    Roboto, 'Helvetica Neue', Arial, sans-serif;
  font-size: 62.5%;
  word-spacing: 0.1rem;
  -ms-text-size-adjust: 100%;
  -webkit-text-size-adjust: 100%;
  -moz-osx-font-smoothing: grayscale;
  -webkit-font-smoothing: antialiased;
  box-sizing: border-box;
}

*,
*:before,
*:after {
  box-sizing: border-box;
  margin: 0;
}

.app {
  display: grid;
  grid-template-columns: 23rem 1fr;
  grid-template-rows: 1fr 9.1rem;
  grid-template-areas:
    'nav browser'
    'player player';
  height: 100vh;
  background-color: $colorBackground;
  color: $colorText;
}

.nav {
  grid-area: nav;
}
.browser {
  grid-area: browser;
  padding: 0 3.2rem;
  background-color: $colorBackground2;
  overflow-y: scroll;
  font-size: $fontSizeContent;
}
.player {
  grid-area: player;
}

// GENERIC COMPONENT
.section {
  padding: 1.6rem 0;
}
</style>
