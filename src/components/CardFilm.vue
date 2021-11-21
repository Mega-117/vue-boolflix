<template>
  <div class="img-card-container">
    <img
      class="poster"
      :src="
        posterPath === null
          ? imgNonDisponibile
          : urlApiBaseImg + dimensioneImgApi + posterPath
      "
      alt=""
      :class="posterPath === null ? 'poster-img-non-disponibile' : ''"
    />

    <div class="card-testo p-5 text-white text-center">
      <p v-if="title ? (titolo = title) : (titolo = name)">
        Titolo: {{ titolo }}
      </p>
      <p
        v-if="title !== originalTitle ? (titoloOriginale = originalTitle) : ''"
      >
        Titolo originale: {{ originalTitle }}
      </p>
      <div class="p-2">
        Lingua:

        <!-- {{ elemento.original_language }} -->

        <img
          class="bandiere"
          :src="
            lingueSuportate.includes(originalLanguage)
              ? bandiere[originalLanguage]
              : bandiere.globale
          "
          alt=""
        />
      </div>
      <!-- voto {{ elemento.vote_average }} -->
      <!-- voto stelle {{ Math.round(elemento.vote_average / 2) }} -->

      <p>
        Voto:
        <i
          class="fa fa-star"
          aria-hidden="true"
          v-for="(elementoStella, i) in Math.round(voteAverage / 2)"
          :key="'A' + i"
        ></i>
        <i
          class="fa fa-star-o"
          aria-hidden="true"
          v-for="(elementoStellaVuota, i) in 5 - Math.round(voteAverage / 2)"
          :key="'B' + i"
        ></i>
      </p>
      <!-- voto stelle {{ Math.round(elemento.vote_average / 2) }} -->

      <hr />
      <div class="overview">
        <p>{{ overview }}</p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "CardFilm",
  props: [
    "urlApiBaseImg",
    "dimensioneImgApi",
    "lingueSuportate",
    "imgNonDisponibile",
    "bandiere",
    "posterPath",
    "title",
    "name",
    "originalTitle",
    "originalName",
    "originalLanguage",
    "voteAverage",
    "overview",
  ],
  data() {
    return {
      titolo: "",
      titoloOriginale: "",
    };
  },
};
</script>

<style lang="scss">
.img-card-container {
  position: relative;
  min-width: 310px;
  height: 100%;
  background: white;

  .card-testo {
    height: 100%;
    width: 100%;
    position: absolute;
    top: 0;
    left: 0;
    background-color: black;
    opacity: 0;
    transition: opacity 0.3s;
    .bandiere {
      width: 30px;
    }
    .overview {
      overflow: auto;
      height: 220px;
    }
  }
  .card-testo:hover {
    opacity: 0.9;
  }
  .poster {
    width: 100%;
    height: 100%;
  }
  .poster-img-non-disponibile {
    object-fit: contain;
  }
}
</style>