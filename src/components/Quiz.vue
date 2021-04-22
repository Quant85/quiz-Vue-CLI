<template>
  <div class="quiz-container">
    <header id="quiz-header">
      <h1>{{ msg }}</h1>
      <img
        id="logo-quiz"
        alt="Vue logo"
        src="../assets/logo.png"
        style="max-width: 12rem"
      />
    </header>
    <main id="quiz-main">
      <Question
        :index="index"
        :currentQuestion="currentQuestion"
        :loading="loading"
      />
      <Answers
        :index="index"
        :currentQuestion="currentQuestion"
        :loading="loading"
        :handleButtonClick="handleButtonClick"
      />
    </main>
  </div>
</template>

<script>
import Question from "@/components/Question";
import Answers from "@/components/Answers";

export default {
  name: "Quiz",
  components: { Answers, Question },
  props: {
    msg: String,
  },
  data() {
    return {
      questions: Array,
      loading: true,
      index: 0, //questo parametro potrei randomizzarlo anche se gia ad ogni chiamata viene pescato un pull random di 10 domande dall'API
    };
  },
  computed: {
    currentQuestion() {
      if (this.questions !== Array) {
        //console.log(this.questions[this.index]);
        return this.questions[this.index];
      }
      return null;
    },
  },
  methods: {
    async fetchQuestion() {
      this.loading = true;
      /*Ci serviremo dell' API Trivia per ottenere le risorse necessarie a strutturare il quiz,
       * ma potremmo costruire un nostro file json allocato in una cartella api ed effettuare la chiamata all'oggetto costruito al suo interno*/
      let response = await fetch(
        "https://opentdb.com/api.php?amount=10&category=9"
      );
      //convertiamo in json
      let jsonResponse = await response.json();
      //questo indice identifica la singola risposta fa da counter per dare il valore a key
      let index = 0;
      //grazie a map creiamo un nuovo arrey nel quale definiamo l'elemento answers come array delle risposte sia corrette che non corrette per tutti gli elementi mappati
      this.questions = jsonResponse.results.map((question) => {
        question.answers = [
          question.correct_answer,
          ...question.incorrect_answers,
        ];

        //rimescoliamo in modo random le risposte per evitare che sia sempre la prima quella corretta
        let i = question.answers.length - 1;
        for (i; i > 0; i--) {
          //console.log(i);
          //console.log(question.answers);
          //console.log(question.answers[i]);
          const j = Math.floor(Math.random() * (i + 1));
          [question.answers[i], question.answers[j]] = [
            question.answers[j],
            question.answers[i],
          ];
        }

        //aggiungiamo ulteriori parametri a question che identifichino la risposta esatta e la leghino alla domanda

        question.rightAnswer = null;
        question.key = index;
        index++;
        return question;
      });
      this.loading = false;
    },
    handleButtonClick: function (event) {
      /*associa la risposta all'indice della domanda*/
      let index = event.target.getAttribute("index");

      /*catturo il contenuto associato - answer*/
      let pollutedUserAnswer = event.target.innerHTML;
      /*innerHTML può esser contaminato da elementi in Unicode quale &#039; -
      si applica il metodo replace per poter sostituire tali contaminazione*/
      //console.log(pollutedUserAnswer, index);
      /*associamo alla domanda individuata con index la risposta selezionata dall'utente*/
      this.questions[index].userAnswer = pollutedUserAnswer.replace(
        /'/,
        "&#039;"
      );
      //console.log(this.questions[index]);

      /*all'elemento selezionato/cliccato agiungiamo la un valore di classe*/
      event.target.classList.add("clicked");

      /*catturo la NodeList con gli elementi(Button) identificati dal valore index contrassegnato*/
      let allButtons = document.querySelectorAll(`[index="${index}"]`);
      //console.log(allButtons);

      /*disabilito tutti i button eccetto quello selezionato*/

      allButtons.forEach((e) => {
        //console.log(e, event.target);
        if (e !== event.target) {
          e.setAttribute("disabled", "");
        }
      });

      /*Vado ad invocare il metodo checkAnswer*/
      this.checkAnswer(event, index);
    },
    checkAnswer: function (event, index) {
      let question = this.questions[index];
      let x = this.index;
      let y = this.questions.length - 1;

      /*temporizzazione per passare alla domanda sucessiva*/
      if (question.userAnswer) {
        if (x < y) {
          setTimeout(
            function () {
              this.index += 1;
            }.bind(this),
            4000
          );
        }
        let correctAnswer = question.correct_answer;
        /*restituzione visiva del risultato della scelta*/
        if (question.userAnswer === correctAnswer) {
          event.target.classList.add("rightAnswer");
          question.rightAnswer = true;
        } else {
          event.target.classList.add("wrongAnswer");
          question.rightAnswer = false;
        }

        /*Mostriamo la risposta corretta*/
        let allButtons = document.querySelectorAll(`[index="${index}"]`);

        allButtons.forEach((e) => {
          if (e.innerHTML === correctAnswer) {
            e.classList.add("showRightAnswer");
          }
        });
      }
    },
  },
  mounted() {
    this.fetchQuestion();
  },
};
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
h3 {
  margin: 40px 0 0;
}
ul {
  list-style-type: none;
  padding: 0;
}
li {
  display: inline-block;
  margin: 0 10px;
}
a {
  color: #42b983;
}
</style>
