<template>
  <div id="app">
    <h1>Super Quiz</h1>

    <transition name="flip" mode="out-in">
      <Question
        v-if="questionMode"
        :question="questions[currentQuestion]"
        @answered="showResult"
      />
      <Result v-else :result="result" @confirmed="nextQuestion" />
    </transition>
  </div>
</template>

<script>
import Questions from "./util/questions";
import Question from "./components/Question.vue";
import Result from "./components/Result.vue";

export default {
  components: { Question, Result },
  data() {
    return {
      result: false,
      questionMode: true,
      questions: Questions,
      currentQuestion: 0,
    };
  },
  methods: {
    showResult(result) {
      this.result = result;
      this.questionMode = false;
    },
    nextQuestion() {
      let random = Math.random() * this.questions.length;
      this.currentQuestion = parseInt(random);

      this.questionMode = true;
    },
  },
};
</script>

<style>
body {
  background: linear-gradient(to right, rgb(0, 0, 70), rgb(28, 181, 224));
  font-family: "Oswald", sans-serif;
  color: #fff;
  height: 100vh;
}

#app {
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  display: flex;
  flex-direction: column;
  align-items: center;
}

#app h1 {
  font-weight: 200;
  font-size: 4rem;
}

@keyframes flip-out {
  from {
    transform: rotateY(0deg);
  }
  to {
    transform: rotateY(90deg);
  }
}

@keyframes flip-in {
  from {
    transform: rotateY(90deg);
  }
  to {
    transform: rotateY(0deg);
  }
}

.flip-enter-active {
  animation: flip-in 0.3s ease;
}

.flip-leave-active {
  animation: flip-out 0.3s ease;
}

@media screen and (max-width: 600px) {
  .question {
    width: 100%;
		margin-bottom: 30px;
  }

  .question span {
    font-size: 2rem;
    margin: 0 auto;
    padding: 0 10px;
  }

  span.text {
    font-size: 1.5rem;
  }

  ul.answers {
    display: block;
    font-size: 1.5rem;
  }

  .answers span.number {
    padding: 0 10px;
    position: absolute;
    left: 0;
    bottom: 0;
    top: 0;
    font-size: 1.4rem;
  }

  .answers li {
    position: relative;
    display: inline-block !important;
    width: 90%;
    margin: 6px auto !important;
  }

	.result {
		height: 250px;
	}

	.result span {
		font-size: 2rem;
	}

	.result button {
		font-size: 1rem;
	}
}
</style>
