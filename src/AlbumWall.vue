<template>
  <div class="album-cover-wall" @click="handleClickBack(index)">
    <div
      v-for="(album, index) in albums"
      :key="index"
      class="album-cover"
      :class="{ flipped: flippedIndex === index }"
      @click.stop="handleClickAlbum(index)"
    >
      <div
        class="album-cover-front"
        :class="{ flipped: flippedIndex === index }"
      >
        <div class="album-cover-image">
          <img :src="album.cover" alt="Album Cover" />
        </div>
      </div>
      <div
        class="album-cover-back"
        :class="{ flipped: flippedIndex === index }"
      >
        <div class="album-info">
          <h3>{{ album.title }}</h3>
          <p>{{ album.artist }}</p>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AlbumCoverWall",
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
    handleClickBack(index) {
      this.flippedIndex = -1;
    }
  }
};
</script>

<style scoped>
.album-cover-wall {
  display: flex;
  flex-wrap: wrap;
  justify-content: center;
  gap: 10px;
}

.album-cover {
  width: 300px;
  height: 300px;
  position: relative;
  perspective: 800px;
}

.album-cover.flipped {
  /* position: absolute; */
  z-index: 100;
}

.album-cover-front,
.album-cover-back {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  backface-visibility: hidden;
  transition: transform 0.8s cubic-bezier(0.075, 0.82, 0.165, 1),
    opacity 0.6s ease 0.2s, box-shadow 0.8s ease;
  border-radius: 10px;
  box-shadow: 0 6px 20px 2px #00000043;
  overflow: hidden;
}

.album-cover-front {
  transform: rotateY(0deg);
}
.album-cover-front:hover {
  transform: rotateY(0deg) scale(1.08);
}
.album-cover-front:active {
  transform: rotateY(0deg) scale(0.97);
}
.album-cover-front.flipped {
  /* opacity: 0; */
  transform: rotateY(-180deg) scale(1.25);
}
.album-cover-back {
  /* opacity: 0; */
  transform: rotateY(180deg);
}

.album-cover-back.flipped {
  /* opacity: 1; */
  transform: rotateY(0deg) scale(1.25);
  box-shadow: 0 12px 50px 2px #00000053;
}
.album-cover-back:active {
  transform: scale(1.23);
  /* box-shadow: 0 6px 20px 2px #00000043; */
}

.album-cover-image {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  transform-style: preserve-3d;
}

.album-cover-image img {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  backface-visibility: hidden;
}

.album-info {
  display: flex;
  flex-direction: column;
  justify-content: center;
  gap: 10px;
  position: absolute;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  padding: 10px;
  /* background-color: rgba(0, 0, 0, 0.7); */
  background-color: #000;
  color: #ffffff;
  font-size: 14px;
  line-height: 1.4;
  text-align: center;
  transform: translateZ(50px);
}

.album-info h3 {
  margin: 0;
  font-size: 18px;
  font-weight: bold;
  line-height: 1.2;
}

.album-info p {
  margin: 0;
  font-size: 14px;
  font-weight: normal;
  line-height: 1.2;
}
</style>
