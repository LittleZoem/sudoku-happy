<script>
	import { candidates, inferenceKeys } from '@sudoku/stores/candidates';
	import { userGrid } from '@sudoku/stores/grid';
	import { cursor } from '@sudoku/stores/cursor';
	import { hints } from '@sudoku/stores/hints';
	import { gamePaused } from '@sudoku/stores/game';
	import { roam } from '@sudoku/stores/roam';
	import { hintHighLight } from '@sudoku/strategy/hint_high_light';
    import { inferenceGrid } from '@sudoku/stores/inference';


	function clearDisplay() {
		cursor.reset();
		hintHighLight.clear();
		candidates.reset();
		inferenceKeys.reset();
	}

	function undo() {
		userGrid.loadState((roam.undo()).grid);
		inferenceGrid.execute();
		clearDisplay();
	}

	function redo() {
		userGrid.loadState((roam.redo()).grid);
		inferenceGrid.execute();
		clearDisplay();
	}

	function back() {
		userGrid.loadState((roam.back()).grid);
		inferenceGrid.execute();
		clearDisplay();
	}

</script>

<div class="action-buttons space-x-3">

	<!-- <button class="btn btn-round" disabled={$gamePaused || $invalidCells.length !== 0} on:click={roam.save} title="Save">
		<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<circle cx="12" cy="12" r="10" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" />
		</svg>
	</button> -->

	<button class="btn btn-round btn-badge" disabled={$gamePaused || $roam.backStack.length === 0} on:click={back} title="Back">
    	<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="m 13 2 a 10 10 0 1 1 -10 10 h 4 l -4 -4 l -4 4 h 4 a 10 10 0 1 0 10 -10 z" />
		</svg>

		{#if $roam.backStack.length > 0}
			<span class="badge" class:badge-primary={true}>{$roam.backStack.length}</span>
		{/if}
	</button>

	<button class="btn btn-round" disabled={$gamePaused || $roam.undoStack.length < 2} on:click={undo} title="Undo">
		<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M3 10h10a8 8 0 018 8v2M3 10l6 6m-6-6l6-6" />
		</svg>
	</button>

	<button class="btn btn-round" disabled={$gamePaused || $roam.redoStack.length === 0} on:click={redo} title="Redo">
		<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M21 10h-10a8 8 90 00-8 8v2M21 10l-6 6m6-6l-6-6" />
		</svg>
	</button>

	<button class="btn btn-round btn-badge" disabled = {false} on:click={hints.toggle} title="Hints ({$hints})">
		<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M9.663 17h4.673M12 3v1m6.364 1.636l-.707.707M21 12h-1M4 12H3m3.343-5.657l-.707-.707m2.828 9.9a5 5 0 117.072 0l-.548.547A3.374 3.374 0 0014 18.469V19a2 2 0 11-4 0v-.531c0-.895-.356-1.754-.988-2.386l-.548-.547z" />
		</svg>

		{#if $hints > 0}
			<span class="badge" class:badge-primary={true}>{$hints}</span>
		{/if}
	</button>

	<!-- <button class="btn btn-round btn-badge" on:click={notes.toggle} title="Notes ({$notes ? 'ON' : 'OFF'})">
		<svg class="icon-outline" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24" stroke="currentColor">
			<path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15.232 5.232l3.536 3.536m-2.036-5.036a2.5 2.5 0 113.536 3.536L6.5 21.036H3v-3.572L16.732 3.732z" />
		</svg>

		<span class="badge tracking-tighter" class:badge-primary={$notes}>{$notes ? 'ON' : 'OFF'}</span>
	</button> -->

</div>


<style>
	.action-buttons {
		@apply flex flex-wrap justify-evenly self-end;
	}

	.btn-badge {
		@apply relative;
	}

	.badge {
		min-height: 20px;
		min-width:  20px;
		@apply p-1 rounded-full leading-none text-center text-xs text-white bg-gray-600 inline-block absolute top-0 left-0;
	}

	.badge-primary {
		@apply bg-primary;
	}
</style>