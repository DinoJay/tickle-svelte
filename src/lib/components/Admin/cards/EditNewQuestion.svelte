<script>
	import EditQuestion from './EditQuestion.svelte';

	let sq = { id: null, answers: [], img: { url: '', name: '' }, text: '' };

	export let onClose;
	export let onCreate;
	export let open;
</script>

<EditQuestion {open} q={sq} onCreate={() => onCreate(sq)} onChange={(q) => (sq = q)} {onClose} />
