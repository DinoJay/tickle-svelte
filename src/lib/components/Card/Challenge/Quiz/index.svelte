<script>
	import Quiz from './Quiz.svelte';
	export let onSubmit;
	let responses = {};
</script>

<Quiz
	{...$$props}
	{responses}
	onAnswerSubmit={(resp) => {
		responses = { ...responses, [resp.qid]: resp.answer };
		onSubmit(responses);
	}}
/>
