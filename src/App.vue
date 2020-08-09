
<template>
	<div id="app">
		<GithubCorner 
			class="fixed-top" 
			url='https://github.com/bmai53/trivia-frenzy' 
			cornerColor='#007bff'
			gitColor='FFF'
			:size='100'
			leftCorner
		/>
		<Score :correctAnswers="correctAnswers" :total="total" />
		<b-container>
			<b-row>
        <b-col sm="2"/>
				<b-col sm="8">
					<b-spinner v-if="!questions.length" class="spinner" label="Large Spinner" />
					<QuestionBox
						v-if="questions.length"
						:currentQuestion="questions[index]"
						:next="next"
						:increment="increment"
					/>
				</b-col>
        <b-col sm="2"/>
			</b-row>
			<b-row>
        <b-col sm="3"/>
				<b-col sm="6">
        <b-col sm="3"/>
					<QuestionFilter
						:getQuestions="getQuestions"
						:categoryID="categoryID"
						:difficulty="difficulty"
            :updateCategoryID="updateCategoryID"
            :updateDifficulty="updateDifficulty"
					/>
				</b-col>
			</b-row>
		</b-container>
	</div>
</template>

<script>
import GithubCorner from 'vue-github-corners'
import QuestionBox from "./components/QuestionBox.vue";
import Score from "./components/Score.vue";
import QuestionFilter from "./components/QuestionFilter";

import axios from "axios";

export default {
	name: "App",
	components: {
		GithubCorner,
		QuestionBox,
		Score,
		QuestionFilter,
	},
	data() {
		return {
			questions: [],
			index: 0,
			correctAnswers: 0,
			total: 0,
			categoryID: 'any',
			difficulty: 'any',
		};
	},
	methods: {
		next() {
			this.index++;
		},
		increment(isCorrect) {
			if (isCorrect) {
				this.correctAnswers++;
			}
			this.total++;
		},
		getApiUrl() {
			let api = "https://opentdb.com/api.php?amount=10";
			if (this.categoryID !== null && this.categoryID !== "any") {
				api += `&category=${this.categoryID}`;
			}
			if (this.difficulty !== null && this.difficulty !== "any") {
				api += `&difficulty=${this.difficulty}`;
			}
			return api;
		},
		getQuestions() {
			const api = this.getApiUrl();
			axios.get(api).then((res) => {
        this.index = 0;
				this.questions = res.data.results;
			});
    },
    updateCategoryID(e){
      this.categoryID = e.target.value
    },
    updateDifficulty(e){
      this.difficulty = e.target.value
    },

	},
	watch: {
		index() {
			if (this.index % 9 === 0) {
				const api = this.getApiUrl();
				axios.get(api).then((res) => {
					this.questions = this.questions.concat(res.data.results);
				});
			}
		},
	},
	mounted: function () {
		this.getQuestions();
	},
};
</script>

<style>
#app {
	font-family: Avenir, Helvetica, Arial, sans-serif;
	-webkit-font-smoothing: antialiased;
	-moz-osx-font-smoothing: grayscale;
	text-align: center;
	color: #2c3e50;
}

.spinner {
	width: 10vmin;
	height: 10vmin;
	margin: 0 auto 0 auto;
}
</style>
