<template>
  <div>
    <a-scene>
      <a-assets>
        <img id="sky" src="images/59c90f262e7bef8a1f000214.png" alt="" />
        <a-asset-item id="groot" src="model/groot.gltf"></a-asset-item>
      </a-assets>
      <a-sky src="#sky"></a-sky>
      <!-- <a-entity gltf-model="#mastercheif"></a-entity> -->
      <!-- <a-entity
        id="groot-model"
        gltf-model="url(model/groot.gltf)"
        position="-7 0 0"
        rotation="0 90 0"
        animation-mixer="clip:mixamo.com"
      ></a-entity> -->
      <a-entity
        id="deadpool"
        gltf-model="url(model/deadpool_dancing/scene.gltf)"
        position="-7 0 0"
        rotation="0 90 0"
        animation-mixer="clip:mixamo.com"
      ></a-entity>

      <a-entity rotation="0 90 0">
        <a-camera wasd-controls="enabled:false"></a-camera>
      </a-entity>
    </a-scene>
    <!-- <div class="controls">
      <div ref="KeyW" class="button" @click="onClick('KeyW')">W</div>
      <div ref="KeyA" class="button" @click="onClick('KeyA')">A</div>
      <div ref="KeyS" class="button" @click="onClick('KeyS')">S</div>
      <div ref="KeyD" class="button" @click="onClick('KeyD')">D</div>
    </div> -->
    <game @dance="onDance($event)"></game>
  </div>
</template>
<style scoped>
.controls {
  position: fixed;
  bottom: 40px;
  left: 0;
  width: 100%;
  z-index: 10;
  display: flex;
  justify-content: space-around;
}
.button {
  width: 100px;
  height: 100px;
  border-radius: 100%;
  color: #fff;
  border: 4px solid #fff;
  display: flex;
  justify-content: center;
  align-items: center;
  font-size: 50px;
  font-weight: 700;
  cursor: pointer;
  -webkit-touch-callout: none; /* iOS Safari */
  -webkit-user-select: none; /* Safari */
  -khtml-user-select: none; /* Konqueror HTML */
  -moz-user-select: none; /* Old versions of Firefox */
  -ms-user-select: none; /* Internet Explorer/Edge */
  user-select: none;
}
.button.active {
  color: red;
  border-color: red;
}
</style>
<script>
import Vue from "vue";
import AFRAME from "aframe";
import Game from "../components/Game.vue";
require("aframe-extras");
Vue.use(AFRAME);
export default {
  components: { Game },
  mounted() {
    // console.log(deadpool.components["animation-mixer"].attrValue);
    // document.addEventListener("keypress", (e) => {
    //   this.onClick(e.code);
    // });
  },
  methods: {
    // onClick(key) {
    //   this.$refs[key]?.classList.add("active");
    //   setTimeout(() => {
    //     this.$refs[key]?.classList.remove("active");
    //   }, 100);
    //   const deadpool = this.$el.querySelector("#deadpool");
    //   switch (key) {
    //     case "KeyW":
    //       deadpool.setAttribute("animation-mixer", { clip: "chick" });
    //       return;
    //     case "KeyA":
    //       deadpool.setAttribute("animation-mixer", { clip: "gang" });
    //       return;
    //     case "KeyS":
    //       deadpool.setAttribute("animation-mixer", { clip: "hip" });
    //       return;
    //     case "KeyD":
    //       deadpool.setAttribute("animation-mixer", { clip: "ymca" });
    //       return;
    //     case "Space":
    //       deadpool.setAttribute("animation-mixer", { clip: "mixamo.com" });
    //       return;
    //     default:
    //       return;
    //   }
    // },
    onDance(event) {
      const deadpool = this.$el.querySelector("#deadpool");
      switch (event) {
        case 'miss':
        case 'too-soon':
          deadpool.setAttribute("animation-mixer", { clip: "mixamo.com" });
          break
        default: 
          deadpool.setAttribute("animation-mixer", { clip: "hip" });
          break
      }
    }
  },
};
</script>
