<template>
  <section>
    <Navbar />

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
      <div class="playerGeralInfo">
        <div class="infoLeft">
          <div class="playerInfo">
            <img
              :src="playerSteamInfo.response.players[0].avatarfull"
              alt="Player Avatar"
              class="playerAvatar"
            />
            <div class="infoHead">
              <div class="infoTop">
                <div class="infoTopContainer">
                  <h1 class="PlayerName">
                    {{ playerSteamInfo.response.players[0].personaname }}
                  </h1>
                  <img
                    :src="
                      `ranks/` +
                      playerInfo.playerstats.stats[205].value +
                      `.webp`
                    "
                    class="PlayerRank"
                    alt="Player Rank"
                  />
                </div>
              </div>
              <div class="subInfoP">
                <p>
                  {{ playerInfo.playerstats.stats[0].value.toLocaleString() }}
                  Kills
                </p>
                <p>
                  K/D:
                  {{
                    (
                      playerInfo.playerstats.stats[0].value /
                      playerInfo.playerstats.stats[1].value
                    ).toFixed(2)
                  }}
                </p>
                <p>
                  Precision:
                  {{
                    (
                      (playerInfo.playerstats.stats[42].value * 100) /
                      playerInfo.playerstats.stats[43].value
                    ).toFixed(2)
                  }}
                  %
                </p>
              </div>
            </div>
          </div>
          <div class="lastMatch">
            <h2>Last Match Info:</h2>
            <div class="lastMatchInfo">
              <div class="lastMatchText">
                <h2>
                  {{ playerInfo.playerstats.stats[84].value.toLocaleString() }}
                </h2>
                <span> Kills</span>
              </div>
              <div class="lastMatchText">
                <h2>
                  {{ playerInfo.playerstats.stats[85].value.toLocaleString() }}
                </h2>
                <span>Deaths</span>
              </div>
            </div>

            <div class="lastMatchInfo">
              <div class="lastMatchText">
                <h2>
                  {{
                    Math.round(
                      (playerInfo.playerstats.stats[84].value /
                        playerInfo.playerstats.stats[85].value) *
                        100
                    ) / 100
                  }}
                </h2>
                <span>K/D</span>
              </div>
              <div class="lastMatchText">
                <h2>
                  {{ playerInfo.playerstats.stats[86].value.toLocaleString() }}
                </h2>
                <span>MVP's</span>
              </div>
            </div>
          </div>
        </div>
        <div class="infoRight">
          <div class="barChartContainer">
            <apexchart
              width="80%"
              height="100%"
              type="bar"
              :options="chartOptions"
              :series="series"
            ></apexchart>
          </div>
          <div class="radarChartContainer">
            <apexchart
              width="80%"
              height="100%"
              type="radar"
              :options="chartOptions"
              :series="series"
            ></apexchart>
          </div>
        </div>
      </div>

      <div>
        Total Deaths:
        {{ playerInfo.playerstats.stats[1].value.toLocaleString() }}
      </div>

      <div>
        Total Planted Bombs:
        {{ playerInfo.playerstats.stats[3].value.toLocaleString() }}
      </div>
      <div>
        Total Defused Bombs:
        {{ playerInfo.playerstats.stats[4].value.toLocaleString() }}
      </div>
      <div>
        Total Round Wins:
        {{ playerInfo.playerstats.stats[5].value.toLocaleString() }}
      </div>
      <div>
        Total Damage Done:
        {{ playerInfo.playerstats.stats[6].value.toLocaleString() }}
      </div>
      <div>
        Total Money Earned: ${{
          playerInfo.playerstats.stats[7].value.toLocaleString()
        }}
      </div>
      <div>
        Total Rescued Hostages:
        {{ playerInfo.playerstats.stats[8].value.toLocaleString() }}
      </div>
      <div>
        Total Knife Kills:
        {{ playerInfo.playerstats.stats[9].value.toLocaleString() }}
      </div>
      <div>
        Total HE Kills:
        {{ playerInfo.playerstats.stats[10].value.toLocaleString() }}
      </div>
      <div>
        Total Glock Kills:
        {{ playerInfo.playerstats.stats[11].value.toLocaleString() }}
      </div>
      <div>
        Total Ak Kills:
        {{ playerInfo.playerstats.stats[20].value.toLocaleString() }} -
        Precision:
        {{
          (
            (playerInfo.playerstats.stats[64].value * 100) /
            playerInfo.playerstats.stats[50].value
          ).toFixed(2)
        }}%
      </div>
      <div>
        Total AWP Kills:
        {{ playerInfo.playerstats.stats[19].value.toLocaleString() }}-
        Precision:
        {{
          (
            (playerInfo.playerstats.stats[63].value * 100) /
            playerInfo.playerstats.stats[49].value
          ).toFixed(2)
        }}%
      </div>
      <div>
        Total AUG Kills:
        {{ playerInfo.playerstats.stats[21].value.toLocaleString() }}-
        Precision:
        {{
          (
            (playerInfo.playerstats.stats[65].value * 100) /
            playerInfo.playerstats.stats[51].value
          ).toFixed(2)
        }}%
      </div>
      <div>
        Total Headshot Kills:
        {{ playerInfo.playerstats.stats[25].value.toLocaleString() }} -
        Precision:
        {{
          (
            (playerInfo.playerstats.stats[25].value * 100) /
            playerInfo.playerstats.stats[0].value
          ).toFixed(2)
        }}%
      </div>
      <div>
        Total Kills with an Enemy Weapon:
        {{ playerInfo.playerstats.stats[26].value.toLocaleString() }}
      </div>
      <div>
        Total Pistol Round Wins:
        {{ playerInfo.playerstats.stats[27].value.toLocaleString() }}
      </div>
      <div>
        Total Dust 2 Round Wins:
        {{ playerInfo.playerstats.stats[31].value.toLocaleString() }}
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
        Total Inferno Round Wins:
        {{ playerInfo.playerstats.stats[32].value.toLocaleString() }}
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
        Total Nuke Round Wins:
        {{ playerInfo.playerstats.stats[33].value.toLocaleString() }}
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
        Total Weapons Donated:
        {{ playerInfo.playerstats.stats[0].value.toLocaleString() }}
      </div>
      <div>
        Total Kills against a Blinded Enemy:
        {{ playerInfo.playerstats.stats[36].value }}
      </div>
      <div>
        Total Knife Fight Wins:
        {{ playerInfo.playerstats.stats[37].value.toLocaleString() }}
      </div>
      <div>
        Total Knife Kills against a Zoomed Sniper:
        {{ playerInfo.playerstats.stats[38].value.toLocaleString() }}
      </div>
      <div>
        Last Match Info:
        <ul>
          <li>
            Last Match Wins
            {{ playerInfo.playerstats.stats[82].value.toLocaleString() }}
          </li>
          <li>
            Last Match Kills
            {{ playerInfo.playerstats.stats[84].value.toLocaleString() }}
          </li>
          <li>
            Last Match Deaths
            {{ playerInfo.playerstats.stats[82].value.toLocaleString() }}
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
          <li>
            Last Match MVP's
            {{ playerInfo.playerstats.stats[86].value.toLocaleString() }}
          </li>
          <li>
            Last Match Damage
            {{ playerInfo.playerstats.stats[91].value.toLocaleString() }}
          </li>
        </ul>
      </div>
      <div>
        Total MVP's:
        {{ playerInfo.playerstats.stats[95].value.toLocaleString() }}
      </div>
      <div>
        Total Matches Won:
        {{ playerInfo.playerstats.stats[118].value.toLocaleString() }}
        Win Rate:
        {{
          Math.round(
            (playerInfo.playerstats.stats[118].value /
              playerInfo.playerstats.stats[119].value) *
              10000
          ) / 100
        }}%
      </div>
      <div>Last Match Score: {{ playerInfo.playerstats.stats[125].value }}</div>
      <div>
        SSG Kills:
        {{ playerInfo.playerstats.stats[125].value.toLocaleString() }}
        Precision:
        {{
          Math.round(
            (playerInfo.playerstats.stats[139].value /
              playerInfo.playerstats.stats[140].value) *
              1000
          ) / 100
        }}%
      </div>
      <div>
        MP7 Kills:
        {{ playerInfo.playerstats.stats[144].value.toLocaleString() }}
        Precision:
        {{
          Math.round(
            (playerInfo.playerstats.stats[142].value /
              playerInfo.playerstats.stats[143].value) *
              1000
          ) / 100
        }}%
      </div>
      <div>
        M4A1 Kills:
        {{ playerInfo.playerstats.stats[168].value.toLocaleString() }}
        Precision:
        {{
          Math.round(
            (playerInfo.playerstats.stats[172].value /
              playerInfo.playerstats.stats[175].value) *
              1000
          ) / 100
        }}
      </div>
      <div>
        Molotov Kills:
        {{ playerInfo.playerstats.stats[170].value.toLocaleString() }}
      </div>
    </div>
  </section>
</template>

<script>
import { HalfCircleSpinner } from "epic-spinners";
import Navbar from "../components/Navbar.vue";
import VueApexCharts from "vue3-apexcharts";

export default {
  components: {
    HalfCircleSpinner,
    Navbar,
    apexchart: VueApexCharts,
  },
  data() {
    return {
      loading: false,
      playerSteamInfo: [],
      playerInfo: [],
      chartOptions: {
        chart: {
          id: "vuechart-example",
        },
        xaxis: {
          categories: [1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998],
        },
      },
      series: [
        {
          name: "series-1",
          data: [30, 40, 35, 50, 49, 60, 70, 91],
        },
      ],
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
      try {
        const responseUser = await fetch(
          "https://skinky.herokuapp.com/https://api.steampowered.com/ISteamUser/GetPlayerSummaries/v2/?key=708CFC659DA8AF1B52841691D92B69AF&format=json&steamids=76561198360105612"
        );

        this.playerSteamInfo = await responseUser.json();
      } catch (error) {
        console.log(error);
      }
    },
  },
};
</script>

<style>
@import url("../components/playerInfo.css");

.container {
  height: auto;
  width: 100vw;
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 30px;
  background-image: url("/back.webp");
  background-repeat: no-repeat;
  background-size: cover;
}
</style>
