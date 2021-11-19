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
    />

    <div class="card-testo p-5 text-white text-center">
      <p>titolo: {{ title }}</p>
      <p>titolo originale: {{ originalTitle }}</p>
      <div class="p-2">
        lingua:

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
    </div>
  </div>
</template>

<script>
export default {
  name: "CardFilm",
  props: [
    //"arrRispostaApiMovie",
    "urlApiBaseImg",
    "dimensioneImgApi",
    "lingueSuportate",
    "imgNonDisponibile",
    "bandiere",
    "posterPath",
    "title",
    "originalTitle",
    "originalLanguage",
    "voteAverage",
  ],
};
</script>

<style lang="scss">
.bandiere {
  width: 20px;
  height: 20px;
}
.img-card-container {
  position: relative;
  min-width: 310px;
  //max-height: 620px;
  //height: 620px;
  height: 100%;
  background: white;

  .card-testo {
    height: 100%;
    width: 100%;
    //background-color: black;
    position: absolute;
    top: 0;
    left: 0;
    background-color: black;
    opacity: 0;
    transition: opacity 0.3s;
    img {
      width: 20px;
    }
  }
  .card-testo:hover {
    opacity: 0.9;
  }
  .poster {
    width: 100%;
    max-height: 100%;
    object-fit: cover;
  }
}
</style>