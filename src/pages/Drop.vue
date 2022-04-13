<template>
  <section>
    <div
      v-if="loading == true"
      class="loading"
      style="
        display: flex;
        align-items: center;
        justify-content: center;
        width: 80vw;
        height: 100%;
      "
    >
      <half-circle-spinner
        :animation-duration="1000"
        :size="120"
        color="#5e5b5b"
      />
    </div>
    <div v-else class="container">
      <div class="skinCarousel" id="skinCarousel">
        <div class="skin" v-for="x in skins.results" v-bind:key="x">
          <img
            class="cardImage"
            v-bind:src="
              'https://community.akamai.steamstatic.com/economy/image/' +
              x.asset_description.icon_url +
              '/fxf'
            "
            alt="skin"
          />
          <p>{{ skinRarity }}</p>
          {{ x.name }}
        </div>
      </div>
      <button @click="(started = true), random()">spin</button>
    </div>
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
      loading: false,
      started: false,
      skinRarity: null,
      skins: [],
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

        this.skins = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    random() {
      console.log(this.started);
      try {
        let random = Math.random() * 100;
        if (random <= 0.3) {
          this.skinRarity = "Exceedingly Rare";
        } else if (random >= 0.31 && random <= 2.83) {
          this.skinRarity = "Classified";
        } else if (random >= 2.84 && random <= 15.6) {
          this.skinRarity = "Restricted";
        } else if (random >= 15.7 && random <= 83) {
          this.skinRarity = "Covert";
        }
      } catch (error) {
        console.log(error);
      }
      document.getElementById("skinCarousel").scrollLeft += 2000;
    },
  },
};
</script>

<style>
.container {
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}

.skinCarousel {
  display: flex;
  width: 70%;
  overflow-x: hidden;
  scroll-behavior: smooth;
}

.skin {
  min-width: 200px;
  height: 230px;
  border-radius: 14px;
  margin-left: 5px;
  background-color: white;
  color: black;
}

.cardImage {
  width: 200px;
  background-image: url("/imageCoverBackground.webp");
  background-size: cover;
  background-repeat: no-repeat;
  border-radius: 12px 12px 0 0;
  object-fit: scale-down;
  text-align: center;
  transition: background-color 0.3s;
}
</style>
