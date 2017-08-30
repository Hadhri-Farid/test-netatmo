<template>
<div id="app">

  <input @input="filtre($event)" class="search" type="text" placeholder="Recherche">

  <div class="conversation">
    <ul>
      <li class="echange" v-for="res in echanges">
        <div class="question">
          {{ res.question }} ?
        </div>
        <div class="reponse">
          <h2>{{ res.reponse.answer }}</h2>
          <img :src="res.reponse.image" alt="wtf">
        </div>
      </li>
    </ul>
  </div>

  <input class="message" @keyup.enter="getResponse($event)" type="text" placeholder="votre question ici">
  <button @click="getResponse($event)" type="button" name="button">Ok</button>
</div>
</template>

<script>
export default {
  name: 'app',
  data() {
    return {
      echanges: [],
      backup: null
    }
  },
  // created() {
  //   // this.getResponse();
  // },
  methods: {
    getResponse(evt) {
      // Je n'ai pas compris pourquoi http(s) (aide de mon prof ici)
      const url = "https://yesno.wtf/api/";
      // compatibilité mozilla / microsoft (google accepte les deux)
      const src = evt.target || evt.srcElement;
      // effectue un appel AJAX sur l'url
      const xhr = new XMLHttpRequest();
      xhr.open("GET", url);
      // log(evt) pour connaitre le chemin de la value
      console.log(evt);
      // fat arrow pour ne pas perdre le contexte de this
      xhr.onload = (e) => {
        const res = JSON.parse(e.target.response);
        // this.img = res.image;
        // this.text = res.answer;
        console.log(res);
        this.echanges.push({
          question: src.value,
          reponse: {
            answer: res.answer,
            image: res.image,
          }
        });
        this.backup = this.echanges;
        src.value = "";
      }

      xhr.send();
    },
    filtre(e) {
      var src = e.target;
      this.echanges = this.echanges.filter(function(echange) {
        return echange.question.toLowerCase().match(src.value) !== null;
      });

      if (!this.echanges.length || !src.value) {
        this.echanges = this.backup;
      }
      console.log(res);
    }

  }

}
</script>

<style lang="scss">
.app {
    width: 100vw;
    height: 100vh;
}
.echange {
    display: flex;
}
.echange .question,
.echange .reponse {
    flex: 1;
    display: flex;
    justify-content: center;
    align-items: center;
}
.echange .question {
    font-size: 2rem;
    border: 1px solid black;
}
.echange .reponse {
    display: flex;
    flex-direction: column;
    width: 200px;
    height: 200px;
    border: 1px solid black;
}

.message {
    width: 50vh;
    background: lightgrey;
    margin: 0 auto;
    display: block;
}
</style>
