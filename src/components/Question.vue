<template>
  <section class="questions">
    <hr class="divider" />
    <div>
      <!--  {{ loading ? 'Loading...' : currentQuestion.question }}   sono scritture equivalenti
      tuttavia se ci fossero stringhe con tag html all'interno dell'API,
      v-html decodificherebbe correttamente queste entità-->
      <h2 v-html="loading ? 'Loading...' : currentQuestion.question"></h2>
    </div>
    <hr class="divider" />
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
