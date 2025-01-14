<script>
	import Candidates from './Candidates.svelte';
	import { fade } from 'svelte/transition';
	import { SUDOKU_SIZE } from '@sudoku/constants';
	import { cursor } from '@sudoku/stores/cursor';
	import { hintHighLight } from '@sudoku/strategy/hint_high_light';
	import { candidates, inferenceKeys } from '@sudoku/stores/candidates';
    import { inferenceGrid } from '@sudoku/stores/inference';
	import { get } from 'svelte/store';
	import { hints } from '@sudoku/stores/hints'
    import { userGrid } from '@sudoku/stores/grid';

	export let value;
	export let cellX;
	export let cellY;
	export let candidate;
	export let inferenceKey;

	export let disabled;
	export let conflictingNumber;
	export let userNumber;
	export let gridNumber;
	export let inferenced;
	export let selected;
	export let sameArea;
	export let sameNumber;

	const borderRight = (cellX !== SUDOKU_SIZE && cellX % 3 !== 0);
	const borderRightBold = (cellX !== SUDOKU_SIZE && cellX % 3 === 0);
	const borderBottom = (cellY !== SUDOKU_SIZE && cellY % 3 !== 0);
	const borderBottomBold = (cellY !== SUDOKU_SIZE && cellY % 3 === 0);

	function isInferencedAnswer(x, y) {
		return get(hints) > 0 && get(userGrid)[y][x] === 0 && get(inferenceGrid)[y][x].length === 1;
	}

	function clickCell (x, y, inferenced) {
		cursor.set(x, y);
		hintHighLight.clear();
		candidates.reset();
		inferenceKeys.reset();

		if (inferenced) {
			inferenceGrid.showInferece(x, y);
		}
	}

	function doubleClick(x, y) {
		cursor.set(x, y);
		if (isInferencedAnswer(x, y)) {
			inferenceGrid.set(get(cursor), get(inferenceGrid)[y][x][0]);
			cursor.reset();
			hintHighLight.clear();
			candidates.reset();
			inferenceKeys.reset();
		}
	}
</script>

<div class="cell row-start-{cellY} col-start-{cellX}"
     class:border-r={borderRight}
     class:border-r-4={borderRightBold}
     class:border-b={borderBottom}
     class:border-b-4={borderBottomBold}>

	{#if !disabled}
		<div class="cell-inner"
		     class:user-number={userNumber}
			 class:inferenced={inferenced}
		     class:selected={selected}
		     class:same-area={sameArea}
		     class:same-number={sameNumber}
		     class:conflicting-number={conflictingNumber}>

			<button class="cell-btn" on:click={clickCell(cellX - 1, cellY - 1, inferenced)} on:dblclick={doubleClick(cellX - 1, cellY - 1)}>
				{#if candidate}
					<Candidates {candidate} inferenceKey={inferenceKey}/>
				{:else if inferenced && value.length > 1}
					<Candidates {value} />
				{:else if value.length == 1 && (inferenced || userNumber || gridNumber)}
					<span class="cell-text">{value[0] || ''}</span>
				{:else}
					<span class="cell-text">{''}</span>
				{/if}
			</button>

		</div>
	{/if}

</div>

<style>
	.cell {
		@apply h-full w-full row-end-auto col-end-auto;
	}

	.cell-inner {
		@apply relative h-full w-full text-gray-800;
	}

	.cell-btn {
		@apply absolute inset-0 h-full w-full;
	}

	.cell-btn:focus {
		@apply outline-none;
	}

	.cell-text {
		@apply leading-full text-base;
	}

	@media (min-width: 300px) {
		.cell-text {
			@apply text-lg;
		}
	}

	@media (min-width: 350px) {
		.cell-text {
			@apply text-xl;
		}
	}

	@media (min-width: 400px) {
		.cell-text {
			@apply text-2xl;
		}
	}

	@media (min-width: 500px) {
		.cell-text {
			@apply text-3xl;
		}
	}

	@media (min-width: 600px) {
		.cell-text {
			@apply text-4xl;
		}
	}

	.user-number {
		@apply text-primary;
	}

	.same-area {
		@apply bg-primary-lighter;
	}

	.same-number {
		@apply bg-primary-light;
	}

	.inferenced {
		@apply bg-green-500 text-white;
	}

	.selected {
		@apply bg-primary text-white;
	}

	.conflicting-number {
		@apply text-red-600;
	}
</style>