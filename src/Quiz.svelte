<script>
	import { fade, blur, fly, slide, scale } from "svelte/transition";
	import { onMount, beforeUpdate, afterUpdate, onDestroy } from "svelte";
	import Question from "./Question.svelte";
	import Modal from "./Modal.svelte";

	let activeQuestion = 0;
	let score = 0;
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
		score = 0;
		activeQuestion = 0;
		quiz = getQuiz();
	}

	function addToScore() {
		score = score + 1;
	}
	// Reactive statement
	$: if (score > 0) {
		isModalOpen = true;
	}

	// Reactive declaration
	$: questionNumber = activeQuestion + 1;
</script>

<div>
	<button on:click|once={resetQuiz}>Start New Quiz</button>

	<h3>My Score: {score}</h3>
	<h4>Question #{questionNumber}</h4>

	{#await quiz}
		loading...
	{:then data}
		{#each data.results as question, index}
			{#if index === activeQuestion}
				<div in:fly={{ x: 100 }} out:fly={{ x: -200 }} class="fade-wrapper">
					<Question {addToScore} {nextQuestion} {question} />
				</div>
			{/if}
		{/each}
	{/await}
</div>

{#if isModalOpen}
	<Modal>
		<h2>You won!</h2>
		<p>Congratulations</p>
		<button on:click={resetQuiz}>Start Over</button>
	</Modal>
{/if}

<style>
	.fade-wrapper {
		position: absolute;
	}
</style>
