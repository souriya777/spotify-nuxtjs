<template>
  <nav class="nav">
    <div class="nav-header">
      <nuxt-link to="/" class="nav-logo">
        <logo-icon />
      </nuxt-link>
    </div>
    <ul class="nav-list">
      <li class="nav-item">
        <nuxt-link to="/">
          <span class="nav-item_icon">
            <home-icon />
          </span>
          <span class="nav-item_label">Accueil</span>
        </nuxt-link>
      </li>
      <li class="nav-item">
        <nuxt-link to="/albums">
          <span class="nav-item_icon">
            <album-icon />
          </span>
          <span class="nav-item_label">Albums</span></nuxt-link
        >
      </li>
      <li class="nav-item">
        <nuxt-link to="/playlists">
          <span class="nav-item_icon">
            <playlist-icon />
          </span>
          <span class="nav-item_label">Playlists</span></nuxt-link
        >
      </li>
      <li class="nav-item">
        <nuxt-link to="/search">
          <span class="nav-item_icon">
            <search-icon />
          </span>
          <span class="nav-item_label">Rechercher</span>
        </nuxt-link>
      </li>
    </ul>
  </nav>
</template>
<script>
import LogoIcon from './icons/LogoIcon'
import HomeIcon from './icons/HomeIcon'
import AlbumIcon from './icons/AlbumIcon'
import PlaylistIcon from './icons/PlaylistIcon'
import SearchIcon from './icons/SearchIcon'
export default {
  components: { LogoIcon, HomeIcon, AlbumIcon, PlaylistIcon, SearchIcon }
}
</script>
<style lang="scss">
.nav {
  padding-top: 2.4rem;

  &-header {
    padding-left: 2.4rem;
    padding-bottom: 1.8rem;
    background-color: inherit;
  }

  &-logo {
    color: $colorText;
  }

  &-list {
    width: 100%;
    padding: 0;
    list-style: none;
  }

  &-item {
    padding: 0 0.8rem;
    font-size: $fontSizeButton;
    font-weight: 700;

    &_label {
      margin-left: 1.6rem;
    }

    > a {
      height: 4rem;
      display: flex;
      align-items: center;
      padding: 0 1.6rem;
      border-radius: 0.4rem;
      color: $colorTextInactive;
      text-decoration: none;
    }

    > .nuxt-link-exact-active {
      background-color: $colorButtonActive;
      color: $colorText;
    }
  }
}
</style>
