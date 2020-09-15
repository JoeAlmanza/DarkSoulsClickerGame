<template>
  <div id="app" class="container-fluid">
    <div class="row text-center">
      <div class="col-12">
      <h1 class="text-center text-info my-3"><u>Soul Clicker</u></h1>
      <p class="text-danger">Click Hellkite dragon to harvest souls</p>
      <img alt="Vue logo" class="shadow" src="https://vignette.wikia.nocookie.net/darksouls/images/a/a6/Hellkite_Wyvern_Render.png/revision/latest?cb=20130714154316" height="250px" @click="mine" />
      <h3 class="text-info">Souls: <span class="text-danger"> {{Math.floor(souls)}}</span></h3>
      <button class="mx-1 my-1" v-for="item in upgrades" :key="item.name" @click="buy(item)"><img class="btnImg" :src="item.imgUrl"/> <b><u>{{item.name}}</u></b> <p>{{item.description}}</p> <b class="text-success"> ${{Math.floor(item.cost)}}</b> (Quantity: {{item.quantity}})</button>
      </div>
    </div>
  </div>
</template>

<script>
  export default{

    mounted(){
      setInterval(()=> {
        this.autoMine()
      }, 3000);
    },
    data(){
      return{
        souls: 0,
        upgrades: {
          dagger: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Wpn_Dagger.png",
            name: "Dagger",
            cost: 10,
            multiplier: 1,
            quantity: 0,
            description: "Adds +1 soul per click for each Dagger owned",
            click: true
          },
          claymore: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/claymore.png",
            name: "Claymore",
            cost: 100,
            multiplier: 5,
            quantity: 0,
            description: "Adds +5 souls per click for each Claymore owned",
            click: true
          },
          greatLordGreatsword: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/great_lord_greatsword_1.png",
            name: "Great Lord Greatsword",
            cost: 10000,
            multiplier: 25,
            quantity: 0,
            description: "Adds +25 souls per click for each GLG owned",
            click: true
          },
          
          fireball: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Fireball.png",
            name: "Fireball",
            cost: 30,
            multiplier: 1,
            quantity: 0,
            description: "Adds +1 soul every 3 seconds for each Fireball owned",
            click: false
          },
          poisonMist: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Poison_Mist.png",
            name: "Poison Mist",
            cost: 500,
            multiplier: 5,
            quantity: 0,
            description: "Adds +5 souls every 3 seconds for each Poison Mist owned",
            click: false
          },
          greatChaosFireball: {
            imgUrl: "https://darksouls.wiki.fextralife.com/file/Dark-Souls/Great_Chaos_Fireball.png",
            name: "Great Chaos Fireball",
            cost: 1000,
            multiplier: 10,
            quantity: 0,
            description: "Adds +10 souls every 3 seconds for each GCF owned",
            click: false
          }
        },
      }
    },
    methods: {
      mine(){
        this.souls++
        this.souls += this.clickModifiersTotal
      },

      autoMine(){
        this.souls += this.autoModifiersTotal
      },

      buy(item){
        if(item.cost <= this.souls){
          this.souls -= item.cost
          item.quantity++
          item.cost *= 1.5
        }
      }
    },
    computed: {
      clickModifiersTotal(){
        let total = 0
        for(let key in this.upgrades) {
          let upgrade = this.upgrades[key]
          if(upgrade.click) {
            total += upgrade.multiplier * upgrade.quantity
          }
        }
        return total
      },
      autoModifiersTotal(){
        let total = 0
        for(let key in this.upgrades){
          let upgrade = this.upgrades[key]
          if(!upgrade.click) {
            total += upgrade.multiplier * upgrade.quantity
          }
        }
        return total
      }
    }
  }
</script>

<style>
  body{
    background-image: url("https://i.imgur.com/6KTwz4W.gif");
    background-size: cover;
    background-position: center center;
    height: 100vh;
  }

  img{
    padding: 10px 10px;
    text-align: center;
    cursor: pointer;
    border-radius: 5px;
  }

  img:active{
  transform: translateY(2px)
  }

  button{
    background-color: rgba(134, 148, 83, 0.5);
    padding: 10px 10px;
    text-align: center;
    cursor: pointer;
    border-radius: 5px;
    box-shadow: 0 2px #999;
  }
  
  .btnImg{
    max-height: 10vh;
  }

</style>