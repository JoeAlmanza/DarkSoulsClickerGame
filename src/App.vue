<template>
  <div id="app" class="container-fluid">
    <div class="row text-center">
      <div class="col-12">
        <h1 class="text-center text-dark soulNum my-3">Soul Clicker</h1>
        <p class="text-danger text-shadow">
          Click Hellkite dragon to harvest souls
        </p>
        <img
          alt="Vue logo"
          class="mainImg"
          src="https://vignette.wikia.nocookie.net/darksouls/images/a/a6/Hellkite_Wyvern_Render.png/revision/latest?cb=20130714154316"
          height="250px"
          @click="mine"
          ondragstart="return false;"
        />
        <h2 class="text-info text-shadow">
          Souls:
          <span class="text-dark soulNum"> {{ Math.floor(souls) }}</span>
        </h2>
        <div class="row">
          <!-- CLICK UPGRADES START -->
          <div class="col-5">
            <div class="row justify-content-center">
              <h2 class="text-center text-danger text-shadow">
                Click Upgrades
              </h2>
              <div class="col-12">
                <span @mouseover="hover = true" @mouseleave="hover = false">
                  <span v-if="!hover">
                    <button
                      class="m-1"
                      v-for="item in clickUpgrades"
                      :key="item.name"
                      @click="buy(item)"
                    >
                      <img class="btnImg" :src="item.imgUrl" />
                      <br />
                      <b
                        ><u>{{ item.name }}</u></b
                      >
                      <br />
                      <b class="text-success price">
                        ${{ Math.floor(item.cost) }}</b
                      >
                      <br />
                      <b class="itemBonus soulNum">+{{ item.multiplier }}</b>
                    </button>
                  </span>
                  <span v-if="hover">
                    <button
                      class="m-1"
                      v-for="item in clickUpgrades"
                      :key="item.name"
                      @click="buy(item)"
                    >
                      <img class="btnImg" :src="item.imgUrl" />
                      <br />
                      <b class="text-success price">
                        ${{ Math.floor(item.cost) }}</b
                      >
                      <br />
                      <b class="itemBonus soulNum">+{{ item.multiplier }}</b>
                      <p>/ click x {{ item.quantity }}</p>
                      <p>
                        (Total:
                        <span class="soulNum"
                          >+{{ item.quantity * item.multiplier }}</span
                        >
                        / click)
                      </p>
                    </button>
                  </span>
                </span>
              </div>
            </div>
          </div>
          <!-- CLICK UPGRADES END -->
          <!-- EQUIPMENT UPGRADES START -->
          <div class="col-2">
            <h4 class="text-danger text-shadow mt-4">Equipment</h4>
            <div v-if="!this.equipment.covetousSilverSerpentRing.equipped">
              <span @mouseover="hover = true" @mouseleave="hover = false">
                <button
                  class="m-1"
                  v-for="item in equipment"
                  :key="item.name"
                  @click="equip(item)"
                >
                  <img class="btnImg" :src="item.imgUrl" />
                  <br />
                  <b
                    ><u>{{ item.name }}</u></b
                  >
                  <br />
                  <span v-if="hover">
                    <p>{{ item.description }}</p>
                  </span>
                  <b class="text-success price">
                    ${{ Math.floor(item.cost) }}</b
                  >
                </button>
              </span>
            </div>
            <div v-else>
              <span @mouseover="hover = true" @mouseleave="hover = false">
                <button
                  class="m-1 equippedItem"
                  v-for="item in equipment"
                  :key="item.name"
                >
                  <img class="btnImg" :src="item.imgUrl" />
                  <br />
                  <b
                    ><u>{{ item.name }}</u></b
                  >
                  <br />
                  <span v-if="hover">
                    <p>{{ item.description }}</p>
                  </span>
                  <b class="text-warning"> Equipped </b>
                </button>
              </span>
            </div>
          </div>
          <!-- EQUIPMENT UPGRADES END -->
          <!-- AUTO UPGRADES START -->
          <div class="col-5">
            <div class="row justify-content-center">
              <h2 class="text-center text-danger text-shadow">Auto Upgrades</h2>
              <div class="col-12">
                <span @mouseover="hover = true" @mouseleave="hover = false">
                  <span v-if="!hover">
                    <button
                      class="m-1"
                      v-for="item in autoUpgrades"
                      :key="item.name"
                      @click="buy(item)"
                    >
                      <img class="btnImg" :src="item.imgUrl" />
                      <br />
                      <b
                        ><u>{{ item.name }}</u></b
                      >
                      <br />
                      <b class="text-success price">
                        ${{ Math.floor(item.cost) }}</b
                      >
                      <br />
                      <b class="itemBonus soulNum">+{{ item.multiplier }}</b>
                    </button>
                  </span>
                  <span v-if="hover">
                    <button
                      class="m-1"
                      v-for="item in autoUpgrades"
                      :key="item.name"
                      @click="buy(item)"
                    >
                      <img class="btnImg" :src="item.imgUrl" />
                      <br />
                      <b class="text-success price">
                        ${{ Math.floor(item.cost) }}</b
                      >
                      <br />
                      <b class="itemBonus soulNum">+{{ item.multiplier }}</b>
                      <p>/ 2 seconds x {{ item.quantity }}</p>
                      <p>
                        (Total:
                        <span class="soulNum"
                          >+{{ item.quantity * item.multiplier }}</span
                        >
                        / 2 seconds)
                      </p>
                    </button>
                  </span>
                </span>
              </div>
            </div>
          </div>
          <!-- AUTO UPGRADES END -->
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      hover: false,
      souls: 0,
      equipment: {
        covetousSilverSerpentRing: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/4022.png",
          name: "Covetous Silver Serpent Ring",
          cost: 1000000,
          equipped: false,
          description: "Grants 20% more souls",
        },
      },
      autoUpgrades: {
        fireball: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Fireball.png",
          name: "Fireball",
          cost: 100,
          multiplier: 1,
          quantity: 0,
          description: "Adds +1 soul every 2 seconds for each Fireball owned",
        },
        poisonMist: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Poison_Mist.png",
          name: "Poison Mist",
          cost: 1000,
          multiplier: 5,
          quantity: 0,
          description:
            "Adds +5 souls every 2 seconds for each Poison Mist owned",
        },
        greatChaosFireball: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Great_Chaos_Fireball.png",
          name: "Great Chaos Fireball",
          cost: 7500,
          multiplier: 25,
          quantity: 0,
          description: "Adds +15 souls every 2 seconds for each GCF owned",
        },
      },
      clickUpgrades: {
        dagger: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Wpn_Dagger.png",
          name: "Dagger",
          cost: 30,
          multiplier: 1,
          quantity: 0,
          description: "Adds +1 soul per click for each Dagger owned",
        },
        claymore: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/claymore.png",
          name: "Claymore",
          cost: 200,
          multiplier: 5,
          quantity: 0,
          description: "Adds +5 souls per click for each Claymore owned",
        },
        greatLordGreatsword: {
          imgUrl:
            "https://darksouls.wiki.fextralife.com/file/Dark-Souls/great_lord_greatsword_1.png",
          name: "Great Lord Greatsword",
          cost: 5000,
          multiplier: 25,
          quantity: 0,
          description: "Adds +25 souls per click for each GLG owned",
        },
      },
    };
  },

  computed: {
    clickModifiersTotal() {
      let total = 0;
      for (let key in this.clickUpgrades) {
        let upgrade = this.clickUpgrades[key];
        total += upgrade.multiplier * upgrade.quantity;
      }
      return total;
    },
    autoModifiersTotal() {
      let total = 0;
      for (let key in this.autoUpgrades) {
        let upgrade = this.autoUpgrades[key];
        total += upgrade.multiplier * upgrade.quantity;
      }
      return total;
    },
  },

  mounted() {
    if (localStorage.souls) {
      this.souls = localStorage.souls;
      this.souls++;
    }

    if (localStorage.getItem("autoUpgrades")) {
      try {
        this.autoUpgrades = JSON.parse(localStorage.getItem("autoUpgrades"));
      } catch (e) {
        localStorage.removeItem("autoUpgrades");
      }
    }

    if (localStorage.getItem("clickUpgrades")) {
      try {
        this.clickUpgrades = JSON.parse(localStorage.getItem("clickUpgrades"));
      } catch (e) {
        localStorage.removeItem("clickUpgrades");
      }
    }

    if (localStorage.getItem("equipment")) {
      try {
        this.equipment = JSON.parse(localStorage.getItem("equipment"));
      } catch (e) {
        localStorage.removeItem("equipment");
      }
    }

    setInterval(() => {
      this.autoMine();
    }, 2000);
  },
  methods: {
    mine() {
      this.souls++;
      if (this.equipment.covetousSilverSerpentRing.equipped) {
        this.souls += this.clickModifiersTotal * 1.2;
      } else {
        this.souls += this.clickModifiersTotal;
      }
    },

    autoMine() {
      if (this.equipment.covetousSilverSerpentRing.equipped) {
        this.souls += this.autoModifiersTotal * 1.2;
      } else {
        this.souls += this.autoModifiersTotal;
      }
    },

    buy(item) {
      if (item.cost <= this.souls) {
        this.souls -= item.cost;
        item.quantity++;
        item.cost *= 1.15;
        this.saveAutoUpgrades();
        this.saveClickUpgrades();
      }
    },

    equip(item) {
      if (item.cost <= this.souls) {
        this.souls -= item.cost;
        item.equipped = true;
        this.saveEquipment();
      }
    },

    saveAutoUpgrades() {
      const parsed = JSON.stringify(this.autoUpgrades);
      localStorage.setItem("autoUpgrades", parsed);
    },

    saveClickUpgrades() {
      const parsed = JSON.stringify(this.clickUpgrades);
      localStorage.setItem("clickUpgrades", parsed);
    },

    saveEquipment() {
      const parsed = JSON.stringify(this.equipment);
      localStorage.setItem("equipment", parsed);
    },
  },

  watch: {
    souls(souls) {
      localStorage.souls = souls;
    },
  },
};
</script>

<style>
body {
  background-image: url("https://i.imgur.com/6KTwz4W.gif");
  background-size: cover;
  background-position: center center;
  background-repeat: no-repeat;
  background-attachment: fixed;
  height: 100vh;
  font-family: Garamond, serif;
}

img {
  padding: 10px 10px;
  text-align: center;
  cursor: pointer;
  border-radius: 5px;
}

img:active {
  transform: translateY(2px);
}

p {
  margin-bottom: 0;
  font-weight: bold;
}

button {
  background-color: rgba(134, 148, 83, 0.502);
  /* background-color: rgba(102, 126, 163, 0.502); */
  padding: 10px 10px;
  text-align: center;
  cursor: pointer;
  border-radius: 5px;
  box-shadow: 2px 2px #6e6e6e, -2px -2px #6e6e6e, 2px -2px #6e6e6e,
    -2px 2px #6e6e6e;
  width: 150px;
}

.btnImg {
  max-height: 7vh;
  padding-top: 0;
  padding-bottom: 0;
}

.mainImg {
  border-radius: 50%;
}

.price {
  font-size: 16pt;
  text-shadow: 1px 1px black;
}

.text-shadow {
  text-shadow: 1px 1px black;
}

.soulNum {
  text-shadow: 1px 1px whitesmoke, 1px -1px whitesmoke, -1px -1px whitesmoke,
    -1px 1px whitesmoke;
}

.itemBonus {
  font-size: 14pt;
}

.equippedItem {
  border: 3px solid rgb(212, 195, 42);
}
</style>
