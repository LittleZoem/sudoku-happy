<script>
	import ActionBar from './ActionBar/index.svelte';
	import Keyboard from './Keyboard.svelte';
	import { hintMsg } from '@sudoku/stores/hints';
</script>

<div class="px-4 pb-5 flex justify-center">
	<div class="w-full max-w-xl">

		{#if $hintMsg.enabled}
			<div class="hintMsg">
				{ $hintMsg.content }
			</div>
		{/if}

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