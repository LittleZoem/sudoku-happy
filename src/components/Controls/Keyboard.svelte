<script>
	import { cursor } from '@sudoku/stores/cursor';
	import { candidates, inferenceKeys, visitedNums } from '@sudoku/stores/candidates';
	import { hints } from '@sudoku/stores/hints'

	// TODO: Improve keyboardDisabled
	import { keyboardDisabled } from '@sudoku/stores/keyboard';
    import { inferenceGrid } from '@sudoku/stores/inference';
	import { hintHighLight } from '@sudoku/strategy/hint_high_light';

	function handleKeyButton(num) {
		if (!$keyboardDisabled) {
			inferenceGrid.set($cursor, num);
			cursor.reset();
			hintHighLight.clear();
			candidates.reset();
			inferenceKeys.reset();
		}
	}

	function inputNumDisable(hintsStore, inferenceGridStore, visitedNumsStore, cursorStore, num) {
		return hintsStore > 0
			   && inferenceGridStore[cursorStore.y][cursorStore.x].length <= hintsStore
			   && (!inferenceGridStore[cursorStore.y][cursorStore.x].includes(num)
			   		|| (visitedNumsStore[cursorStore.x + ',' + cursorStore.y] && visitedNumsStore[cursorStore.x + ',' + cursorStore.y].includes(num)));
	}

	function handleKey(e) {
		switch (e.key || e.keyCode) {
			case 'ArrowUp':
			case 38:
			case 'w':
			case 87:
				cursor.move(0, -1);
				break;

			case 'ArrowDown':
			case 40:
			case 's':
			case 83:
				cursor.move(0, 1);
				break;

			case 'ArrowLeft':
			case 37:
			case 'a':
			case 65:
				cursor.move(-1);
				break;

			case 'ArrowRight':
			case 39:
			case 'd':
			case 68:
				cursor.move(1);
				break;

			case 'Backspace':
			case 8:
			case 'Delete':
			case 46:
				handleKeyButton(0);
				break;

			default:
				if (e.key && e.key * 1 >= 0 && e.key * 1 < 10) {
					handleKeyButton(e.key * 1);
				} else if (e.keyCode - 48 >= 0 && e.keyCode - 48 < 10) {
					handleKeyButton(e.keyCode - 48);
				}
				break;
		}
	}
</script>

<svelte:window on:keydown={handleKey} /><!--on:beforeunload|preventDefault={e => e.returnValue = ''} />-->

<div class="keyboard-grid">

	{#each Array(10) as _, keyNum}
		{#if keyNum === 9}
			<button class="btn btn-key" disabled={$keyboardDisabled} title="Erase Field" on:click={() => handleKeyButton(0)}>
				<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
					<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M19 7l-.867 12.142A2 2 0 0116.138 21H7.862a2 2 0 01-1.995-1.858L5 7m5 4v6m4-6v6m1-10V4a1 1 0 00-1-1h-4a1 1 0 00-1 1v3M4 7h16" />
				</svg>
			</button>
		{:else}
			<button class="btn btn-key" disabled={$keyboardDisabled || inputNumDisable($hints, $inferenceGrid, $visitedNums, $cursor, keyNum + 1)} title="Insert {keyNum + 1}" on:click={() => handleKeyButton(keyNum + 1)}>
				{keyNum + 1}
			</button>
		{/if}
	{/each}

</div>

<style>
	.keyboard-grid {
		@apply grid grid-rows-2 grid-cols-5 gap-3;
	}


	.btn-key {
		@apply py-4 px-0;
	}
</style>