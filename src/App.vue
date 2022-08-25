
<template>
  <div class="hello" v-if="step == 1">
    <h1>Bienvenu dans le jeu de morpion</h1>

    <div>
      <p>regle du jeux</p>

    </div>
    <input name="joueur1" type="text" v-model="joueur[0].joueur1" placeholder="joueur 1" />
    <input name="joueur2" type="text" v-model="joueur[1].joueur2" placeholder="joueur 2" />
    <div></div>
    <button v-on:click="plus">GO</button>


  </div>

  <div class="jeu" v-if="step == 2">
    <h1>a vous de jouer</h1>
    <div id="plateau">
      <div v-for="i in 9" :key=i :data-value=i v-on:click="jouer">| | </div>
    </div>
    <p>{{ this.joueur[0].joueur1 }} {{ this.joueur[0].symbol }}</p>
    <p>{{ this.joueur[1].joueur2 }} {{ this.joueur[1].symbol }}</p>
    <button v-on:click="plus">GO</button>
  </div>
  <div class="gagant" v-if="step == 3">
    <h1>tu as gagné {{ }}</h1>
  </div>

</template>

<script>



export default {
  data() {
    return {
      nb_cell:0,
      row:3,
      col:3,
      step: 1,
      joueur: [{
        joueur: "",
        score: 0,
        symbol: "X",
        case_jouer: []
      }, {
        joueur: "",
        score: 0,
        symbol: "O",
        case_jouer: []
      }],
      tour_jeux: 1,
      control: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
  },

  methods: {
    plus() {
      this.step = this.step + 1;
    },
    jouer(event) {

      let val = event.target.getAttribute('data-value');
      let auto = this.control.find(element => element == val);
      if (auto == val) {
        let index = this.tour_jeux % 2;
        event.target.innerHTML = this.joueur[index].symbol;
        this.pion = this.joueur[index].symbol;
        this.joueur[index].case_jouer.push(val);
        this.control.splice(val, 1, "x");
        this.verif(index);
        this.tour_jeux++;

      }
    },
    verif(index) {
      this.nb_cell=this.row*this.col;
      let tab_gagne:[];
      for (let n=0 ;n < this.row;n++){
        
      }
      let kld = this.joueur[index].case_jouer;
      console.log(kld);
    }
  }
}



</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}

#plateau {
  display: grid;
  grid-auto-columns: minmax(3, auto);
  grid-auto-rows: auto(3);
}
</style>
