<template>
  <link rel="preload" as="image" href="/ak1.webp" />
  <link rel="preload" as="image" href="/ak2.webp" />
  <link rel="preload" as="image" href="/ak3.webp" />

  <section>
    <div class="containerTop">
      <div class="topText">
        <img src="/logo.webp" alt="logo" class="sliderImages" />
      </div>
    </div>
    <div v-if="loading == true" class="loading" style="margin: 1% 0px 1% 46%">
      <atom-spinner :animation-duration="1000" :size="120" color="#009f16" />
    </div>
    <div v-else>
      <div class="containerMid">
        <div class="presentation">
          <div class="midText">
            <div class="title">Find the best price</div>
            <div class="subTitle" id="flip">
              <div><div>Market Overview</div></div>
              <div><div>Easy Search</div></div>
              <div><div>No Costs</div></div>
            </div>
            <a href="#/market">
              <button class="btnMarket">Get Started</button></a
            >
          </div>

          <div
            class="imageEffect"
            v-motion
            :initial="{ opacity: 0 }"
            :enter="{ opacity: 1 }"
            :delay="200"
          >
            <div class="container__image">
              <div class="container__info">
                Lowest Price:
                <span :style="{ color: price1 }">{{ neo.lowest_price }}</span>
              </div>
              <div class="container__info container__moreInfo">
                Median Price:
                <span :style="{ color: price2 }">{{ neo.median_price }}</span>
              </div>
            </div>
          </div>
        </div>
        <div class="weapons" ref="demo" v-drag="dragHandler">
          <div
            class="containerCards"
            v-for="skin in skins.results"
            :key="skin.name"
          >
            <!-- <p>{{ skin }}</p> -->

            <div class="card">
              <div class="cardImage">
                <img
                  class="cardImageCover"
                  v-bind:src="
                    'https://community.akamai.steamstatic.com/economy/image/' +
                    skin.asset_description.icon_url +
                    '/fxf'
                  "
                  alt="skin"
                  width="300"
                />
              </div>
              <div class="cardContent">
                <h3>{{ skin.name }}</h3>
                <p>
                  Listings: <b>{{ skin.sell_listings }}</b>
                </p>
                <p>
                  Starting at: <b>{{ skin.sell_price_text }}</b>
                </p>
              </div>
            </div>
          </div>
        </div>
        <div class="degrade" />
      </div>
      <div class="containerParallax">
        <div class="parallaxTextBackground">
          <h1 class="titleParallax" style="line-height: 100%">
            Track the bests discounts
          </h1>
        </div>
      </div>
      <div class="containerBottom">
        <div class="bottomText">
          <h1 class="title">Make Sure it worths</h1>
          <div class="subTitle">To low and High tiers</div>
          <a href="#/market">
            <button class="btnMarket">Go to Market</button></a
          >
        </div>
        <div class="containerBottomRight">
          <img
            class="sliderImages"
            v-if="slider >= 70"
            src="/ak3.webp"
            alt="ak3"
          />
          <img
            v-else-if="slider > 30 && slider < 70"
            class="sliderImages"
            src="/ak2.webp"
            alt="ak2"
          />
          <img
            v-else-if="slider < 70"
            class="sliderImages"
            src="/ak1.webp"
            alt="ak"
          />
          <div class="sliderContainer">
            <input
              type="range"
              min="1"
              max="100"
              v-model="slider"
              class="slider"
              id="myRange"
            />
          </div>
        </div>
      </div>
    </div>
  </section>
</template>

<script>
import { HalfCircleSpinner, AtomSpinner } from "epic-spinners";
import { dragDirective, useDrag } from "@vueuse/gesture";
import { ref } from "vue";

const demo = ref();

const dragHandler = ({ movement: [x, y], dragging }) => {
  if (!dragging) {
    set({ x: 0, y: 0, cursor: "grab" });
    return;
  }

  set({
    cursor: "grabbing",
    x,
    y,
  });
};

// Composable usage
useDrag(dragHandler, {
  domTarget: demo,
});
export default {
  directives: {
    drag: dragDirective,
  },
  components: {
    HalfCircleSpinner,
    AtomSpinner,
  },
  data() {
    return {
      skins: [],
      neo: [],
      price1: "#d63031",
      price2: "#009f16",
      loading: false,
      slider: 80,
    };
  },
  created() {
    this.fetchData();
  },

  methods: {
    async fetchData() {
      this.loading = true;
      try {
        const response = await fetch(
          "https://skinky.herokuapp.com/https://steamcommunity.com/market/search/render/?search_descriptions=0&sort_column=default&sort_dir=desc&appid=730&norender=1&count=50"
        );

        const responseNeo = await fetch(
          "https://skinky.herokuapp.com/https://steamcommunity.com/market/priceoverview/?appid=730&currency=1&market_hash_name=M4A4%20|%20Neo-Noir%20%28Field-Tested%29"
        );

        this.skins = await response.json();
        this.neo = await responseNeo.json();
        if (this.neo.lowest_price < this.neo.median_price) {
          this.price1 = "#009f16";
          this.price2 = "#d63031";
        }
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    moveDiv() {
      const weapons = document.getElementById("weapons");
      weapons.style.transform = "translateX(-200px)";
    },
  },
};
</script>

<style>
@import url("../assets/home.css");
</style>
