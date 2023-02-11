<script>
	import { fade, blur, fly, slide, scale } from "svelte/transition";
	import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
	import Question from "./Question.svelte";
	import Modal from "./Modal.svelte";
	import { score } from "./store.js";

	let activeQuestion = 0;
	let quiz = getQuiz();
	let isModalOpen = false;

	onMount(() => {
		console.log("Mounted");
	});
	beforeUpdate(() => {
		console.log("before update");
	});
	afterUpdate(() => {
		console.log("after update");
	});

	async function getQuiz() {
		const res = await fetch(
			"https://opentdb.com/api.php?amount=10&category=18&difficulty=easy&type=multiple"
		);
		const quiz = await res.json();
		// console.log("quiz ", quiz);
		// debugger;
		return quiz;
	}

	function nextQuestion() {
		activeQuestion = activeQuestion + 1;
	}

	function resetQuiz() {
		isModalOpen = false;
		score.set(0);
		activeQuestion = 0;
		quiz = getQuiz();
	}

	// Reactive statement
	$: if (questionNumber > 10) {
		isModalOpen = true;
	}
	// $: if ($score > 7) {
	// 	isModalOpen = true;
	// }

	// Reactive declaration
	$: questionNumber = activeQuestion + 1;
</script>

<div>
	<button on:click={resetQuiz}>Bail and Start New Quiz</button>

	<h3>My Score: {$score}</h3>
	{#if questionNumber < 11}
		<h4>Question #{questionNumber}</h4>
	{/if}

	<div class="container">
		{#await quiz}
			loading...
		{:then data}
			{#each data.results as question, index}
				{#if index === activeQuestion}
					<div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
						<Question {nextQuestion} {question} />
					</div>
				{/if}
			{/each}
		{/await}
	</div>
</div>

{#if isModalOpen}
	<Modal on:close={resetQuiz}>
		<h2>Score: {$score} / 10</h2>
		<p>Congratulations!</p>
		<!-- <button on:click={resetQuiz}>Start Over</button> -->
	</Modal>
{/if}

<style>
	.fade-wrapper {
		position: absolute;
	}
	.container {
		min-height: 400px;
	}
</style>
