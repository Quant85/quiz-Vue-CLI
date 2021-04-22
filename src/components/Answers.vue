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

<style scoped>
button {
  font-size: 1.1rem;
  box-sizing: border-box;
  padding: 1rem;
  margin: 0.3rem;
  width: 47%;
  background-color: rgba(100, 100, 100, 0.3);
  border: none;
  border-radius: 0.4rem;
  box-shadow: 3px 5px 5px rgba(0, 0, 0, 0.2);
}

button:hover:enabled {
  transform: scale(1.02);
  box-shadow: 0 3px 3px 0 rgba(0, 0, 0, 0.14), 0 1px 7px 0 rgba(0, 0, 0, 0.12),
    0 3px 1px -1px rgba(0, 0, 0, 0.2);
}

button:focus {
  outline: none;
}

button:active:enabled {
  transform: scale(1.05);
}
</style>
