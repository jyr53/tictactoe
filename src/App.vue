
<template>
  <div class="hello" v-if="step == 1">
    <h1>Bienvenu dans le jeu de morpion</h1>
    <h2>Un peu d'histoire</h2>
    <p>TIC TAC TOE (jeu du morpion sur plateau)
Petite Histoire : Ce jeu est d’origine japonaise. Il se joue dans les collèges et lycées français depuis la
fin du XIXème. En France il est couramment appelé "jeu du morpion. Le but du jeu : faire un
alignement de 5 cases soit horizontalement, soit verticalement, soit diagonalement en traçant un
trait sur l’alignement réalisé (son trait mord les pions) La variante proposée ici se joue sur un plateau
carré de 81 cases. Le plus souvent le jeu est pratiqué en utilisant une simple feuille de papier avec
des carreaux</p>

    <div>
      <h2>Régle du jeux</h2>

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
    <div id="joueur1">
      <p>{{ this.joueur[0].joueur }} {{ this.joueur[0].symbol }} {{ this.aff_player1 }}</p>
      <p>score {{ this.joueur[0].score }}</p>
    </div>
    <div id="joueur2">
      <p>{{ this.joueur[1].joueur }} {{ this.joueur[1].symbol }} {{ this.aff_player2 }}</p>
      <p>score {{ this.joueur[1].score }}</p>
    </div>

  </div>
  <div class="gagant" v-if="step == 3">
    <h1>tu as gagné {{ this.gagne }}</h1>
    <button v-on:click="rejoue">REJOUE</button>
  </div>


  <div class="gagant" v-if="step == 4">
    <h1>vous etes ex eaquo</h1>
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
      joueur: [{//tableau des variables en objet
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
      this.affich_joueur(1);
    },

    affich_joueur(index) {


      this.aff_player1 = this.joueur[index].player1;
      this.aff_player2 = this.joueur[index].player2;
    },

    jouer(event) {//quand on clik dans la case 

      let val = event.target.getAttribute('data-value'); //recuperation de la valeur de la case
      let auto = this.control.find(element => element == val);//est-elle dans le tableau si elle ne l'est pas elle as ete jouer
      if (auto == val) {//sinon 
        let index = this.tour_jeux % 2;//pour l'index du tableau
        this.affich_joueur(index);
        event.target.innerHTML = this.joueur[index].symbol;//on pose le symbole du joueur
        this.joueur[index].case_jouer.push(val);//on stock la case 
        this.control.splice(val, 1, "x");//on la remplace par un x
        this.tour_jeux++;
        let indeo = this.tour_jeux % 2;//pour entissiper le tour prochain
        this.affich_joueur(indeo);
        if (this.joueur[index].case_jouer.length >= this.row) {// verification si trois cout on ete jouer
          this.controler(index);
        }
      }

    },
    tab_verif() {//tableau de verification  a peaufine
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
            for (let k = j + 1; k < tab.length; k++) {

              let temp = [];
              temp.push(tab[i]);
              temp.push(tab[j]);
              temp.push(tab[k]);
              this.verif(temp, index);
            }
          }
        }

      }

    },
    verif(kld, index) {//verification si ca match avec le tableau de resultat
      let tab_verifi = this.tab_verif();
      let tab = this.croissant(kld);
      let temp = tab.join('');
      if (tab_verifi.findIndex(element => element == temp) != -1) {
        this.gagne = this.joueur[index].joueur;
        this.step = this.step + 1;
        this.joueur[index].score = this.joueur[index].score + 1;
      }
      if (this.tour_jeux > 9) {//si pas de gagant  c'est ex eaquo
        this.step = 4;
      }

    },
    croissant(tab) {//tri en ordre croissant
      for (let i = 0; tab.length > i; i++)
        if (tab[i] > tab[i + 1]) {
          let temp = tab[i];
          tab[i] = tab[i + 1];
          tab[i + 1] = temp;
        }
      if (this.ordreCroissant(tab) == false) {//verification
        this.croissant(tab);//fonction recursive
      }
      return tab;
    },
    ordreCroissant(tableau) {//verification si c'est dans l'ordre
      let result = true;
      for (let index = 0; index < tableau.length; index++) {
        if (tableau[index] > tableau[index + 1]) {
          result = false;
        }
      }
      return result;
    },
    rejoue() {//reinitialisation des varibles pour rejouer
      this.control = [0, 1, 2, 3, 4, 5, 6, 7, 8, 9];
      this.step = 2;
      this.joueur[0].case_jouer = [];
      this.joueur[1].case_jouer = [];
      this.tour_jeux = 1;
      this.affich_joueur(1);
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

}

#plateau {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 300px;
  margin: auto;
  font-size: 1.2em;
}
</style>
