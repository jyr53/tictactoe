
<template>
  <div class="hello" v-if="step == 1">
    <h1 class="h1">Bienvenue dans le jeu du morpion</h1>
    <h2>Un peu d'histoire</h2>
    <p>TIC TAC TOE
      : Ce jeu est d’origine japonaise. Il se joue dans les collèges et lycées français depuis la
      fin du XIXème. En France il est couramment appelé "jeu du morpion. Le but du jeu : faire un
      alignement de 3 cases soit horizontalement, soit verticalement, soit diagonalement en traçant un
      trait sur l’alignement réalisé </p>

    <div>
      <h2>Régle du jeu</h2>
      <ul>
        <li>Deux joueurs, sur un damier de 3 cases sur 3 .</li>
        <li>Chaque joueur est représenté par un "symbole"</li>
        <li>Un joueur utilise toujours le même "symbole"</li>
        <li>Un premier joueur pose son symbole sur une case.</li>
        <li> Puis c'est au tour de l'autre joueur de poser son
          symbole sur une case vide.</li>
        <li>Le but est de réussir à aligner ses trois symboles en horizontal, en vertical ou en diagonale. </li>
        <li>On remporte alors la partie.</li>
        <li>Si la grille est remplie et qu'aucune ligne ne comporte trois symboles identiques, les joueurs finissent par
          ex æquo.</li>
      </ul>

    </div>
    <div>
      <input class="joueur" type="text" v-model="joueur[0].joueur" placeholder="joueur 1" />

    </div>
    <div>
      <div id="v-model-select" class="">
        <select v-model="this.joueur[0].symbol">
          <option disabled value="">selectionnner un symbole</option>
          <option v-for="i in 8" :key=i :data-value=i v-on:click="select">{{  this.picked[i]  }}</option>
        </select>
        {{  this.joueur[0].symbol  }}
      </div>
      <div>
        <div v-if="block == 0" id="v-model-radiobutton">
          <input type="radio" id="one" value="pile" v-model="pileFace" :data-value=1 v-on:click="bloc" />
          <label for="one">pile</label>
          <br />
          <input type="radio" id="two" value="face" v-model="pileFace" :data-value=0 v-on:click="bloc" />
          <label for="two">face</label>
          <br />

        </div>
        <span v-if="block == 1">coté {{  this.pileFace  }}</span>
        <div v-if="commence == 1">{{  this.joueur[0].joueur  }} va commencer</div>
      </div>
    </div>
    <div>
      <input class="joueur" type="text" v-model="joueur[1].joueur" placeholder="joueur 2" />

      <div id="v-model-select" class="">
        <select v-model="this.joueur[1].symbol">
          <option disabled value="selectionnez un symbole">selectionnner un symbole</option>
          <option v-for="i in 8" :key=i :data-value=i v-on:click="select">{{  this.picked[i]  }}</option>
        </select>{{  this.joueur[1].symbol  }}

      </div>
    </div>
    <div>
      <div v-if="block == 0" id="v-model-radiobutton">
        <input type="radio" id="one" value="pile" v-model="pileFace1" :data-value=11 v-on:click="bloc" />
        <label for="one">pile</label>
        <br />
        <input type="radio" id="two" value="face" v-model="pileFace1" :data-value=10 v-on:click="bloc" />
        <label for="two">face</label>
        <br />
      </div>
      <span v-if="block == 1">coté {{  this.pileFace1  }}</span>
      <div v-if="commence == 2">{{  this.joueur[1].joueur  }} va commencer</div>

    </div>
    <div class="hazard">
      <div class="piece" v-on:click="piece"></div>
      <div>Cliker sur la piece pour savoir qui va commencer</div>
    </div>
    <button v-on:click="plus">GO</button>
    <div>Cliker sur go pour commencer a jouer</div>

    <br>
  </div>

  <div class="jeu" v-if="step == 2">
    <h1>A vous de jouer!</h1>

    <div id="joueur1" class="tabjoueur">
      <p>{{  this.joueur[0].joueur  }} {{  this.joueur[0].symbol  }} {{  this.aff_player1  }}</p>
      <p>score {{  this.joueur[0].score  }}</p>
    </div>
    <div id="plateau">
      <div class="case" v-for="i in 9" :key=i :data-value=i v-on:click="jouer"></div>
    </div>

    <div id="joueur2" class="tabjoueur">
      <p>{{  this.joueur[1].joueur  }} {{  this.joueur[1].symbol  }} {{  this.aff_player2  }}</p>
      <p>score {{  this.joueur[1].score  }}</p>
    </div>

  </div>
  <div class="gagant" v-if="step == 3">
    <h1>yesss!!!</h1>
    <h2>Tu as gagné {{  this.gagne  }}</h2>
    <br>
    <br>
    <button v-on:click="rejoue">REJOUE</button>
    <br> <br> <br> <br>
  </div>


  <div class="gagant" v-if="step == 4">
    <h1>oh no!!!</h1>
    <br>
    <br>
    <h2>Vous etes ex æquo</h2>
    <button v-on:click="rejoue">REJOUE</button>
  </div>

</template>

<script>



export default {
  data() {
    return {
      picked: ["", "❃", "⚽", "🦄", "🛸", "🎯", "🐼", "🐶", "🚗"],
      recup: ["", "☮", "☘", "💋", "@", "☀", "🚒", "♕", "♘"],
      pileFace: "",
      pileFace1: "",
      aff_player1: "",
      aff_player2: "",
      nb_cell: 0,
      row: 3,
      col: 3,
      step: 1,
      gagne: "",
      tour_jeux: 1,
      block: 0,
      commence: 0,
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
      tab_gagne: [],
      control: [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    }
  },

  methods: {
    bloc(event) {// permet  de faire le pile ou face
      let val = event.target.getAttribute('data-value');
      if (this.block == 0) {
        if (val == 1) {
          this.pileFace1 = "face";
          this.block = 1;
        }
        if (val == 0) {
          this.pileFace1 = "pile";
          this.block = 1;
        }
        if (val == 11) {
          this.pileFace = "face";
          this.block = 1;
        }
        if (val == 10) {
          this.pileFace = "pile";
          this.block = 1;
        }
      }
    },
    piece() {
      let arr = [1, 0, 1, 0, 1, 0, 1, 0, 1, 0];
      let fde = arr[Math.floor(Math.random() * 10)];
      let cote = "pile";
      if (fde == 0) { cote = "face"; }
      this.controlPiece(cote);
    },
    controlPiece(piece) {
      if (piece == this.pileFace) {
        this.tour_jeux = 0;
        this.commence = 1;
      }
      if (piece == this.pileFace1) {
        this.tour_jeux = 1;
        this.commence = 2;
      }
    },
    select(event) { //permet d'echanger les symboles pour jouer
      let indSymbol = event.target.getAttribute('data-value');
      let temp = this.picked[indSymbol];
      this.picked.splice(indSymbol, 1, this.recup[indSymbol]);
      this.recup.splice(indSymbol, 1, temp);
    },
    plus() {//pour commencer
      this.step = 2;
      this.affich_joueur(this.tour_jeux);
      this.tab_verif();
    },

    affich_joueur(index) {//permet de savoir a qui de jouer
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
      let temp = [];
      for (let n = 0; n < this.row; n++) {
        temp.push((n * this.row) + 1);
        temp.push((n * this.row) + 2);
        temp.push((n * this.row) + 3);
        this.tab_gagne.push(temp.join(''));
        temp = [];
        temp.push(n + 1);
        temp.push(n + 4);
        temp.push(n + 7);
        this.tab_gagne.push(temp.join(''))
        temp = [];
      }
      temp = [];
      temp.push(1);
      temp.push(this.row + (this.col - 1));
      temp.push(this.row * this.col);
      this.tab_gagne.push(temp.join(''));
      temp = [];
      temp.push(this.row);
      temp.push(this.row + (this.col - 1));
      temp.push(this.row + this.col + 1);
      this.tab_gagne.push(temp.join(''));
    },

    controler(index) { //permet de verifier avec le tab gagne etle nombre de cout jouer pour la fin de partie
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
      let tab = this.croissant(kld);
      let temp = tab.join('');
      if (this.tab_gagne.findIndex(element => element == temp) != -1) {
        this.gagne = this.joueur[index].joueur;
        this.step = 3;
        this.joueur[index].score = this.joueur[index].score + 1;
      }
      if (this.tour_jeux > 9 && this.step != 3) {//si pas de gagant  c'est ex eaquo
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
  background-size: 100%;
  background-image: url("/fond.jpg");
  border-collapse: collapse;
  font-weight: bold;

}

.case {
  border: 2px outset #8DAA9D;
  height: 100px;
  line-height: 100px;
  background-color: white;
}

.jeu {
  color: #7b0828;
  font-weight: bold;
  height: 100%;

}

#plateau {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  width: 300px;
  margin: auto;
  font-size: 100px;
}

.hazard {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
}

.piece {
  width: 30px;
  height: 30px;
  writing-mode: horizontal-tb;
  background-image: url("/euro.svg");
  background-repeat: no-repeat;
  background: size 5%;
  cursor: pointer;
}

.gagnant {
  color: #522b47;
  height: 100vh;
  width: 100vw;
}
</style>
