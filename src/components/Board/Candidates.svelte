<script>
	import { CANDIDATE_COORDS } from '@sudoku/constants';

	export let candidate = [];
	export let value = [];

	// let data = candidate || value;

	export let inferenceKey = [];
	export let visitedNum = [];

</script>

<div class="candidate-grid">
	{#each CANDIDATE_COORDS as [row, col], index}
		<div class="candidate row-start-{row} col-start-{col}"
		     class:invisible={!candidate.includes(index + 1) && !value.includes(index + 1)}
		     class:visible={candidate.includes(index + 1) || value.includes(index + 1)}
			 class:bg-gray-400={value.includes(index + 1) && visitedNum.includes(index + 1)}
			 class:bg-yellow-600={candidate.includes(index + 1) && inferenceKey.includes(index + 1)}>
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