<script>
	import ArrowRightDropCircleOutline from 'svelte-material-icons/ArrowRightDropCircleOutline.svelte';

	export let title = '';
	export let selectedEnvironment = true;

	let expanded = false;
</script>

<!-- Main block -->
<div
	class="panel-container h-[3rem] 
		border-2 border-c-black custom-shadow
		overflow-hidden 
		{expanded ? 'expanded' : ''}"
>
	<!-- Title container -->
	<div
		on:keydown={() => null}
		class="flex h-[3rem] w-full justify-center "
		on:click={() => {
			if (selectedEnvironment) expanded = !expanded;
		}}
	>
		<!-- Icon -->
		<span
			class="my-auto mr-2 transition-all
			{expanded ? 'rotate' : ''}"
		>
			<ArrowRightDropCircleOutline size={24} />
		</span>
		<!-- Title content -->
		<h1 class="my-auto text-xl font-medium">
			{title}
		</h1>
	</div>

	<!-- Body's block -->
	<div class="h-[37rem] w-full p-2 flex flex-col">
		<slot />
	</div>
</div>

<style>
	.panel-container {
		transition: all 0.3s ease;
	}

	.expanded {
		height: 40rem;
	}

	.rotate {
		transform: rotate(90deg);
	}

	.custom-shadow {
		box-shadow: 5px 5px 5px #a6a29f;
	}
</style>
