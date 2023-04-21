<template>
  <!-- <transition-group
    name="grid"
    tag="div"
    class="album-cover-wall"
    @click="handleClickBack()"
  > -->
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
  <!-- </transition-group> -->
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
      required: true
    }
  },
  data() {
    return {
      flippedIndex: -1
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
    }
  }
};
</script>

<style>
/* .album-cover-wall {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 20px;
} */
.album-cover-wall {
  --cover-size: 300px;
  --gap-width: 20px;
  display: grid;
  /* grid-template-columns: repeat(auto-fill, minmax(300px, 1fr)); */
  grid-template-columns: repeat(auto-fill, var(--cover-size));
  grid-auto-rows: var(--cover-size);
  gap: var(--gap-width);
  width: 100%;
  padding-top: 30px;
  justify-content: center;
  transition: grid-template-columns 0.2s ease, grid-auto-rows 0.2s ease;
}
.grid-enter-active,
.grid-leave-active,
.grid-move {
  transition: transform 0.5s ease-out;
}

.grid-enter,
.grid-leave-active {
  transform: translateY(50%);
}
@media screen and (max-width: 800px) {
  .album-cover-wall {
    --cover-size: 200px;
    --gap-width: 10px;
  }
}
@media screen and (max-width: 500px) {
  .album-cover-wall {
    --cover-size: calc((100vw - 50px) / 2);
    --gap-width: 10px;
  }
}
</style>
