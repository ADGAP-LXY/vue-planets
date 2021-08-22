<template>
  <div class="missions">
    <p class="label">missions</p>
    <h2>Exploring the Universe</h2>
    <p class="text-body-md">
      Spacecraft developed at JPL have flown to <br />
      every planet in the solar system and the Sun, <br />
      and beyond.
    </p>
  </div>

  <div class="planets" :style="{ width: planetsWidth + 'px'}">
    <div v-for="item in planets" :key="item.status" :style="{ width: planetWidth + 'px' ,height: planetWidth + 'px'}" class="planet" >
      <img :src="item.image" />
    </div>
  </div>

  <div
    class="
      swiper-container
      lg:mt-0
      lg:mb-0
      mb-14
      mt-3
      swiper-container-initialized
      swiper-container-horizontal
      swiper-container-pointer-events
      opacity-100
    "
  >
    <div>
      <a href=""></a>
    </div>
  </div>
</template>

<script>
import HelloWorld from "./components/HelloWorld.vue";

export default {
  data() {
    return {
      name: "App",
      planets: [],
      planetWidth: 500,
    };
  },
  mounted() {
    fetch("/data.json")
      .then((response) => response.json())
      .then((json) => {
        this.planets.push(...json);
      });
  },
  computed: {
    planetsWidth() {
      return this.planets.length * this.planetWidth
    }
  },
  
};
</script>

<style>
body {
  background-image: url("/images/bg.jpg");
}
p {
  display: block;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
}
.text-white {
  --text-opacity: 1;
  color: #fff;
  color: rgba(255, 255, 255, var(--text-opacity));
}
.col-start-2 {
  grid-column-start: 2;
}
.px-4 {
  padding-left: 1rem;
  padding-right: 1rem;
}
.order-1 {
  order: 1;
}

div {
  display: block;
}
.text-white {
  --text-opacity: 1;
  color: #fff;
  color: rgba(255, 255, 255, var(--text-opacity));
}
.bg-black {
  --bg-opacity: 1;
  background-color: #000;
  background-color: rgba(0, 0, 0, var(--bg-opacity));
}
</style>