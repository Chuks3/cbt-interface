<template>
  <main class="app">
    <h1>The Quiz</h1>
  </main>

  <section class="quiz" v-if="!quizCompleted">
    <div class="quiz-info">
      <span class="question">{{ getCurrentQuestion().question }}</span>
      <span class="score">Score: {{ score() }}/{{ questions.length  * 10 }}</span>
    </div>

    <div class="options">
      <label v-for="(option, indexOf) in getCurrentQuestion().options" 
      :key="indexOf"
      :class="`option ${
        getCurrentQuestion().selected == indexOf
          ? indexOf == getCurrentQuestion().answer
            ? 'correct'
            : 'wrong'
          : ''
       } ${
        getCurrentQuestion().selected != null &&
        indexOf != getCurrentQuestion().selected
          ? 'disabled'
          : ''
      }`">
        <input 
          type="radio" 
          :name="getCurrentQuestion().indexOf"
          :value="indexOf"
          v-model="getCurrentQuestion().selected"
          :disabled="getCurrentQuestion().selected"
          @change="setAnswer($event)">

        <span>{{ option }}</span>
      </label>
    </div>

    <button
      @click="nextQuestion()"
      :disabled="!getCurrentQuestion().selected">
      {{ getCurrentQuestion().indexOf == questions.length - 1 
        ? 'Finish'
        : getCurrentQuestion().selected == null
          ? 'Select an option'
          : 'Next Question'}}
    </button>
  </section>

  <section v-else>
    <h2>You have finished the quiz!</h2>
    <p>{{ grade(score()) }}, your score is {{ score() }}/{{ questions.length * 10 }}</p>
  </section>
</template>

<script>
export default {
  name: 'App',
  components: {  },

  data () { 
    return { 
      questions: [
        {
          question: 'What is Vue JS?',
          answer: 0,
          options: [
            'A front end framework',
            'A library',
            'An ice cream maker'
          ],
          selected: null
        },
        {
          question: 'Which one of this is a computer?',
          answer: 3,
          options: [
            'A personal pomputer',
            'A phone',
            'A laptop',
            'All of the above'
          ],
          selected: null
        },
        {
          question: 'What does HTML stand for?',
          answer: 2,
          options: [
            'HyperText Markdown Language',
            'HyperTop Markup Language',
            'HyperText Markup Language',
            'HyperLink Mardown Language'
          ],
          selected: null
        },
        {
          question: 'Which one of these is not a programming language?',
          answer: 1,
          options: [
            'JAVA',
            'Phyton',
            'C language',
            'None of the above'
          ],
          selected: null
        },
        {
          question: 'What is Vuex?',
          answer: 1,
          options: [
            'Vue with an x',
            'State management library',
            'A cheese selection',
            'An artificial intelligence'
          ]
        }
      ], 

      quizCompleted: false,
      currentQuestion: 0,
    }
  },

  methods: {
    score() {
      let value = 0
      this.questions.map(q => {
        if (q.selected == q.answer) {
          value += 10
        }
      })
      return value
    },

    getCurrentQuestion() {
      let question = this.questions[this.currentQuestion]
      question.indexOf = this.currentQuestion
      return question
    },

    setAnswer($event) {
      this.questions[this.currentQuestion].selected = $event.target.value
      $event.target.value = null
    },

    nextQuestion() {
      if (this.currentQuestion < this.questions.length - 1) {
        this.currentQuestion++
      } else {
        this.quizCompleted = true
      }
    },

    grade(score) {
      if (score < 25) {
        return 'Sorry'
      } else {
        return 'Congratulations'
      }
    }
  }
}
</script>

<style>
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Montserrat', sans-serif;
}

body{
  background-color: #271c36;
  color: #fff;
}

#app {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 2rem;
  min-height: 100vh;
}

h1 {
  font-size: 2rem;
  margin-bottom: 2rem;

}

.quiz {
  background-color: #382a4b;
  padding: 3rem;
  width: 100%;
  max-width: 640px;
  border-radius: 0.5rem;
}

.quiz-info {
  display: flex;
  justify-content: space-between;
  margin-bottom: 1rem;
}

.quiz-info .question {
  color: #8f8f8f;
  font-size: 1.25rem;
}

.quiz-info .score {
  color: #fff;
  font-style: 1.25rem;
}

.options {
  margin-bottom: 1rem;
}

.option {
  padding: 1rem;
  display: block;
  background-color: #271c36;
  margin-bottom: 0.5rem;
  border-radius: 0.5rem;
  cursor: pointer;
}

.option:hover {
  background-color: #2d213f;
}

.option.correct {
  background-color: green;
}

.option.wrong {
  background-color: red;
}

.option:last-of-type {
  margin-bottom: 0;
}
.option.disabled {
  opacity: 0.5;
}
.option input {
  display: none;
}
button {
  appearance: none; 
  outline: none;
  border: none;
  cursor: pointer;
  padding: 0.5rem 1rem;
  background-color: green;
  color: #2d213f;
  font-weight: 700;
  text-transform: uppercase;
  font-size: 1.25rem;
  border-radius: 0.5rem;
}
button:disabled {
  opacity: 0.5;
}
h2 {
  font-size: 2rem;
  margin-bottom: 2rem;
  text-align: center;
}
p {
  color: #afafaf;
  font-size: 1.25rem;
  text-align: center;
}
</style>