
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
      <div v-for="i in 9" :key=i :data-value=i v-on:click="jouer">| {{  }} | </div>
    </div>
    <p>{{ this.joueur[0].joueur1 }}</p>
    <p>{{ this.joueur[1].joueur2 }}</p>
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
      bloc: 0,
      pion: " ",
      step: 1,
      joueur: [{
        joueur1: "",
        symbol: "X",
        case_jouer: []
      }, {
        joueur2: "",
        symbol2: "O",
        case_jouer2: []
      }],
      tour_jeux: 1
    }
  },

  methods: {
    plus() {
      this.step = this.step + 1;

    },
    jouer(event) {

      if (this.tour_jeux == 1 && this.bloc == 0) {
        this.pion = this.joueur[0].symbol;
        this.joueur[0].case_jouer.push(event.target.getAttribute('data-value'));
        console.log(event.target.getAttribute('data-value'));
        this.tour_jeux++;
        this.bloc = 1;
      }
      if (this.tour_jeux == 2 && this.bloc == 0) {
        this.pion = this.joueur[1].symbol2;
        console.log(this.pion);
        this.joueur[1].case_jouer2.push(this.id);
        this.tour_jeux--;
      }
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
