
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
      <div class="case" v-for="i in 9" :key=i :data-value=i v-on:click="jouer"></div>
    </div>
    <p>{{ this.joueur[0].joueur }} {{ this.joueur[0].symbol }} {{ this.aff_player2 }}</p>
    <p>{{ this.joueur[1].joueur }} {{ this.joueur[1].symbol }} {{ this.aff_player1 }}</p>



  </div>
  <div class="gagant" v-if="step == 3">
    <h1>tu as gagné {{ this.gagne }}</h1>
    <button v-on:click="rejoue">REJOUE</button>
  </div>

</template>

<script>



export default {
  data() {
    return {
      aff_player1: "",
      aff_player2: "",
      nb_cell: 0,
      row: 3,
      col: 3,
      step: 1,
      gagne: "",
      joueur: [{
        joueur: "",
        score: 0,
        symbol: "X",
        player1: "c'est a toi de jouer",
        player2: "",
        case_jouer: []
      }, {
        joueur: "",
        score: 0,
        symbol: "O",
        player2: "c'est a toi de jouer",
        player1: "",
        case_jouer: []
      }],
      tour_jeux: 1,
      control: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
  },

  methods: {
    plus() {
      this.step = this.step + 1;
      this.affich_joueur();
    },

    affich_joueur() {
      let index = this.tour_jeux % 2;
      this.aff_player1 = this.joueur[index].player1;
      this.aff_player2 = this.joueur[index].player2;
    },

    jouer(event) {

      let val = event.target.getAttribute('data-value');
      let auto = this.control.find(element => element == val);
      if (auto == val) {
        let index = this.tour_jeux % 2;
        this.affich_joueur();
        event.target.innerHTML = this.joueur[index].symbol;
        this.joueur[index].case_jouer.push(val);
        this.control.splice(val, 1, "x");
        this.tour_jeux++;
        if (this.joueur[index].case_jouer.length >= this.row) {
          this.controler(index);
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

    controler(index) { //permet de verifier le nombre de cout jouer pour la fin de partie
      let kld = this.joueur[index].case_jouer;
      if (kld.length == 3) { //cas de trois
        this.verif(kld, index);
      }

      if (kld.length > 3) { //cas plus de trois
        let tab = this.croissant(kld);

        for (let i = 0; i < tab.length; i++) {
          for (let j = i + 1; j < tab.length; j++) {
            let k = j + 1
            let temp = [];
            temp.push(tab[i]);
            temp.push(tab[j]);
            temp.push(tab[k]);
            this.verif(temp, index);
          }
        }

      }

    },
    verif(kld, index) {
      let tab_verifi = this.tab_verif();
      let tab = this.croissant(kld);
      let temp = tab.join('');
      if (tab_verifi.findIndex(element => element == temp) != -1) {
        this.gagne = this.joueur[index].joueur;
        this.step = this.step + 1;
      }


    },
    croissant(tab) {
      for (let i = 0; tab.length > i; i++)
        if (tab[i] > tab[i + 1]) {
          let temp = tab[i];
          tab[i] = tab[i + 1];
          tab[i + 1] = temp;
        }
      if (this.ordreCroissant(tab) == false) {
        this.croissant(tab);
      }
      return tab;
    },
    ordreCroissant(tableau) {
      let result = true;
      for (let index = 0; index < tableau.length; index++) {
        if (tableau[index] > tableau[index + 1]) {
          result = false;
        }
      }
      return result;
    },
    rejoue() {
      this.control = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
      this.step = 2;
      this.joueur[0].case_jouer = [];
      this.joueur[1].case_jouer = [];


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

.case {
  border: 1px solid black;
  height: 100px;
  line-height: 100px;
  font: size 60px;
}

#plateau {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 300px;
  margin: auto;
}
</style>
