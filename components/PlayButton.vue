<template>
  <button class="play-button">
    <play-icon class="play-button_icon" />
  </button>
</template>
<script>
import PlayIcon from './icons/PlayIcon'

export default {
  components: { PlayIcon }
}
</script>
<style lang="scss">
.play-button {
  width: 4rem;
  height: 4rem;
  border-radius: 50%;
  color: $colorText;
  border-color: $colorButtonPlay;
  background-color: $colorButtonPlay;
  font-size: 1.2rem;

  &:hover {
    transform: scale(1.06);
    transition: transform 33ms cubic-bezier(0.3, 0, 0, 1);

    & > .play-button_icon {
      font-size: 1.2rem;
    }
  }
}
</style>
