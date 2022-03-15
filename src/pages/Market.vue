<template>
  <section>
    <div v-if="modalOpen == true" class="modalContainer">
      <div class="modal">
        <div class="modalTop" @click="modalOpen = false">X</div>
        <div class="modalBottom">
          <div class="modalImageContainer">
            <img
              class="modalImage"
              v-bind:src="`https://community.akamai.steamstatic.com/economy/image/${skinDataAUX.image}/fxf`"
              alt="skin"
            />
          </div>
          <div class="modalInfoContainer">
            <h1 class="skinTitles">{{ skinDataAUX.name }}</h1>
            <h2>Volume: {{ skinData.volume }}</h2>
            <h2>Median price: {{ skinData.median_price }}</h2>
            <h1 class="skinTitles">
              Lowest price: {{ skinData.lowest_price }}
            </h1>
          </div>
        </div>
      </div>
    </div>
    <div class="market">
      <div class="marketTop">
        <a href="#/"><img src="/logo.webp" alt="logo" width="150" /></a>
        <div class="inputContainer">
          <magnify style="display: flex; align-self: center; padding: 5px" />
          <input
            v-model="search"
            @change="fetchData()"
            class="marketSearch"
            type="text"
            placeholder="Search"
          />
        </div>
        <div class="userContent">
          <bell-outline /> <cart-outline />
          <img src="/avatar.webp" alt="user avatar" class="userAvatar" />
        </div>
      </div>
      <div class="marketMid">
        <div class="sidebarContainer">
          <div class="sidebar">
            <div class="priceFilter">
              <div class="priceFilterContent">
                <span>$</span>
                <input
                  type="number"
                  class="priceFilterInput"
                  min="1"
                  step="any"
                  placeholder="min"
                />
              </div>
              <div style="padding: 0 10px 0 10px">-</div>
              <div class="priceFilterContent">
                <span>$</span>

                <input
                  type="number"
                  class="priceFilterInput"
                  min="1"
                  step="any"
                  placeholder="max"
                />
              </div>
            </div>
            <button
              class="sidebarButton"
              @click="(url = knifeURL), fetchDataFiltered()"
            >
              Knifes
            </button>
            <button
              class="sidebarButton"
              @click="(url = glovesURL), fetchDataFiltered()"
            >
              Gloves
            </button>
            <button
              class="sidebarButton"
              @click="(url = riflesURL), fetchDataFiltered()"
            >
              Rifles
            </button>
            <button
              class="sidebarButton"
              @click="(url = pistolsURL), fetchDataFiltered()"
            >
              Pistols
            </button>
            <button
              class="sidebarButton"
              @click="(url = smgURL), fetchDataFiltered()"
            >
              SMG
            </button>
            <button
              class="sidebarButton"
              @click="(url = shotgunsURL), fetchDataFiltered()"
            >
              Shotguns
            </button>
            <button
              class="sidebarButton"
              @click="(url = stickersURL), fetchDataFiltered()"
            >
              Stickers
            </button>
            <button
              class="sidebarButton"
              @click="(url = agentsURL), fetchDataFiltered()"
            >
              Agents
            </button>
          </div>
        </div>
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
        <div v-else class="skinsContainer">
          <div
            class="skins"
            v-for="skin in skins.results"
            :key="skin"
            @click="
              (skinDataAUX.name = skin.name),
                (skinDataAUX.image = skin.asset_description.icon_url),
                fetchSkin(),
                (modalOpen = true)
            "
          >
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
                <div class="skinsInformations">
                  <h3 class="skinName">{{ skin.name }}</h3>
                  <p class="skinPrice">
                    Listings: <b>{{ skin.sell_listings }}</b>
                  </p>
                </div>
              </div>
              <div class="priceSkin">
                <b>{{ skin.sell_price_text }}</b>
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
import BellOutline from "vue-material-design-icons/BellOutline.vue";
import CartOutline from "vue-material-design-icons/CartOutline.vue";
import Magnify from "vue-material-design-icons/Magnify.vue";

export default {
  components: {
    HalfCircleSpinner,
    BellOutline,
    CartOutline,
    Magnify,
  },
  data() {
    return {
      skins: [],
      loading: false,
      knifeURL: `CSGO_Type_Knife`,
      glovesURL: `Type_Hands`,
      riflesURL: "CSGO_Type_Rifle",
      pistolsURL: "CSGO_Type_Pistol",
      smgURL: "CSGO_Type_SMG",
      shotgunsURL: "CSGO_Type_Shotgun",
      stickersURL: "CSGO_Tool_Sticker",
      agentsURL: "Type_CustomPlayer",
      search: "",
      modalOpen: false,
      skinData: [],
      skinDataAUX: [{ name: "", image: "" }],
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
          `https://skinky.herokuapp.com/https://steamcommunity.com/market/search/render/?search_descriptions=0&appid=730&norender=1&count=500`
        );

        this.skins = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    async fetchDataFiltered() {
      this.loading = true;
      try {
        const responseFilteder = await fetch(
          `https://skinky.herokuapp.com/https://steamcommunity.com/market/search/render/search?&category_730_ItemSet%5B%5D=any&category_730_ProPlayer%5B%5D=any&category_730_StickerCapsule%5B%5D=any&category_730_TournamentTeam%5B%5D=any&category_730_Weapon%5B%5D=any&category_730_Type%5B%5D=tag_${this.url}&norender=1&count=500`
        );

        this.skins = await responseFilteder.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
    async fetchSkin() {
      try {
        const responseSkin = await fetch(
          `https://skinky.herokuapp.com/https://steamcommunity.com/market/priceoverview/?appid=730&currency=1&market_hash_name=${this.skinDataAUX.name}`
        );
        this.skinData = await responseSkin.json();
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
@import url("../assets/home.css");
@import url("../components/modal.css");

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

.inputContainer {
  width: 40%;
  height: 30%;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #44444b;
  border-radius: 4px;
}

.marketSearch {
  width: 100%;
  color: white;
  border: none;
  outline: none;
  background-color: transparent;
}

.userContent {
  display: flex;
  align-items: center;
  justify-content: space-around;
  width: 10%;
  height: 100%;
}

.userAvatar {
  width: 40px;
  height: 40px;
  border-radius: 100%;
  object-fit: cover;
}

.marketMid {
  display: flex;
  justify-content: center;
  align-self: center;
  width: 95%;
  height: 100%;
  background-color: #141414;
  border-radius: 8px;
  overflow: hidden;
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

.priceFilterContent {
  background-color: #252525;
  border-radius: 4px;
  padding-left: 4px;
  width: 35%;
}

.priceFilterInput {
  width: 75%;
  background-color: transparent;
  border: none;
  color: white;
  outline: none;
}

.sidebarButton {
  width: 80%;
  height: 40px;
  background-color: transparent;
  border: none;
  color: #bcbcc2;
}

.sidebarButton:hover {
  color: white;
  cursor: pointer;
}
</style>
