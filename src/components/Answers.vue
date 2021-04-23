<template>
  <br class="loading" v-if="loading" />
  <div class="answers" v-else>
    <button
      v-for="answer in currentQuestion.answers"
      :index="currentQuestion.key"
      :key="answer"
      v-html="answer"
      @click.prevent="handleButtonClick"
    ></button>
  </div>
</template>

<script>
export default {
  name: "Answers",
  props: {
    index: Number,
    currentQuestion: Object,
    loading: Boolean,
    handleButtonClick: Function,
  },
};
</script>

<style scoped></style>
