<template>
  <nav class="nav">
    <ul>
      <li class="nav-item">
        <nuxt-link to="/">Accueil (temp)</nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link to="/albums">Albums</nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link class="nav-item" to="/playlists">Playlists</nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link to="/search">Rechercher</nuxt-link>
      </li>
    </ul>
  </nav>
</template>

<style lang="scss">
nav {
  > ul {
    list-style: none;
  }

  &-item a {
    color: $colorText;
  }
}
</style>
