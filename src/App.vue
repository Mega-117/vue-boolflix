<template>
  <div id="app">
    <input
      v-model="parolaDaCercare"
      @keyup.enter="ricercaMovie(), ricercaSerieTv()"
      type="text"
    />
    <button @click="ricercaMovie(), ricercaSerieTv()">cerca</button>

    <div class="container">
      <div class="row row-cols-2">
        <div class="col">
          <h2>Film</h2>
          <!-- film -->
          <ul>
            <li v-for="elemento in arrRispostaApiMovie" :key="elemento.id">
              titolo{{ elemento.title }} <br />
              titolo originale{{ elemento.original_title }} <br />
              lingua {{ elemento.original_language }}

              <img
                class="bandiere"
                :src="
                  lingueSuportate.includes(elemento.original_language)
                    ? bandiere[elemento.original_language]
                    : bandiere.globale
                "
                alt=""
              /><br />
              voto {{ elemento.vote_average }}
              <hr />
            </li>
          </ul>
        </div>
        <div class="col">
          <h2>Serie</h2>
          <ul>
            <li v-for="elemento in arrRispostaApiSerieTv" :key="elemento.id">
              <img
                :src="urlApiBaseImg + dimensioneImgApi + elemento.poster_path"
                alt=""
              />
              <br />
              titolo: {{ elemento.name }} <br />
              titolo: originale{{ elemento.original_name }} <br />
              lingua: {{ elemento.original_language }}

              <img
                class="bandiere"
                :src="
                  lingueSuportate.includes(elemento.original_language)
                    ? bandiere[elemento.original_language]
                    : bandiere.globale
                "
                alt=""
              /><br />
              voto {{ elemento.vote_average }}
              <hr />
            </li>
          </ul>
        </div>
      </div>
    </div>

    <!-- serie tv -->

    <!-- <img
      src="https://banner2.cleanpng.com/20180404/rgq/kisspng-earth-computer-icons-planet-symbol-earth-5ac577c7c43086.5353773315228906958036.jpg"
      alt=""
    /> -->
  </div>
</template>

<script>
import axios from "axios";

export default {
  name: "App",
  data() {
    return {
      urlApiBase: "https://api.themoviedb.org/3",
      urlApiBaseImg: "https://image.tmdb.org/t/p/",
      chiaveApi: "61cc31731d807d15a32cde73a023a2b5",
      arrRispostaApiMovie: [],
      arrRispostaApiSerieTv: [],

      parolaDaCercare: "",

      lingueSuportate: ["it", "en", "es", "de", "jp"],
      bandiere: {
        it: "https://upload.wikimedia.org/wikipedia/commons/c/ca/Bandiera_italiana_foto.svg",
        en: "https://upload.wikimedia.org/wikipedia/commons/thumb/a/ae/Flag_of_the_United_Kingdom.svg/310px-Flag_of_the_United_Kingdom.svg.png",
        de: "https://www.wall-art.de/out/pictures/generated/product/2/780_780_80/2586-wandtattoo-deutschland-flagge-einzel-2.jpg",
        es: "https://www.resolfin.com/wp-content/uploads/2018/02/spagna-100x150.png",
        jp: "https://shop.magicgamesparty.com/wp-content/uploads/2019/01/62249-Bandiera-giappone-90x150cm.jpg",
        globale:
          "data:image/png;base64,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",
      },
      dimensioneImgApi: "/w154",
      imgNonDisponibile:
        "https://www.benimobili.it/resources/img/img_non_disponibile.jpg?v=202111121350",
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
    ricercaSerieTv() {
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
