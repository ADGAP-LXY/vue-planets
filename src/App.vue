<template>
  <div class="missions">
    <p class="label">missions</p>
    <h2>Exploring the Universe</h2>
    <p class="text-body-md">
      Spacecraft developed at JPL have flown to <br />
      every planet in the solar system and the Sun, <br />
      and beyond.
    </p>
    <p>
      winWidth: {{ winWidth }}, left: {{ left }}, N: {{ N }}, pointer:
      {{ pointer }}
    </p>
  </div>

  <div
    class="planets"
    :style="{
      left: left + 'px',
      width: planetsWidth + 'px',
      transition: offset === 0 ? `left ${moveSpeed}ms ease-in-out` : '',
    }"
  >
    <div
      v-for="(planet, index) in viewPlanets"
      :key="pointer + index"
      :style="{
        width: planetWidth + 'px',
        height: planetWidth + 'px',
        opacity: 1 - (Math.abs(index - (N + 1) / 2) * 1) / (N - 2),
        transform: `scale(${
          1 - (Math.abs(index - (N + 1) / 2) * 1) / (N + 2)
        })`,
        transition: `opacity ${moveSpeed}ms ease-in-out, transform ${moveSpeed}ms ease-in-out`,
      }"
      class="planet"
    >
      <img :src="planet.image" />

      <div class="planet-text">
        <p class="p1">
          {{ planet.name }}
        </p>
        <p class="p2">Current missions: {{ planet.missionStatus.current }}</p>
        <p class="p3">Past missions: {{ planet.missionStatus.past }}</p>
        <span class="explore">
          Explore
          <svg
            width="8"
            height="14"
            viewBox="0 0 8 14"
            xmlns="http://www.w3.org/2000/svg"
            aria-hidden="true"
            focusable="false"
            class="IconCaret ml-1 text-base"
          >
            <path
              d="M7.864 7.004L1.5 13.368.086 11.954l4.948-4.95-4.948-4.95L1.5.64l6.364 6.364z"
              fill="currentColor"
            ></path>
          </svg>
        </span>
      </div>
    </div>
  </div>

  <div class="button left-button" @click="moveRight">
    <Button />
  </div>

  <div class="button right-button" @click="moveLeft">
    <Button style="transform: rotate(180deg)" />
  </div>
</template>

<script>
import Button from "./components/Button.vue";

const planetWidth = 400;
const moveSpeed = 500;

export default {
  data() {
    return {
      allPlanets: [],
      planetWidth,
      moveSpeed,
      pointer: 0,
      offset: 0,
      clickable: true,
      winWidth: window.innerWidth,
    };
  },
  mounted() {
    window.addEventListener("resize", () => {
      this.winWidth = window.innerWidth;
    });
    fetch("/data.json")
      .then((response) => response.json())
      .then((json) => {
        this.allPlanets.push(...json);
      });
  },
  computed: {
    planetsWidth() {
      return (this.N + 2) * this.planetWidth;
    },
    N() {
      const n = Math.ceil(this.winWidth / this.planetWidth);
      if (n % 2 == 0) {
        return n + 1; //确保我们得到的n是奇数，便于pointer定位指针的位置
      }
      return n;
    },
    viewPlanets() {
      if (this.allPlanets.length === 0) {
        return [];
      } //当Json还没解析完成时，返回一个空数组 数据加载是async的，而computed会立刻计算属性
      //所以我们需要等星球数据加载完成，再去计算。

      let pointer = this.pointer % this.allPlanets.length;
      if (pointer < 0) {
        pointer += this.allPlanets.length;
      }
      const arr = [this.allPlanets[pointer]]; //加载完成首页最中间的图片
      // right
      const wingCount = (this.N - 1) / 2 + 1; //冗余：当winwidth手动改变时产生的边界问题
      let nextPointer = pointer + 1;
      let previousPointer = pointer - 1;
      for (let i = 0; i < wingCount; i++) {
        if (nextPointer >= this.allPlanets.length) {
          nextPointer = 0;
        }
        arr.push(this.allPlanets[nextPointer]);
        nextPointer++;
      }
      for (let i = 0; i < wingCount; i++) {
        if (previousPointer < 0) {
          previousPointer = this.allPlanets.length - 1;
        }
        arr.unshift(this.allPlanets[previousPointer]);
        previousPointer--;
      }

      return arr;
    },
    left() {
      return this.offset + (this.winWidth - this.planetsWidth) / 2;
    },
  },

  methods: {
    moveLeft() {
      if (!this.clickable) {
        return;
      }

      this.clickable = false;
      this.pointer++;
      this.offset = this.planetWidth;

      setTimeout(() => {
        this.offset = 0;
      }, 0);
      setTimeout(() => {
        this.clickable = true;
      }, this.moveSpeed);
    },
    moveRight() {
      if (!this.clickable) {
        return;
      }

      this.clickable = false;
      this.pointer--;
      this.offset = -this.planetWidth;

      setTimeout(() => {
        this.offset = 0;
      }, 0);
      setTimeout(() => {
        this.clickable = true;
      }, this.moveSpeed);
    },
  },
  components: {
    Button,
  },
};
</script>

<style>
</style>