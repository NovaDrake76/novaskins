<template>
  <section>
    <div class="market">
      <div class="marketTop">
        <img src="logo.webp" alt="logo" width="150" />
        <input class="marketSearch" type="text" placeholder="Search" />
        <div class="userContent">Notificações, carrinho, saldo, user</div>
      </div>
      <div class="marketMid">
        <div class="sidebar">
          <button>a</button>
          <button>b</button>
          <button>c</button>
        </div>
        <div class="skinsContainer">
          <div class="skins" v-for="skin in skins.results" :key="skin">
            {{ skin.sell_listings }}
            {{ skin.name }}
            {{ skin.sell_price_text }}
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
}

.marketMid {
  display: flex;
  height: 100%;
}

.skinsContainer {
  display: flex;
  justify-content: space-around;
  flex-wrap: wrap;
  width: 80vw;
  height: 100%;
}

.skins {
  display: flex;
  border: 1px solid red;
}

.sidebar {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 15vw;
  background-color: #141414;
}
</style>
