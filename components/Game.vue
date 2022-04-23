<!-- Please remove this file from your project -->
<template>
  <div class="game-overlay">
    <div class="score">Score: {{ score }}</div>
    <div class="controls">
      <div class="collumn">
        <drop-button
          v-for="item in dropButtons"
          :key="item"
          :ref="item"
          @tapped="tapped($event)"
        ></drop-button>
        <div ref="KeyW" class="button" @click="tap()">
          A
          <div v-if="showBlast" ref="blast" class="blast">
            <span>
              {{ blastClass }}
            </span>
            <svg viewBox="0 0 1144.000000 1280.000000" :class="blastClass">
              <g
                transform="translate(0.000000,1280.000000) scale(0.100000,-0.100000)"
                fill="currentColor"
                stroke="none"
              >
                <path
                  d="M829 12748 c35 -80 208 -473 1050 -2377 463 -1046 841 -1904 839
-1905 -2 -2 -149 25 -328 60 -1104 213 -2003 385 -2005 382 -1 -2 375 -619
836 -1373 461 -753 839 -1371 839 -1371 0 -1 -461 -606 -1025 -1345 -564 -739
-1025 -1347 -1025 -1350 0 -4 199 34 443 83 2948 595 2827 571 2834 565 2 -3
-48 -915 -112 -2028 -64 -1112 -115 -2035 -113 -2049 2 -22 213 259 1203 1611
660 901 1202 1638 1205 1638 3 1 392 -572 865 -1273 473 -701 862 -1272 864
-1271 3 4 65 519 271 2250 55 468 104 864 108 882 l7 32 1725 -799 c949 -440
1740 -807 1759 -815 33 -16 34 -16 20 1 -8 10 -560 675 -1227 1478 -667 802
-1211 1460 -1210 1462 2 1 296 7 653 13 1486 26 1759 30 1938 33 103 1 187 4
186 7 0 3 -516 339 -1146 746 -630 407 -1155 747 -1166 756 -19 14 15 59 849
1108 478 601 871 1097 872 1102 3 6 -28 8 -85 4 -48 -3 -225 -10 -393 -15
-168 -6 -435 -15 -595 -21 -159 -5 -587 -20 -950 -33 -363 -13 -661 -23 -663
-21 -1 1 361 753 804 1671 444 918 826 1711 851 1763 l43 93 -62 -53 c-2125
-1842 -3084 -2666 -3089 -2656 -4 7 -87 217 -184 467 -349 897 -795 2040 -892
2289 l-57 144 -42 -149 c-23 -82 -122 -432 -219 -779 -97 -346 -222 -790 -276
-985 -55 -195 -137 -485 -181 -645 -45 -159 -84 -293 -87 -297 -4 -4 -892 672
-1975 1502 -1082 831 -1969 1510 -1971 1510 -3 0 4 -19 14 -42z"
                />
              </g>
            </svg>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>
<style lang="scss" scoped>
.game-overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  left: 0;
  top: 0;
  z-index: 10;
}
.controls {
  position: absolute;
  bottom: 40px;
  left: 0;
  width: 100%;
  z-index: 10;
  display: flex;
  justify-content: space-around;
}
.collumn {
  position: relative;
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
  position: relative;

  .blast {
    width: 100px;
    height: 112px;
    position: absolute;
    bottom: 100%;
    left: 100%;
    color: green;
    text-shadow: 0 0 3px #fff;
    opacity: 0;
    transform: scale(0);
    transition: all 0.2s ease;
    display: flex;
    justify-content: center;
    align-items: center;
    pointer-events: none;
    animation: blast 0.3s ease;

    &.tapped {
    }

    span {
      z-index: 2;
      position: relative;
    }

    svg {
      position: absolute;
      width: 100%;
      height: 100%;

      &.perfect {
        color: red;
      }
      &.good {
        color: orangered;
      }
      &.bad {
        color: yellow;
      }
      &.miss {
        color: blue;
      }
    }
  }
}
.score {
  color: #fff;
  font-size: 30px;
}
@keyframes blast {
  0% {
    opacity: 0;
    transform: scale(0);
  }
  99% {
    opacity: 1;
    transform: scale(1);
  }
  100% {
    opacity: 0;
  }
}
</style>
<script>
import DropButton from "./DropButton.vue";
export default {
  components: { DropButton },
  data() {
    return {
      score: 0,
      config: {
        score: {
          perfect: 100,
          good: 50,
          bad: 10,
          miss: 0,
        },
      },
      blastClass: "",
      showBlast: false,
      dropButtons: [],
    };
  },
  mounted() {
    setInterval(() => {
      const rand = Math.floor(Math.random() * 2);
      if (rand) {
        this.addDrop();
      }
    }, 1000);
  },
  methods: {
    drop() {},
    tap() {
      const id = this.dropButtons[0];
      const btn = this.$refs[id];
      if (btn && btn[0]) {
        btn[0]?.tap();
      }
    },
    tapped(event) {
      this.score += this.config.score[event] || 0
      this.$emit('dance', event)
      if (event !== 'too-soon') {
        this.dropButtons.shift();
      }
      this.blastClass = event !== 'too-soon' ? event : 'miss' ;
      this.showBlast = false
      this.$nextTick(() => {
        this.showBlast = true
      })
    },
    addDrop() {
      this.dropButtons.push(this.makeid(10));
    },
    makeid(length) {
      const timestamp = Date.now();
      let code = "";
      const characters =
        "ABCDEFGHIJKLMNOPQRSTUVWXYZabcdefghijklmnopqrstuvwxyz0123456789";
      const charactersLength = characters.length;
      for (let i = 0; i < length; i++) {
        code += characters.charAt(Math.floor(Math.random() * charactersLength));
      }
      return timestamp + code;
    },
  },
};
</script>
