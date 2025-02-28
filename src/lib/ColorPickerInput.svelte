<script lang="ts">
	export let labelWrapper: HTMLLabelElement;
	export let hex: string;
	export let label: string;
	export let isOpen: boolean;
</script>

<!-- svelte-ignore a11y-click-events-have-key-events -->
<!-- svelte-ignore a11y-no-noninteractive-element-interactions -->
<label
	class="grid [&>*]:row-start-1 [&>*]:col-start-1 cursor-pointer"
	bind:this={labelWrapper}
	on:click|preventDefault
	on:mousedown|preventDefault
>
	<div class="flex items-center justify-center">
		<input
			class="w-4 h-4"
			type="color"
			value={hex}
			on:click|preventDefault
			on:mousedown|preventDefault
			aria-haspopup="dialog"
		/>
	</div>
	<div class="alpha w-12 h-12 rounded-full select-none" />
	<div
		class="w-12 h-12 rounded-full select-none z-10"
		style:background-color={hex}
	/>
</label>

<style>
	.alpha {
		clip-path: circle(50%);
		background-image:
			linear-gradient(
				to right,
				rgba(238, 238, 238, 0.75),
				rgba(238, 238, 238, 0.75)
			),
			linear-gradient(to right, black 50%, white 50%),
			linear-gradient(to bottom, black 50%, white 50%);
		background-blend-mode: normal, difference, normal;
		background-size: 0.75rem 0.75rem;
	}
</style>
