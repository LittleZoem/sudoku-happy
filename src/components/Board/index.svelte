<script>
	import { BOX_SIZE } from '@sudoku/constants';
	import { gamePaused } from '@sudoku/stores/game';
	import { grid, userGrid, invalidCells } from '@sudoku/stores/grid';
	import { settings } from '@sudoku/stores/settings';
	import { cursor } from '@sudoku/stores/cursor';
	import { candidates, inferenceKeys, visitedNums } from '@sudoku/stores/candidates';
	import Cell from './Cell.svelte';
	import { hintHighLight } from '@sudoku/strategy/hint_high_light';
	import { inferenceGrid } from '@sudoku/stores/inference';
	import { hints } from '@sudoku/stores/hints'

	function isSelected(cursorStore, x, y) {
		return (cursorStore.x === x && cursorStore.y === y);
	}

	function isHighLight(hintHighLight, x ,y) {
		let key = x + ',' + y;

		return hintHighLight.hasOwnProperty(key);
	}

	function isSameArea(cursorStore, x, y) {
		if (cursorStore.x === null && cursorStore.y === null) return false;
		if (cursorStore.x === x || cursorStore.y === y) return true;

		const cursorBoxX = Math.floor(cursorStore.x / BOX_SIZE);
		const cursorBoxY = Math.floor(cursorStore.y / BOX_SIZE);
		const cellBoxX = Math.floor(x / BOX_SIZE);
		const cellBoxY = Math.floor(y / BOX_SIZE);
		return (cursorBoxX === cellBoxX && cursorBoxY === cellBoxY);
	}

	function isInference(hintsStore, userGridStore, x, y, value) {
		if (userGridStore[y][x] != 0) return false;
		if (hintsStore > 0 && value.length <= hintsStore) return true;
		return false;
	}

	function getValueAtCursor(gridStore, cursorStore) {
		if (cursorStore.x === null && cursorStore.y === null) return null;

		return gridStore[cursorStore.y][cursorStore.x];
	}
</script>

<div class="board-padding relative z-10">
	<div class="max-w-xl relative">
		<div class="w-full" style="padding-top: 100%"></div>
	</div>
	<div class="board-padding absolute inset-0 flex justify-center">
		<div class="bg-white shadow-2xl rounded-xl overflow-hidden w-full h-full max-w-xl grid" class:bg-gray-200={$gamePaused}>

			{#each $inferenceGrid as row, y}
				{#each row as value, x}
					<Cell {value}
					      cellY={y + 1}
					      cellX={x + 1}
					      candidate={$candidates[x + ',' + y]}
						  inferenceKey={$inferenceKeys[x + ',' + y]}
						  visitedNum={$visitedNums[x + ',' + y]}
					      disabled={$gamePaused}
					      selected={isSelected($cursor, x, y) || isHighLight($hintHighLight, x, y) }
						  inferenced={isInference($hints, $userGrid, x, y, value)}
						  gridNumber={$grid[y][x] != 0}
					      userNumber={$grid[y][x] === 0 && $userGrid[y][x] != 0}
					      sameArea={$settings.highlightCells && !isSelected($cursor, x, y) && isSameArea($cursor, x, y)}
					      sameNumber={$settings.highlightSame && value && !isSelected($cursor, x, y) && getValueAtCursor($userGrid, $cursor) === value}
					      conflictingNumber={$settings.highlightConflicting && $grid[y][x] === 0 && $invalidCells.includes(x + ',' + y)} />
				{/each}
			{/each}

		</div>

	</div>
</div>

<style>
	.board-padding {
		@apply px-4 pb-4;
	}
</style>