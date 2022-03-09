<template>
  <section>
    <div class="market">
      <div class="marketTop">
        <a href="#/"><img src="logo.webp" alt="logo" width="150" /></a>
        <input class="marketSearch" type="text" placeholder="Search" />
        <div class="userContent">Notificações, carrinho, saldo, user</div>
      </div>
      <div class="marketMid">
        <div class="sidebarContainer">
          <div class="sidebar">
            <div class="priceFilter">
              <input
                type="number"
                class="priceFilterInput"
                min="1"
                step="any"
                placeholder="min"
              />
              <div>-</div>
              <input
                type="number"
                class="priceFilterInput"
                min="1"
                step="any"
                placeholder="max"
              />
            </div>
            <button class="sidebarButton">Knifes</button>
            <button class="sidebarButton">Gloves</button>
            <button class="sidebarButton">Rifles</button>
            <button class="sidebarButton">Pistols</button>
            <button class="sidebarButton">SMG</button>
            <button class="sidebarButton">Shotguns</button>
            <button class="sidebarButton">Stickers</button>
            <button class="sidebarButton">Agents</button>
          </div>
        </div>
        <div
          v-if="loading == true"
          class="loading"
          style="
            display: flex;
            align-items: center;
            justify-content: center;
            width: 100%;
          "
        >
          <half-circle-spinner
            :animation-duration="1000"
            :size="120"
            color="#5e5b5b"
          />
        </div>
        <div v-else class="skinsContainer">
          <div class="skins" v-for="skin in skins.results" :key="skin">
            <div class="cardSkins">
              <div class="cardImage">
                <img
                  class="cardImageCover"
                  v-bind:src="
                    'https://community.akamai.steamstatic.com/economy/image/' +
                    skin.asset_description.icon_url +
                    '/fxf'
                  "
                  alt="skin"
                  style="object-fit: scale-down"
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
      skins: [],
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
          "https://skinky.herokuapp.com/https://steamcommunity.com/market/search/render/?search_descriptions=0&appid=730&norender=1&count=500"
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

input::-webkit-outer-spin-button,
input::-webkit-inner-spin-button {
  -webkit-appearance: none;
}

/* Firefox */
input[type="number"] {
  -moz-appearance: textfield;
}

.market {
  display: flex;
  flex-direction: column;
  width: 100vw;
  height: 100vh;
}

.marketTop {
  display: flex;
  height: 20vh;
  align-items: center;
  justify-content: space-around;
}

.marketSearch {
  width: 40%;
  height: 30%;
  background-color: #44444b;
  color: white;
  border: none;
  border-radius: 4px;
  outline: none;
}

.marketMid {
  display: flex;
  justify-content: center;
  align-self: center;
  width: 95%;
  height: 100%;
  background-color: #141414;
  border-radius: 8px;
}

.skinsContainer {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  width: 80vw;
  height: 100%;
  overflow-y: auto;
  scrollbar-color: gray;
  margin-top: 10px;
}

.skinsContainer::-webkit-scrollbar {
  width: 12px;
  scrollbar-color: gray;
  color: gray;
}

.skinsContainer::-webkit-scrollbar-track {
  -webkit-box-shadow: inset 0 0 6px rgba(0, 0, 0, 0.3);
  border-radius: 10px;
}

.skinsContainer::-webkit-scrollbar-thumb {
  border-radius: 10px;
  background: #434348;
}

.skins {
  display: flex;
  padding-bottom: 10px;
}

.cardSkins {
  width: 260px;
  height: 300px;
  border-radius: 12px;
  box-shadow: rgba(0, 0, 0, 0.1) 0px 4px 12px;
  background-color: #252525;
  color: white;
  object-fit: scale-down;
}

.sidebarContainer {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 15vw;
}

.sidebar {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
  margin-top: 10px;
}

.priceFilter {
  display: flex;
  justify-content: center;
  width: 100%;
}

.priceFilterInput {
  width: 35%;
  background-color: #252525;
  border-radius: 4px;
  border: none;
  color: white;
  outline: none;
}

.sidebarButton {
  width: 80%;
  height: 40px;
  background-color: transparent;
  border: none;
  color: white;
}
</style>
