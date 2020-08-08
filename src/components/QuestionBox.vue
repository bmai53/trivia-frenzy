<template>
	<div class="question-box-container">
		<b-jumbotron>
			<template v-slot:lead>
				<p v-html="currentQuestion.question" />
			</template>

			<hr class="my-4" />

			<b-list-group>
				<b-list-group-item
					v-for="(a, i) in shuffledAnswers"
					:key="i"
					@click="selectAnswer(i)"
					:class="answerClass(i)"
					v-html="a"
				/>
			</b-list-group>

			<b-button
				class="submit-button"
				variant="primary"
				@click="submitAnswer"
				:disabled="selectedIndex === null || answered"
			>Submit</b-button>
			<b-button class="next-button" variant="success" @click="next">Next</b-button>
		</b-jumbotron>
	</div>
</template>

<script>
import _ from "lodash";

export default {
	props: {
		currentQuestion: Object,
		next: Function,
		increment: Function,
	},
	data() {
		return {
			selectedIndex: null,
			shuffledAnswers: [],
			answered: false,
			correctIndex: null,
		};
	},
	computed: {
		answers() {
			let answers = [...this.currentQuestion.incorrect_answers];
			answers.push(this.currentQuestion.correct_answer);
			return answers;
		},
	},
	watch: {
		currentQuestion: {
			immediate: true,
			handler() {
				this.selectedIndex = null;
				this.shuffleAnswers();
				this.answered = false;
			},
		},
	},
	methods: {
		selectAnswer(i) {
			this.selectedIndex = i;
		},
		submitAnswer() {
			let isCorrect = false;
			if (this.selectedIndex === this.correctIndex) {
				isCorrect = true;
			}
			this.increment(isCorrect);
			this.answered = true;
		},
		shuffleAnswers() {
			let answers = [
				...this.currentQuestion.incorrect_answers,
				this.currentQuestion.correct_answer,
			];
            this.shuffledAnswers = _.shuffle(answers);
			this.correctIndex = this.shuffledAnswers.indexOf(this.currentQuestion.correct_answer);
		},
		answerClass(i) {
			let answerClass = "";
			if (!this.answered && this.selectedIndex === i) {
				answerClass = "selected";
			} else if (this.answered && this.correctIndex === i) {
				answerClass = "correct";
			} else if (
				this.answered &&
				this.correctIndex !== i &&
				this.selectedIndex === i
			) {
				answerClass = "incorrect";
			}

			return answerClass;
		},
	},
};
</script>

<style scoped>
.list-group {
	margin-bottom: 15px;
}
.list-group-item:hover {
	background-color: #eee;
	cursor: pointer;
}
.selected {
	background-color: lightblue;
}
.selected:hover {
	background-color: lightblue;
}
.correct {
	background-color: lightgreen;
}
.correct:hover {
	background-color: lightgreen;
}
.incorrect {
	background-color: lightcoral;
}
.incorrect:hover {
	background-color: lightcoral;
}
.submit-button {
	display: block;
	margin: 10px auto 0px auto;
	width: 50%;
}

.next-button {
	display: block;
	margin: 10px auto 0px auto;
	width: 50%;
}
</style>