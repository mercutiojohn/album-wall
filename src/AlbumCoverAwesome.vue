<template>
  <div ref="container" class="tilt-container" :class="{ flipped: isFlipped }">
    <div class="tilt-wrapper" ref="wrapper" :class="{ flipped: isFlipped }">
      <div class="tilt-div" ref="div">
        <div class="back"></div>
        <div class="front">
          <div class="album-cover-image">
            <img :src="cover" alt="Album Cover" />
          </div>
        </div>
        <div class="overlay"></div>
      </div>
    </div>
    <div class="tilt-flipped" ref="flipped" :class="{ flipped: isFlipped }">
      <div class="album-info">
        <h3>{{ title }}</h3>
        <p>{{ artist }}</p>
      </div>
      <div class="close" @click.stop="handleClose">
        <span class="close-icon">x</span>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "AlbumCover",
  props: {
    cover: {
      type: String,
      required: true
    },
    title: {
      type: String,
      required: true
    },
    artist: {
      type: String,
      required: true
    },
    isFlipped: {
      type: Boolean,
      required: false,
      default: false
    }
  },
  data() {
    return {
      containerEl: null,
      wrapperEl: null,
      divEl: null,
      overlayEl: null,
      //
      containerRect: "",
      mouseX: "",
      mouseY: "",
      //
      tiltX: "",
      tiltY: "",
      //
      wrapperRect: "",
      wrapperCenterX: "",
      wrapperCenterY: "",
      //
      translateX: "",
      translateY: ""
    };
  },
  mounted() {
    this.containerEl = this.$refs.container;
    this.wrapperEl = this.$refs.wrapper;
    this.flippedEl = this.$refs.flipped;
    this.divEl = this.$refs.div;
    this.overlayEl = this.divEl.querySelector(".overlay");

    this.containerEl.addEventListener("mousemove", this.handleMouseMove);
    this.containerEl.addEventListener("mousedown", this.handleMouseDown);
    this.containerEl.addEventListener("mouseup", this.handleMouseUp);
    this.containerEl.addEventListener("mouseout", this.handleMouseOut);
  },
  beforeUnmount() {
    this.containerEl.removeEventListener("mousemove", this.handleMouseMove);
    this.containerEl.removeEventListener("mouseout", this.handleMouseOut);
  },
  watch: {
    isFlipped() {
      this.handleMouseOut();
    }
  },
  methods: {
    calculate(event) {
      this.containerRect = this.containerEl.getBoundingClientRect();
      this.mouseX = event.clientX - this.containerRect.left;
      this.mouseY = event.clientY - this.containerRect.top;

      this.tiltX = (this.mouseY / this.containerRect.height - 0.5) * 10;
      this.tiltY = (this.mouseX / this.containerRect.width - 0.5) * 10;

      // this.wrapperEl.style.transform = `rotateX(${this.tiltX}deg) rotateY(${this.tiltY}deg) scale(0.97)`;
      // this.overlayEl.style.transform = `translate(${this.mouseX}px, ${this.mouseY}px)`;

      // this.wrapperRect = this.wrapperEl.getBoundingClientRect();
      // this.wrapperCenterX = this.wrapperRect.left + this.wrapperRect.width / 2;
      // this.wrapperCenterY = this.wrapperRect.top + this.wrapperRect.height / 2;

      // this.translateX = this.mouseX - this.wrapperCenterX;
      // this.translateY = this.mouseY - this.wrapperCenterY;

      // this.overlayEl.style.transformOrigin = `${-translateX}px ${-translateY}px`;
      // this.overlayEl.style.transform = `translate(${translateX}px, ${translateY}px)`;
    },
    handleMouseMove(event) {
      if (!this.isFlipped) {
        this.calculate(event);

        this.wrapperEl.style.transform = `rotateX(${-this.tiltX}deg) rotateY(${
          this.tiltY
        }deg) scale(1.02)`;
        this.overlayEl.style.transform = `translate(${this.mouseX}px, ${
          this.mouseY
        }px)`;

        this.overlayEl.style.opacity = "1";

        // this.wrapperEl.style.transformStyle = "flat";
      }
    },
    handleMouseDown(event) {
      if (!this.isFlipped) {
        this.calculate(event);

        this.wrapperEl.style.transform = `rotateX(${-this.tiltX *
          1.8}deg) rotateY(${this.tiltY * 1.8}deg)`;
        this.overlayEl.style.transform = `translate(${this.mouseX}px, ${
          this.mouseY
        }px)`;
      }
    },
    handleMouseUp(event) {
      if (!this.isFlipped) {
        this.calculate(event);

        this.wrapperEl.style.transform = `rotateX(${-this.tiltX}deg) rotateY(${
          this.tiltY
        }deg) scale(1.02)`;
        this.overlayEl.style.transform = `translate(${this.mouseX}px, ${
          this.mouseY
        }px)`;
      }
    },
    handleMouseOut() {
      if (!this.isFlipped) {
        this.wrapperEl.style.transform = "";
        // this.overlayEl.style.transform = ``;
        this.overlayEl.style.opacity = "0";
      }
    },
    handleClose() {
      console.log("[before-emit]");
      this.$emit("close");
    }
  }
};
</script>

<style scoped>
.tilt-container {
  border-radius: 15px;
  transform-style: preserve-3d;
  transform: perspective(800px);
  /* width: 300px;
  height: 300px; */
  width: 100%;
  height: 100%;
  position: relative;
}
.tilt-container:hover {
  z-index: 50;
}
.tilt-container.flipped {
  /* position: absolute; */
  z-index: 100;
}
.tilt-wrapper,
.tilt-flipped {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  border-radius: 15px;
  /* transition: all 0.3s ease 0s; */
  overflow: hidden;
  background: #000;
  backface-visibility: hidden;
  transition: transform 0.8s cubic-bezier(0.075, 0.82, 0.165, 1),
    opacity 0.6s ease 0.2s, box-shadow 0.4s ease, filter 0.9s ease;
}
.tilt-wrapper {
  cursor: pointer;
  box-shadow: rgba(14, 21, 47, 0.6) 0px 3px 5px -2px;
}
.tilt-flipped {
  box-shadow: rgba(14, 21, 47, 0.6) 0px 3px 5px -2px;
}
.tilt-wrapper:hover {
  box-shadow: rgba(14, 21, 47, 0.5) 0px 12px 30px -3px;
  filter: saturate(180%);
}
.tilt-wrapper:active {
  box-shadow: rgba(14, 21, 47, 0.3) 0px 6px 10px -2px;
}
.tilt-div {
  position: absolute;
  width: 100%;
  height: 100%;
  /* transition: all 0.2s ease-out 0s; */
  border-radius: 15px;
}

/* flip */
.tilt-wrapper {
  transform: rotateY(0deg);
}
.tilt-wrapper.flipped {
  /* opacity: 0; */
  transform: rotateY(-180deg) scale(1.25) !important;
}
.tilt-flipped {
  /* opacity: 0; */
  transform: rotateY(180deg);
}

.tilt-flipped.flipped {
  /* opacity: 1; */
  transform: rotateY(0deg) scale(1.25);
  box-shadow: rgba(14, 21, 47, 0.6) 0px 16px 40px -2px;
  z-index: 100;
}
.tilt-flipped.flipped:active {
  transform: rotateY(0deg) scale(1.23);
  /* box-shadow: 0 6px 20px 2px #00000043; */
}

.front {
  position: relative;
  width: 100%;
  height: 100%;
  border-radius: 15px;
  overflow: hidden;
  transform-style: preserve-3d;
}

.back {
  position: absolute;
  inset: 0px;
  border-radius: 15px;
  background: linear-gradient(
    135deg,
    rgba(255, 255, 255, 0.25) 0%,
    rgba(255, 255, 255, 0) 60%
  );
  background: #000;
}
.overlay:before {
  /* content: "";
  position: absolute;
  top: -50%;
  left: -50%;
  width: 200%;
  height: 200%;
  background: radial-gradient(
    ellipse farthest-corner at center,
    rgba(255, 255, 255, 0.8) 0%,
    rgba(255, 255, 255, 0) 70%
  );
  transform: scale(0);
  transition: transform 0.3s ease-out; */
}

.overlay:hover:before {
  /* transform: scale(1); */
}
.overlay {
  position: absolute;
  /* inset: 0px; */
  /* border-radius: 15px; */
  /* background: rgba(0, 0, 0, 0.4); */
  opacity: 0;
  top: -400px;
  left: -400px;
  /* width: 200%;
  height: 200%;
  top: 0;
  left: 0; */
  width: 800px;
  height: 800px;
  background: radial-gradient(
    ellipse farthest-corner at center,
    rgba(255, 255, 255, 0.2) 0%,
    rgba(255, 255, 255, 0) 60%
  );
  /* backdrop-filter: saturate(180%); */
  /* background: #ABC; */
  /* transform: scale(0); */
  transition: opacity 0.3s ease-out, transform 0.01s ease 0s;
  /* , transform 0.3s ease-out */
  /* pointer-events: none; */
}
.overlay:hover {
  /* transform: scale(1); */
  /* opacity: 0.8; */
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
.tilt-flipped .close {
  position: absolute;
  right: 10px;
  top: 10px;
  width: 40px;
  height: 40px;
  background: #ffffff23;
  color: #fff;
  user-select: none;
  cursor: pointer;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  transition: background 0.2s ease, color 0.2s ease,
    transform 0.5s cubic-bezier(0.075, 0.82, 0.165, 1);
}
.tilt-flipped .close:hover {
  background: #ffffff78;
  color: #000;
}
.tilt-flipped .close:active {
  background: #ffffff;
  transform: scale(1.12);
}
</style>
