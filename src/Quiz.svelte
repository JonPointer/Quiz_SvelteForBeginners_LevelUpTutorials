<script>
	import Question from "./Question.svelte";

	let quiz = getQuiz();

	async function getQuiz() {
		const res = await fetch(
			"https://opentdb.com/api.php?amount=10&category=18&difficulty=easy&type=multiple"
		);
		const quiz = await res.json();
		// console.log("quiz ", quiz);
		// debugger;
		return quiz;
	}

	function handleClick() {
		quiz = getQuiz();
	}
</script>

<div>
	<button on:click={() => handleClick()}>Get Questions</button>

	{#await quiz}
		loading...
	{:then data}
		{#each data.results as question}
			<Question {question} />
		{/each}
	{/await}
</div>
