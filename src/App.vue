
<template>
  <div class="hello" v-if="step == 1">
    <h1>Bienvenu dans le jeu de morpion</h1>

    <div>
      <p>regle du jeux</p>
    </div>
    <input name="joueur 1" type="text" v-model="joueur1" placeholder="joueur 1" />
    <input name="joueur 2" type="text" v-model="joueur2" placeholder="joueur 2" />
    <div></div>
    <button v-on:click="plus">GO</button>

  </div>

  <div class="jeu" v-if="step == 2">
    <h1>a vous de jouer</h1>
    <p>{{ joueur1 }}</p>
    <p>{{ joueur2 }}</p>
  </div>

</template>

<script>


export default {
  data() {
    return {
      step: 1,
      joueur1: "joueur1",
      joueur2: "joueur2",
    }
  },

  methods: {
    plus() {
      this.step = this.step + 1;
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
</style>
