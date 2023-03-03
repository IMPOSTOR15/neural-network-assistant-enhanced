<template>
  <section>
    <h2 class="choose-header">Select functional</h2>
    <div>
      <button ref="firstRippleButton" class="select-btn" @click="onClickselect" @animationend="onAnimationEnd">chatGPT</button>
      <button ref="firstRippleButton" class="select-btn" @click="onClickselect" @animationend="onAnimationEnd">chatGPT V2</button>
      <!-- <button ref="secondRippleButton" class="select-btn" @click="onClickselect" @animationend="onAnimationEnd">Stable diffusion</button> -->
    </div>
    <div class="content">
      <ChatGPT/>
    </div>
  </section>
</template>

<script>
import ChatGPT from '@/components/ChatGPTCompanent.vue'
// import StableDiffusion from './StableDiffusion.vue';
export default {
  components: {
    ChatGPT,

  },
  data() {
    return {
      rippleButton: null,
    }
  },
  mounted() {
  },
  methods: {
    onClickselect(event) {
      if (event.path[0].textContent === "Stable diffusion") {
        this.mousePositionToCustomProp(event, this.$refs.secondRippleButton);
        this.$refs.secondRippleButton.classList.add("pulse");
      }
      if (event.path[0].textContent === "chatGPT") {
        this.mousePositionToCustomProp(event, this.$refs.firstRippleButton);
        this.$refs.firstRippleButton.classList.add("pulse");
      }
      if (event.path[0].textContent === "chatGPT") {
        this.mousePositionToCustomProp(event, this.$refs.firstRippleButton);
        this.$refs.firstRippleButton.classList.add("pulse");
      }
      
      
    },
    onAnimationEnd(event) {
      if (event.path[0].textContent === "Stable diffusion") {
        this.$refs.secondRippleButton.classList.remove("pulse");
      }
      if (event.path[0].textContent === "chatGPT") {
        this.$refs.firstRippleButton.classList.remove("pulse");
      }
      
    },
    mousePositionToCustomProp(event, element) {
      let posX = event.offsetX;
      let posY = event.offsetY;

      element.style.setProperty("--x", posX + "px");
      element.style.setProperty("--y", posY + "px");
    },

  }
}
</script>

<style>
.select-btn {
  display: inline-grid;
  place-items: center;
  position: relative;
  isolation: isolate;
  appearance: none;
  cursor: pointer;
  font-size: 1.5rem;
  padding: 0.5em 0.5em;
  text-transform: uppercase;
  background-color: transparent;
  color: hsl(200 100% 65%);
  border: 5px solid currentColor;
  border-radius: 0.125em;
  overflow: hidden;
  min-width: 200px;
  max-width: 400px;
  margin: 0 20px;
}

.select-btn::before {
  content: "";
  position: absolute;
  top: var(--y);
  left: var(--x);
  transform: translate(-50%, -50%) scale(0);
  transition: transform 750ms;
  z-index: -1;
  width: 150%;
  aspect-ratio: 1 / 1;
  border-radius: 50%;
  background: #fff;
  opacity: 0.5;
}

.pulse::before {
  animation: pulse 500ms;
}

@keyframes pulse {
0% {
  transform: translate(-50%, -50%) scale(0);
  opacity: 0.5;
}
100% {
  transform: translate(-50%, -50%) scale(1);
  opacity: 0;
}
}
</style>