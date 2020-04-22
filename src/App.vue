<template>
  <div id="app">
    <v-app id="inspire">
      <LootViewer :monster="selectedMonster" v-model="showLootViewer"/>
      <v-toolbar>
          <v-row>
              <v-toolbar-title class="ma-2" text>MONSTERTRACKER</v-toolbar-title>
              <v-spacer></v-spacer>
              <v-btn class="ma-2" text>Home</v-btn>
              <v-btn class="ma-2" text>Loot Viewer</v-btn>
              <v-btn class="ma-2" text>Grand Exchange Price Data</v-btn>
              <v-btn class="ma-2" id="collectMonsters" @click="getMonsters" text>get monsters</v-btn>
              <v-btn class="ma-2" id="checkMonsters" @click="getMonsterDrops" text>get drops</v-btn>
          </v-row>
      </v-toolbar>
      <v-parallax id="jadParallax" height="400" dark src="./assets/hydra.png">
        <v-row align="center" justify="center">
          <v-col class="justify-center" cols="12">
            <h1 class="display-1 font-weight-thin mb-4">MONSTERTRACKER</h1>
            <h4 class="subheading">Discover the most profitable monsters in OSRS</h4>
          </v-col>
        </v-row>
      </v-parallax>

      <v-card id="monstersCard" width="1400" class="mx-auto" justify="center" align="center">
        <v-container fluid>

          <div class="search-wrapper">
            <v-toolbar>
              <v-spacer></v-spacer>
              <input type="text" placeholder="Enter Total Kills.." />
              <v-spacer></v-spacer>
              <input type="text" v-model="search" placeholder="Search Monster.." />
              <v-spacer></v-spacer>
              <v-btn class="ma-2" text>Sort by Profit</v-btn>
              <v-spacer></v-spacer>
              <v-btn class="ma-2" text>Sort by Name</v-btn>
              <v-spacer></v-spacer>
              <v-btn class="ma-2" outlined>Filters</v-btn>
              <v-spacer></v-spacer>
            </v-toolbar>
          </div>

          <v-row>
            <v-col v-for="monster in filteredList" :key="monster.title" :cols="2">
              <v-card dark max-width="250">
                <v-img
                  :src="monster.src"
                  class="white--text align-end"
                  gradient="to bottom, rgba(0,0,0,.1), rgba(0,0,0,.5)"
                  height="160px"
                  max-width="250px"
                ></v-img>

                <v-divider></v-divider>

                <v-chip>{{Number(monster.profit).toLocaleString()}}</v-chip>

                <v-flex text-xs-center>
                  <h1 id="monsterTitle" class="monsterTitle" v-text="monster.title"></h1>
                </v-flex>

                <v-card-actions>
                  <v-flex text-xs-center>
                    <v-btn outlined @click.stop="selectedMonster=monster, showLootViewer=true">View Loot</v-btn>
                  </v-flex>
                </v-card-actions>
              </v-card>
            </v-col>
          </v-row>
        </v-container>
      </v-card>
    </v-app>
  </div>
</template>


<script type="text/javascript">

class Monster {
  constructor(title, src, profit) {
    this.title = title;
    this.src = src;
    this.profit = profit;
  }
}

import HighestProfit from "./scripts/HighestProfit";
import LootViewer from "./components/LootViewer";
export default {
  created() {

  },
  data: () => ({
    selectedMonster: {},
    showLootViewer: false,
    itemPrices: {},
    search: "",
    monsterDrops: [],
    monsterList: [
      new Monster(
        "Zulrah",
        "https://oldschool.runescape.wiki/images/b/bc/Zulrah_%28serpentine%29.png?29a54",
        10000000
      ),
      new Monster(
        "Cerberus",
        "https://oldschool.runescape.wiki/images/4/45/Cerberus.png?47f4c",
        5000000
      ),
      new Monster(
        "Skotizo",
        "https://oldschool.runescape.wiki/images/a/a8/Skotizo.png?dc8b8",
        4000000
      ),
      new Monster(
        "Zalcano",
        "https://oldschool.runescape.wiki/images/3/30/Zalcano.png?6244d",
        3500000
      ),
      new Monster(
        "Vorkath",
        "https://oldschool.runescape.wiki/images/6/6d/Vorkath%27s_head_detail.png?e82eb",
        3000000
      ),
      new Monster(
        "Alchemical Hydra",
        "https://oldschool.runescape.wiki/images/a/a3/Alchemical_Hydra.png?925dd",
        2500000
      ),
      new Monster(
        "Something",
        "https://oldschool.runescape.wiki/images/6/6d/Vorkath%27s_head_detail.png?e82eb",
        2000000
      )
    ]
  }),
  components: {
    LootViewer
  },
  computed: {
    filteredList() {
      return this.monsterList.filter(monster => {
        return monster.title.toLowerCase().includes(this.search.toLowerCase());
      });
    }
  },
  methods: {
    getMonsters: function() {
      var monsters;
      monsters = HighestProfit.getSortedMonsterProfit();
      this.monsterList = monsters;
      return this.monsterList;
    },
    getMonsterDrops: function() {
      var mDrops;
      mDrops = HighestProfit.getMonsterDrops();
      console.log(mDrops);
    },
    checkMonsters: function() {
      console.log(JSON.parse(JSON.stringify(this.monsterList)));
    }
  }
};
</script>

<style lang="scss">
body {
  background-color: black;
  font-family: "Roboto", sans-serif;
  text-align: center;
  font-weight: 300;
  font-size: 20px;
}

#monstersCard {
  background-color: black;
  margin-top: 0.55rem;
}

.v-parallax {
  margin-bottom: 0.55rem;
}

.v-chip {
  margin-top: 0.55rem;
  margin-right: 0.2rem;
}

.monsterTitle {
  margin-top: 0.55rem;
  font-size: 20px;
  font-weight: 300;
}
</style>