<template>
  <div class="album-cover-wall" @click="handleClickBack()">
    <div
      v-for="(album, index) in albums"
      @click.stop="handleClickAlbum(index)"
      :key="index"
      class="album-clickbox"
    >
      <AlbumCover
        :cover="album.cover"
        :title="album.title"
        :artist="album.artist"
        :isFlipped="isFlipped(index)"
      />
    </div>
  </div>
</template>

<script>
// import AlbumCover from "./AlbumCover.vue";
import AlbumCover from "./AlbumCoverAwesome.vue";

export default {
  name: "AlbumCoverWall",
  components: {
    AlbumCover
  },
  props: {
    albums: {
      type: Array,
      required: true,
    },
  },
  data() {
    return {
      flippedIndex: -1,
    };
  },
  methods: {
    handleClickAlbum(index) {
      this.flippedIndex = index;
    },
    handleClickBack() {
      this.flippedIndex = -1;
    },
    isFlipped(index) {
      return this.flippedIndex === index;
    },
  },
};
</script>

<style>
.album-cover-wall {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
}
</style>
