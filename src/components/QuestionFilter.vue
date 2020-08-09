<template>
	<div class="question-filter-component">
		<b-button v-b-toggle.collapse-options variant="primary" class='mx-auto d-block'>Toggle Options</b-button>

		<b-collapse id="collapse-options" class="mt-3">
			<b-card title="Options">
				<label for="trivia_category">Select Category:</label>
				<select
					name="trivia_category"
					class="form-control"
					:value="categoryID"
					@change="updateCategoryID"
				>
					<option value="any">Any Category</option>
					<option value="9">General Knowledge</option>
					<option value="10">Entertainment: Books</option>
					<option value="11">Entertainment: Film</option>
					<option value="12">Entertainment: Music</option>
					<option value="13">Entertainment: Musicals &amp; Theatres</option>
					<option value="14">Entertainment: Television</option>
					<option value="15">Entertainment: Video Games</option>
					<option value="16">Entertainment: Board Games</option>
					<option value="17">Science &amp; Nature</option>
					<option value="18">Science: Computers</option>
					<option value="19">Science: Mathematics</option>
					<option value="20">Mythology</option>
					<option value="21">Sports</option>
					<option value="22">Geography</option>
					<option value="23">History</option>
					<option value="24">Politics</option>
					<option value="25">Art</option>
					<option value="26">Celebrities</option>
					<option value="27">Animals</option>
					<option value="28">Vehicles</option>
					<option value="29">Entertainment: Comics</option>
					<option value="30">Science: Gadgets</option>
					<option value="31">Entertainment: Japanese Anime &amp; Manga</option>
					<option value="32">Entertainment: Cartoon &amp; Animations</option>
				</select>

				<label for="trivia_difficulty">Select Difficulty:</label>
				<select
					name="trivia_difficulty"
					class="form-control"
					:value="difficulty"
					@change="updateDifficulty"
				>
					<option value="any">Any Difficulty</option>
					<option value="easy">Easy</option>
					<option value="medium">Medium</option>
					<option value="hard">Hard</option>
				</select>

				<b-button class="load-questions" variant="primary" @click="getQuestions">Load Questions</b-button>
			</b-card>
		</b-collapse>
	</div>
</template>

<script>
export default {
	props: [
		"getQuestions",
		"categoryID",
		"updateCategoryID",
		"difficulty",
		"updateDifficulty",
	],
};
</script>
<style scoped>
.question-filter-component {
	text-align: left;
}
.load-questions {
	width: 100%;
	margin: 10px auto 0px auto;
}
select {
	margin-bottom: 15px;
}
</style>