<template>
  <div>
    <nav class="py-4 px-3 d-flex justify-content-between">
      <h1>boolflix</h1>
      <div class="d-flex align-items-center">
        <input
          v-model="parolaDaCercare"
          @keyup.enter="$emit('avvioRicerva', parolaDaCercare)"
          type="text"
        />
        <button @click="$emit('avvioRicerva', parolaDaCercare)">Cerca</button>
      </div>
    </nav>
  </div>
</template>

<script>
export default {
  name: "Nav",
  data() {
    return {
      parolaDaCercare: "",
    };
  },
};
</script>

<style lang="scss">
nav {
  h1 {
    color: red;
    text-transform: uppercase;
  }

  button {
    color: white;
    background-color: red;
    border-color: red;
    border-radius: 2px;
  }

  background-color: #000000;
}
</style>