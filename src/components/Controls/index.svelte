<script>
	import ActionBar from './ActionBar/index.svelte';
	import Keyboard from './Keyboard.svelte';
    import { strategyTrack } from '@sudoku/stores/hints';
</script>

<div class="px-4 pb-5 flex justify-center">
	<div class="w-full max-w-xl">
		<div class="hintMsg">
			{ $strategyTrack }
		</div>
		<ActionBar />

		<Keyboard />
	</div>
</div>

<style>
	.hintMsg {
		text-align: right;
		margin: 10px 0px 10px 0px;
	}
  </style>