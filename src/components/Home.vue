<template>
  <section>
    <div class="containerTop">
      <div class="topText">
        <img src="logo.webp" alt="" height="240" />
      </div>
    </div>
    <div v-if="loading == true" class="loading" style="margin: 1% 0px 0px 46%">
      <half-circle-spinner
        :animation-duration="1000"
        :size="120"
        color="#5e5b5b"
      />
    </div>
    <div v-else class="containerMid">
      <div class="presentation">
        <div class="midText">
          <div class="title">Find the best price</div>
          <div class="subTitle" id="flip">
            <div><div>Market Overview</div></div>
            <div><div>Easy Search</div></div>
            <div><div>No Costs</div></div>
          </div>
        </div>

        <div class="imageEffect">
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
        <div class="cover atvImg">
          <div
            class="atvImg-layer"
            data-img="http://robindelaporte.fr/codepen/visa-bg-2.jpg"
          ></div>
          <div
            class="atvImg-layer"
            data-img="http://robindelaporte.fr/codepen/visa.png"
          ></div>
        </div>
      </div>
      <div class="weapons">
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
    </div>
    <!-- <div class="container3">
      <h1 class="title" style="margin-right: 10%">The Best Skins</h1>
    </div>
    <div class="containerBottom"></div> -->
  </section>
</template>

<script>
import { HalfCircleSpinner } from "epic-spinners";

export default {
  components: {
    HalfCircleSpinner,
  },
  data() {
    return {
      skins: [],
      neo: [],
      price1: "#d63031",
      price2: "#009f16",
      loading: false,
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
  },
};
</script>

<style>
@import url("../assets/home.css");
</style>
