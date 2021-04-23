<template>
  <section class="questions">
    <div :class="!loading ? 'questionBox' : 'loadingBox'">
      <!--  {{ loading ? 'Loading...' : currentQuestion.question }}   sono scritture equivalenti
      tuttavia se ci fossero stringhe con tag html all'interno dell'API,
      v-html decodificherebbe correttamente queste entità-->
      <h2 v-html="loading ? 'Loading...' : currentQuestion.question"></h2>
    </div>
  </section>
</template>

<script>
export default {
  name: "Question",
  props: {
    index: Number,
    currentQuestion: Object,
    loading: Boolean,
  },
};
</script>

<style scoped></style>
