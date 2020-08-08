<template>
	<div class="score-component">
		<b-card title="Score">
            <b-card-text>
                {{correctAnswers}}/{{total}}
            </b-card-text>
		</b-card>
	</div>
</template>

<script>
export default {
	props: ["correctAnswers", "total"],
};
</script>

<style scoped>
    .score-component{
        width: 250px;
        margin: 25px auto 25px auto;
    }
</style>