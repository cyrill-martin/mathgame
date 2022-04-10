<template>
  <teleport to="body">
    <div @click="closePopup" class="backdrop"></div>
    <transition name="video">
      <div class="popUp">
        <menu><span @click="closePopup">&#10006;</span></menu>
        <div style="text-align: center">
          <iframe
            id="ytplayer"
            type="text/html"
            :width="ytSize.w"
            :height="ytSize.h"
            :src="videoUrl"
            frameborder="0"
          ></iframe>
        </div>
      </div>
    </transition>
  </teleport>
</template>

<script>
export default {
  emits: ["close-popup"],
  computed: {
    videoUrl() {
      const videoId =
        this.videoIds[Math.floor(Math.random() * this.videoIds.length)];
      return `https://www.youtube.com/embed/${videoId}?autoplay=1&mute=1&info=0&controls=0&playlist=${videoId}&loop=1&origin=${process.env.VUE_APP_URL}`;
    },
    screenWidth() {
      return screen.width;
    },
    ytSize() {
      if (this.screenWidth < 560) {
        return {
          w: 250,
          h: 500,
        };
      }
      return {
        w: 300,
        h: 600,
      };
    },
  },
  methods: {
    closePopup() {
      this.$emit("close-popup");
    },
  },
  data() {
    return {
      videoIds: [
        "B7G6n87SRh8",
        "ZLknpTK4jHI",
        "Wm3F8kF9WAE",
        "9Ql4H0ko5-w",
        "Afi_7KNHLj8",
        "aVUUsu9DgUg",
        "enQdW2HusLc",
        "2YEGcxt458k",
        "Fx-4DvKC-_k",
        "ezdh-0hxV_I",
        "c3KMgXdKGqE",
        "XrKO7xIM5_M",
      ],
    };
  },
};
</script>

<style scoped>
.backdrop {
  position: fixed;
  top: 0;
  left: 0;
  height: 100vh;
  width: 100%;
  background-color: rgba(0, 0, 0, 0.25);
  z-index: 10;
}
.popUp {
  border-radius: 4px;
  position: fixed;
  top: 5vh;
  left: 25%;
  width: 50%;
  z-index: 100;
  border: none;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.26);
  padding: 1rem;
  margin: 0;
  overflow: hidden;
  background-color: white;
  max-height: 800px;
  min-width: 300px;
  overflow: auto;
}
menu {
  float: right;
  margin: 0;
  font-size: 1rem;
  cursor: pointer;
}

.video-enter-active,
.video-leave-active {
  transition: opacity 0.5s ease;
}

.video-enter-from,
.video-leave-to {
  opacity: 0;
}

@media only screen and (max-width: 560px) {
  .popUp {
    left: 5%;
    min-width: 0;
    width: 80%;
  }
}
</style>
