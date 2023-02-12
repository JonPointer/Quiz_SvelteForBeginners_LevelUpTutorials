<script>
	import Quiz from "./Quiz.svelte";
</script>

<svelte:head>
	<title>Quiz</title>
</svelte:head>

<h1>Level Up Computer Quiz</h1>
<div>
	<Quiz />
</div>

<style>
	:global(body) {
		background: #524763;
	}
	h1 {
		color: white;
		text-align: center;
	}
	div {
		background: white;
		border-radius: 15px;
		padding: 40px;
	}
	:global(button) {
		border-radius: 5px;
	}
</style>
