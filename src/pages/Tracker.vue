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
      <div>Total Kills: {{ playerInfo.playerstats.stats[0].value }}</div>
      <div>Total Deaths: {{ playerInfo.playerstats.stats[1].value }}</div>
      <div>
        Total Planted Bombs: {{ playerInfo.playerstats.stats[3].value }}
      </div>
      <div>
        Total Defused Bombs: {{ playerInfo.playerstats.stats[4].value }}
      </div>
      <div>Total Round Wins: {{ playerInfo.playerstats.stats[5].value }}</div>
      <div>Total Damage Done: {{ playerInfo.playerstats.stats[6].value }}</div>
      <div>
        Total Money Earned: ${{ playerInfo.playerstats.stats[7].value }}
      </div>
      <div>
        Total Rescued Hostages: {{ playerInfo.playerstats.stats[8].value }}
      </div>
      <div>Total Knife Kills: {{ playerInfo.playerstats.stats[9].value }}</div>
      <div>Total HE Kills: {{ playerInfo.playerstats.stats[10].value }}</div>
      <div>Total Glock Kills: {{ playerInfo.playerstats.stats[11].value }}</div>
      <div>Total Ak Kills: {{ playerInfo.playerstats.stats[20].value }}</div>
      <div>Total AWP Kills: {{ playerInfo.playerstats.stats[19].value }}</div>
      <div>Total AUG Kills: {{ playerInfo.playerstats.stats[21].value }}</div>
      <div>
        Total Headshot Kills: {{ playerInfo.playerstats.stats[25].value }}
      </div>
      <div>
        Total Kills with an Enemy Weapon:
        {{ playerInfo.playerstats.stats[26].value }}
      </div>
      <div>
        Total Pistol Round Wins: {{ playerInfo.playerstats.stats[27].value }}
      </div>
      <div>
        Total Dust 2 Round Wins:
        {{ playerInfo.playerstats.stats[31].value }}
        Win Rate:
        {{
          Math.round(
            (playerInfo.playerstats.stats[31].value /
              playerInfo.playerstats.stats[76].value) *
              10000
          ) / 100
        }}%
      </div>
      <div>
        Total Inferno Round Wins: {{ playerInfo.playerstats.stats[32].value }}
        Win Rate:
        {{
          Math.round(
            (playerInfo.playerstats.stats[32].value /
              playerInfo.playerstats.stats[77].value) *
              10000
          ) / 100
        }}%
      </div>
      <div>
        Total Nuke Round Wins: {{ playerInfo.playerstats.stats[33].value }}
        Win Rate:
        {{
          Math.round(
            (playerInfo.playerstats.stats[33].value /
              playerInfo.playerstats.stats[78].value) *
              10000
          ) / 100
        }}%
      </div>
      <div>
        Total Weapons Donated: {{ playerInfo.playerstats.stats[0].value }}
      </div>
      <div>
        Total Kills against a Blinded Enemy:
        {{ playerInfo.playerstats.stats[36].value }}
      </div>
      <div>
        Total Knife Fight Wins: {{ playerInfo.playerstats.stats[37].value }}
      </div>
      <div>
        Total Knife against a Zoomed Sniper:
        {{ playerInfo.playerstats.stats[38].value }}
      </div>
      <div>Total : {{ playerInfo.playerstats.stats[9].value }}</div>
      <div>
        Last Match Info:
        <ul>
          <li>Last Match Wins {{ playerInfo.playerstats.stats[82].value }}</li>
          <li>Last Match Kills {{ playerInfo.playerstats.stats[84].value }}</li>
          <li>
            Last Match Deaths {{ playerInfo.playerstats.stats[82].value }}
          </li>
          <li>
            Last Match KD
            {{
              Math.round(
                (playerInfo.playerstats.stats[84].value /
                  playerInfo.playerstats.stats[85].value) *
                  100
              ) / 100
            }}
          </li>
          <li>Last Match MVP's {{ playerInfo.playerstats.stats[86].value }}</li>
          <li>
            Last Match Damage {{ playerInfo.playerstats.stats[91].value }}
          </li>
        </ul>
      </div>
      <div>Total MVP's: {{ playerInfo.playerstats.stats[95].value }}</div>
      <div>
        Total Matches Won: {{ playerInfo.playerstats.stats[118].value }}
        Win Rate:
        {{
          Math.round(
            (playerInfo.playerstats.stats[118].value /
              playerInfo.playerstats.stats[119].value) *
              10000
          ) / 100
        }}%
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
      loading: false,
      playerInfo: [],
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
          "https://skinky.herokuapp.com/http://api.steampowered.com/ISteamUserStats/GetUserStatsForGame/v2/?appid=730&key=708CFC659DA8AF1B52841691D92B69AF&steamid=76561198360105612"
        );

        this.playerInfo = await response.json();
        this.loading = false;
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
.container {
  height: 1000px;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
}

.statsInfo {
  width: 100%;
  height: 100%;
}
</style>
