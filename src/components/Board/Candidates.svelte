<script>
	import { CANDIDATE_COORDS } from '@sudoku/constants';

	export let candidate;
	export let value;

	let data = candidate || value;
</script>

<div class="candidate-grid">
	{#each CANDIDATE_COORDS as [row, col], index}
		<div class="candidate row-start-{row} col-start-{col}"
		     class:invisible={!data.includes(index + 1)}
		     class:visible={data.includes(index + 1)}>
			{index + 1}
		</div>
	{/each}
</div>

<style>
	.candidate-grid {
		@apply grid h-full w-full p-1;
	}

	.candidate {
		@apply h-full w-full row-end-auto col-end-auto leading-full;
	}
</style>