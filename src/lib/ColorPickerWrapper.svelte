<script lang="ts">
	export let wrapper: HTMLElement;
	export let isOpen: boolean;
	export let isPopup: boolean;
	export let toRight: boolean;
</script>

<div
	class="my-2 p-2 gap-2 grid-cols-[auto_auto] rounded-xl bg-white border shadow w-max hidden top-12 z-20"
	bind:this={wrapper}
	class:!grid={isOpen}
	class:!absolute={isPopup}
	role={isPopup ? "dialog" : undefined}
	aria-label="color picker"
>
	<slot />
</div>

<style lang="postcss">
	div :global(.picker-wrapper) {
		@apply block mr-0;
	}
	div :global(.slider-wrapper) {
		@apply block mr-0;
	}
	div :global(.text-input) {
		@apply col-span-2 m-0 gap-2;
	}
	div :global(.input-container) {
		@apply gap-2;
	}
	div :global(.input-container input) {
		@apply h-auto leading-none p-2 border border-solid border-inherit bg-white focus:ring-4 focus-visible:outline-none;
	}
	div :global(.text-input button) {
		@apply h-auto leading-none p-2 border border-solid border-inherit;
	}
</style>
