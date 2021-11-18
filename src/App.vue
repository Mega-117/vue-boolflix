<template>
  <div id="app">
    <input v-model="parolaDaCercare" @keyup.enter="ricercaMovie" type="text" />
    <button @click="ricercaMovie">cerca</button>
    <ul>
      <li v-for="elemento in arrRispostaApiMovie" :key="elemento.id">
        titolo{{ elemento.title }} <br />
        titolo originale{{ elemento.original_title }} <br />
        lingua {{ elemento.original_language }} <br />
        voto {{ elemento.vote_average }}
        <hr />
      </li>
    </ul>
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      urlApiBase: "https://api.themoviedb.org/3",
      chiaveApi: "61cc31731d807d15a32cde73a023a2b5",
      arrRispostaApiMovie: [],
      parolaDaCercare: "",

      bandiere: {
        it: "FlagIt.png",
        en: "./assets/FlagUk.png",
        de: "./assets/FlagDe.png",
        es: "./assets/FlagEs.png",
        jp: "./assets/FlagJp.png",
      },
    };
  },
  methods: {
    ricercaMovie() {
      this.chiamataApi(
        "/search/movie",
        this.parolaDaCercare,
        "arrRispostaApiMovie"
      );
    },

    chiamataApi(urlRichiestaApi, queryDaCercare, arrDovePushare) {
      //axios.get(urlapi che è omposto da [urlbaseapi] + [il comando per la richiesta ] + [la chiave api] + [e la query])
      /*  versione senza param
          axios.get(this.urlApiBase + "/search/movie" + "?api_key=" + this.chiaveApi + "&query=ciao"
          ); */
      axios
        .get(this.urlApiBase + urlRichiestaApi, {
          params: {
            api_key: this.chiaveApi,
            query: queryDaCercare,
          },
        })
        .then((respose) => {
          debugger;
          this[arrDovePushare] = respose.data.results;
        });
    },
  },
  mounted() {},
};
</script>

<style lang="scss">
@import "@/styles/app.scss";
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
</style>
