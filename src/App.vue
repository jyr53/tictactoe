
<template>
  <div class="hello" v-if="step == 1">
    <h1>Bienvenu dans le jeu de morpion</h1>

    <div>
      <p>regle du jeux</p>

    </div>
    <input name="joueur1" type="text" v-model="joueur[0].joueur" placeholder="joueur 1" />
    <input name="joueur2" type="text" v-model="joueur[1].joueur" placeholder="joueur 2" />
    <div></div>
    <button v-on:click="plus">GO</button>


  </div>

  <div class="jeu" v-if="step == 2">
    <h1>a vous de jouer</h1>
    <div id="plateau">
      <div v-for="i in 9" :key=i :data-value=i v-on:click="jouer">| | </div>
    </div>
    <p>{{ this.joueur[0].joueur }} {{ this.joueur[0].symbol }}</p>
    <p>{{ this.joueur[1].joueur }} {{ this.joueur[1].symbol }}</p>
    <button v-on:click="plus">GO</button>
  </div>
  <div class="gagant" v-if="step == 3">
    <h1>tu as gagné {{ this.gagne }}</h1>
  </div>

</template>

<script>



export default {
  data() {
    return {
      nb_cell: 0,
      row: 3,
      col: 3,
      step: 1,
      gagne: "",
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
        this.tour_jeux++;
        console.log(this.joueur[index].case_jouer.length);
        console.log(this.row);
        if (this.joueur[index].case_jouer.length >= this.row) {
          this.verif(index);
        }
      }
    },
    tab_verif() {
      let tab_gagne = [];
      let temp = [];
      for (let n = 0; n < this.row; n++) {
        temp.push((n * this.row) + 1);
        temp.push((n * this.row) + 2);
        temp.push((n * this.row) + 3);
        tab_gagne.push(temp.join(''));
        temp = [];
        temp.push(n + 1);
        temp.push(n + 4);
        temp.push(n + 7);
        tab_gagne.push(temp.join(''))
        temp = [];
      }
      temp = [];
      temp.push(1);
      temp.push(this.row + (this.col - 1));
      temp.push(this.row * this.col);
      tab_gagne.push(temp.join(''));
      temp = [];
      temp.push(this.row);
      temp.push(this.row + (this.col - 1));
      temp.push(this.row + this.col + 1);
      tab_gagne.push(temp.join(''));
      return tab_gagne;
    },

    verif(index) {
      let tab_verifi = this.tab_verif();
      let kld = this.joueur[index].case_jouer;
      console.log("jy suis");
      let ret_kld = this.croissant(kld);
      console.log(ret_kld);
      kld = ret_kld.join('');

      if (tab_verifi.findIndex(element => element == kld) != -1) {
        this.gagne = this.joueur[index].joueur;
        console.log(this.joueur[index].joueur);
        this.step = this.step + 1;


      }
      console.log(kld);
      console.log(tab_verifi);

    },
    croissant(tab) {
      for (let i = 0; tab.length < i; i++)
        if (tab[i] > tab[i + 1]) {
          let temp = tab[i];
          tab[i] = tab[i + 1];
          tab[i + 1] = temp;
        }
      return tab;
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
