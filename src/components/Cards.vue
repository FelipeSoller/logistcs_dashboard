<template>
  <div class="container mt-5">
    <div class="row">
      <div class="col-md-6">
        <div class="card text-center">
          <div class="card-header bg-danger text-white">
            <div class="row">
              <div class="col">
                <h3 class="text-center">TOTAL DE MULTAS</h3>
              </div>
            </div>
          </div>
          <div class="card-footer">
              <p class="mt-3">R$: {{ cardInfos.totalFines }}</p>
          </div>
        </div>
      </div>

      <div class="col-md-6">
        <div class="card text-center">
          <div class="card-header bg-success text-white">
            <div class="row">
              <div class="col">
                <h3 class="text-center">TOTAL PERCORRIDO</h3>
              </div>
            </div>
          </div>
          <div class="card-footer">
              <p class="mt-3">{{ cardInfos.totalKmTraveled }} Km</p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: "Cards",
  props: {
    cardInfos: Array,
    logDay: Array,
  },
};
</script>

<style scoped>
@import url("https://fonts.googleapis.com/css2?family=Antonio&display=swap");
@import url('https://fonts.googleapis.com/css2?family=Zen+Dots&display=swap');

h3 {
    font-family: "Antonio", sans-serif;
    letter-spacing: .2rem;
}

p {
    font-family: 'Zen Dots', cursive;
    font-size: 34px;
    letter-spacing: .2rem;
}
</style>
