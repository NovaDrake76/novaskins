<template>
  <div class="marketTop">
    <a class="logoContainer" href="#/"
      ><img class="logo" src="/logo.webp" alt="logo"
    /></a>
    <div class="inputContainer">
      <magnify style="display: flex; align-self: center; padding: 5px" />
      <input class="marketSearch" type="text" placeholder="Search" />
    </div>
    <div class="userContent">
      <div><a href="/#/market">Market</a></div>
      <div><a href="/#/tracker">Tracker</a></div>
      <bell-outline /> <cart-outline />
      <img src="/avatar.webp" alt="user avatar" class="userAvatar" />
    </div>
  </div>
</template>

<script>
import BellOutline from "vue-material-design-icons/BellOutline.vue";
import CartOutline from "vue-material-design-icons/CartOutline.vue";
import Magnify from "vue-material-design-icons/Magnify.vue";

export default {
  components: {
    BellOutline,
    CartOutline,
    Magnify,
  },
};
</script>

<style>
.marketTop {
  display: flex;
  height: 15vh;
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
  width: 15%;
  height: 100%;
}

.userAvatar {
  width: 40px;
  height: 40px;
  border-radius: 100%;
  object-fit: cover;
}
</style>