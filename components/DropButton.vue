<!-- Please remove this file from your project -->
<template>
  <div
    class="drop-button"
    :style="`transform: translateY(${y}px)`"
    @click="tap()"
  >
    A
  </div>
</template>
<style lang="scss" scoped>
.drop-button {
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
  position: absolute;
  left: 0;
  bottom: 0;
  opacity: 1;
}
// .tapped {
//   opacity: 0;
// }
</style>
<script>
export default {
  data() {
    return {
      y: -500,
      tapped: false,
    };
  },
  mounted() {
    this.drop();
  },
  methods: {
    drop() {
      if (this.y < 0 && !this.tapped) {
        requestAnimationFrame(() => {
          const target = this.y + 4;
          this.y = target > 0 ? 0 : target;
          this.drop();
        });
      } else if (this.y >= 0 && !this.tapped) {
        this.$emit("tapped", "miss");
      }
    },
    tap() {
      if (this.y > -10) {
        this.tapped = true;
        this.$emit("tapped", "perfect");
        return;
      }
      if (this.y > -50) {
        this.tapped = true;
        this.$emit("tapped", "good");
        return;
      }
      if (this.y > -100) {
        this.tapped = true;
        this.$emit("tapped", "bad");
        return;
      }
      this.$emit("tapped", "too-soon");
    },
  },
};
</script>
