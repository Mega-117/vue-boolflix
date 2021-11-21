<template>
  <div id="app">
    <Nav
      @avvioRicerva="valoreRicerca($event), ricercaMovie(), ricercaSerieTv()"
    ></Nav>

    <div>
      <div class="container">
        <div class="row">
          <div class="col pt-5">
            <h2 v-if="filmStampati" class="text-white">Film</h2>
            <div class="row row-cols-4">
              <div
                class="col p-0"
                v-for="elemento in arrRispostaApiMovie"
                :key="elemento.id"
              >
                <CardFilm
                  :urlApiBaseImg="urlApiBaseImg"
                  :dimensioneImgApi="dimensioneImgApi"
                  :lingueSuportate="lingueSuportate"
                  :imgNonDisponibile="imgNonDisponibile"
                  :bandiere="bandiere"
                  :posterPath="elemento.poster_path"
                  :title="elemento.title"
                  :originalTitle="elemento.original_title"
                  :name="elemento.name"
                  :originalName="elemento.original_name"
                  :originalLanguage="elemento.original_language"
                  :voteAverage="elemento.vote_average"
                  :overview="elemento.overview"
                ></CardFilm>
              </div>
            </div>
          </div>
        </div>
        <!-- film -->

        <div class="row">
          <!-- serie tv -->
          <div class="col pt-5">
            <h2 v-if="serieStampate" class="text-white">Serie</h2>
            <div class="row row-cols-4 pb-5">
              <div
                class="col p-0"
                v-for="elemento in arrRispostaApiSerieTv"
                :key="elemento.id"
              >
                <CardFilm
                  :urlApiBaseImg="urlApiBaseImg"
                  :dimensioneImgApi="dimensioneImgApi"
                  :lingueSuportate="lingueSuportate"
                  :imgNonDisponibile="imgNonDisponibile"
                  :bandiere="bandiere"
                  :posterPath="elemento.poster_path"
                  :name="elemento.name"
                  :originalName="elemento.original_name"
                  :originalLanguage="elemento.original_language"
                  :voteAverage="elemento.vote_average"
                  :overview="elemento.overview"
                ></CardFilm>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from "axios";
import Nav from "./components/Nav.vue";

import CardFilm from "./components/CardFilm.vue";
//import CardSerie from "./components/CardSerie.vue";

export default {
  name: "App",
  data() {
    return {
      urlApiBase: "https://api.themoviedb.org/3",
      urlApiBaseImg: "https://image.tmdb.org/t/p/",
      chiaveApi: "61cc31731d807d15a32cde73a023a2b5",
      arrRispostaApiMovie: [],
      arrRispostaApiSerieTv: [],
      filmStampati: false,
      serieStampate: false,

      parolaDaCercare: "",

      lingueSuportate: ["it", "en", "es", "de", "ja"],
      bandiere: {
        it: require("./assets/FlagIt.png"),
        en: require("./assets/FlagUk.png"),
        de: require("./assets/FlagDe.png"),
        es: require("./assets/FlagEs.png"),
        ja: require("./assets/FlagJp.png"),
        globale: require("./assets/Terra.png"),
      },
      dimensioneImgApi: "/w342",
      imgNonDisponibile:
        "https://www.carlesistrumenti.it/wp-content/uploads/2018/08/LD20-Hydro-Idrofono.jpg",

      votoConStelle: "",
    };
  },
  components: {
    Nav,
    CardFilm,
    //CardSerie,
  },
  methods: {
    ricercaMovie() {
      this.filmStampati = true;
      this.chiamataApi(
        "/search/movie",
        this.parolaDaCercare,
        "arrRispostaApiMovie"
      );
    },
    ricercaSerieTv() {
      this.serieStampate = true;
      this.chiamataApi(
        "/search/tv",
        this.parolaDaCercare,
        "arrRispostaApiSerieTv"
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
          //debugger;
          this[arrDovePushare] = respose.data.results;

          /* this.lingue = respose.data.Object.keys.results.original_language; */
        });
    },
    valoreRicerca(parolaDaCercare) {
      //debugger;
      this.parolaDaCercare = parolaDaCercare;
    },
  },
  mounted() {},
  computed: {},
};
</script>

<style lang="scss">
@import "@/styles/app.scss";
#app {
  background-color: #555555;
  min-height: 100vh;
}
</style>
