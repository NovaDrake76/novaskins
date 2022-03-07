<template>
  <section>
    <div class="containerTop">
      <div class="topText">
        <img src="logo.webp" alt="" height="240" />
      </div>
    </div>
    <div v-if="loading == true" class="loading">
      <half-circle-spinner
        :animation-duration="1000"
        :size="60"
        color="#5e5b5b"
      />
    </div>
    <div class="containerMid">
      <div class="presentation">
        <div class="midText">
          <div class="title">Find the best price</div>
          <div class="subTitle" id="flip">
            <div><div>Market Overview</div></div>
            <div><div>Easy Search</div></div>
            <div><div>No Costs</div></div>
          </div>
        </div>
        <img src="m4a4.webp" alt="m4a4" width="60%" />
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
                height=""
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
      loading: false,
      phrase: ["Market Overview", "salve", "breno", "careca"],
    };
  },
  created() {
    this.fetchData();
  },
  mounted() {
    window.setInterval(() => {
      this.changePhrase();
      // if (this.phraseAux === true) {
      //   this.phraseAux = false;
      // } else {
      //   this.phraseAux = true;
      // }
    }, 3000);
  },
  methods: {
    async fetchData() {
      this.loading = true;
      try {
        const response = await fetch(
          "https://skinky.herokuapp.com/https://steamcommunity.com/market/search/render/?search_descriptions=0&sort_column=default&sort_dir=desc&appid=730&norender=1&count=50"
        );

        this.skins = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    changePhrase() {
      const textDiv = document.getElementById("subTitle");
      const current = this.phrase.shift();
      this.phrase = this.phrase.concat(current);
      // textDiv.style.transform = "rotate(30deg)";
    },
  },
};
</script>

<style>
@import url("../assets/home.css");
</style>
