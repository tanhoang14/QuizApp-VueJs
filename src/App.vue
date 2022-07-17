<script setup>
import { ref, computed } from 'vue'

const questions = ref([
  {
	question: 'What can you find in the middle of Chicago?',
	answer: 0,
	options: [
		'Letter “C”',
		'Illinois',
		'Neverland'
	],
	selected: null
  },
  {
	question: 'When you drop a white hat in the Black sea, what will it become?',
	answer: 2,
	options: [
		'Nothing',
		'Dirty',
		'It becomes wet'
	],
	selected: null
  },
  {
	question: 'What can we break, but never hold?',
	answer: 1,
	options: [
		'A Secret',
		'A Promise',
		'A lie'
	],
	selected: null
  },
  {
	question: 'Every time you take a bath, this item becomes smaller. What is it?',
	answer: 0,
	options: [
		'A soap',
		'A hair',
		'A duck'
	],
	selected: null
  },
  {
	question: 'There are two people on a boat. When the boat sinks, why does only one of them get his hair wet?',
	answer: 2,
	options: [
		'Because they sinks badly',
		'Because they surrive',
		'Because the other person is bald'
	],
	selected: null
  },
  
])

const quizCompleted = ref(false)
const currentQuestion = ref(0)
const score = computed(() => {
	let value = 0
	questions.value.map(q => {
		if (q.selected != null && q.answer == q.selected) {
			console.log('correct');
			value++
		}
	})
	return value
})

const getCurrentQuestion = computed(() => {
	let question = questions.value[currentQuestion.value]
	question.index = currentQuestion.value
	return question
})

const SetAnswer = (e) => {
	questions.value[currentQuestion.value].selected = e.target.value
	e.target.value = null
}

const NextQuestion = () => {
	if (currentQuestion.value < questions.value.length - 1) {
		currentQuestion.value++
		return
	}
	
	quizCompleted.value = true
}
</script>

<template>
	<main class="app">
		<h1>Quiz Time</h1>
		
		<section class="quiz" v-if="!quizCompleted">
			<div class="quiz-info">
				<span class="question">{{ getCurrentQuestion.question }}</span>
				<span class="score">Score {{ score }}/{{ questions.length }}</span>
			</div>
			
			<div class="options">
				<label 
					v-for="(option, index) in getCurrentQuestion.options" :key ="index" 
					:for="'option' + index" 
					:class="`option ${
						getCurrentQuestion.selected == index 
							? index == getCurrentQuestion.answer 
								? 'correct' 
								: 'wrong'
							: ''
					} ${
						getCurrentQuestion.selected != null &&
						index != getCurrentQuestion.selected
							? 'disabled'
							: ''
					}`">
					<input 
						type="radio" 
						:id="'option' + index" 
						:name="getCurrentQuestion.index" 
						:value="index" 
						v-model="getCurrentQuestion.selected" 
						:disabled="getCurrentQuestion.selected"
						@change="SetAnswer" 
					/>
					<span>{{ option }}</span>
				</label>
			</div>
			<div class="btnContainer"> 
        <button 
          @click="NextQuestion" 
          :disabled="!getCurrentQuestion.selected">
          {{ 
            getCurrentQuestion.index == questions.length - 1 
              ? 'Finish' 
              : getCurrentQuestion.selected == null
                ? 'Select an option'
                : 'Next question'
          }}
        </button>
      </div>
		</section>

		<section v-else>
			<h2>You have finished the quiz!</h2>
			<p>Your score is {{ score }}/{{ questions.length }}</p>
		</section>
	</main>
</template>

<style>
* {
	margin: 0;
	padding: 0;
	box-sizing: border-box;
  font-family: 'Poppins', sans-serif;
}

body {
	background-color: #15cda8;
	color: #f1e4e4;
}

.app {
	display: flex;
	flex-direction: column;
	align-items: center;
	padding: 2rem;
	height: 100vh;
}

h1 {
	font-size: 2rem;
	margin-bottom: 2rem;
}

.quiz {
	background-color: #099a97;
	padding: 1.5rem;
	width: 100%;
	max-width: 660px;
  border-radius: 5%;
}

.quiz-info {
	display: flex;
	justify-content: space-between;
	margin-bottom: 1rem;
}

.quiz-info .question {
  font-weight: bold;
	color: #f1e4e4;
	font-size: 2.5rem;
}

.quiz-info.score {
	color: #FFF;
	font-size: 1.75rem;
}

.options {
	margin-bottom: 1rem;
}

.option {
	padding: 1rem;
	display: block;
	background-color: #393c83;
	margin-bottom: 0.5rem;
	border-radius: 0.5rem;
	cursor: pointer;
  font-weight: 500;
  
}

.option:hover {
	background-color: #2d213f;
  transition-duration: 1.2s;
}

.option.correct {
	background-color: #2cce7d;
}

.option.wrong {
	background-color: #ff5a5f;
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

.btnContainer{
  display: flex;
  align-items: center;
  justify-content: center;
}
button {
	appearance: none;
	outline: none;
	border: none;
	cursor: pointer;
	padding: 0.5rem 1rem;
	background-color: #ff6d24;
	color: #2d213f;
	font-weight: 700;
	text-transform: uppercase;
	font-size: 1.2rem;
	border-radius: 0.5rem;
}



h2 {
	font-size: 2rem;
	margin-bottom: 2rem;
	text-align: center;
}

p {
	color: #f1e4e4;
	font-size: 3rem;
	text-align: center;
}
</style>