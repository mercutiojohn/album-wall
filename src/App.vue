<template>
  <div id="app">
    <AlbumWall :albums="albums" />
  </div>
</template>

<script>
import AlbumWall from "@/AlbumWallSimple";
export default {
  name: "App",
  components: {
    AlbumWall
  },
  data() {
    return {
      albums: [
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=1"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=2"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=3"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=4"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=5"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=6"
        },
        {
          title: "专辑标题",
          artist: "艺术家名称",
          cover: "https://picsum.photos/200/200?random=7"
        }
      ]
    };
  }
};
</script>

<style>
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  /* -------------- */
  display: flex;
}
</style>
