<script lang="ts">
	import defaultColors from "tailwindcss/colors";
	import { Colord, colord, extend, random } from "colord";
	import labPlugin from "colord/plugins/lab";
	import ColorTable from "./ColorTable.svelte";
	import ColorPicker from "svelte-awesome-color-picker";
	import ColorPickerInput from "./ColorPickerInput.svelte";
	import ColorPickerWrapper from "./ColorPickerWrapper.svelte";

	extend([labPlugin]);

	let target: string = random().toHex();

	interface NamedColor {
		name: string;
		color: Colord;
	}

	const colors: NamedColor[] = (
		Object.keys(defaultColors) as (keyof typeof defaultColors)[]
	)
		.filter((name) => name.toLowerCase() === name)
		.flatMap((name): NamedColor[] => {
			const map = defaultColors[name];
			if (typeof map === "string") {
				return [{ name, color: colord(map) }];
			}
			return Object.entries(map).map(([shade, hex]) => ({
				name: `${name}-${shade}`,
				color: colord(hex),
			}));
		})
		.filter(({ color }) => color.isValid() && color.toRgb().a === 1);
</script>

<div class="flex items-center gap-4 px-2 mb-4">
	<ColorPicker
		bind:hex={target}
		label=""
		isAlpha={false}
		components={{ input: ColorPickerInput, wrapper: ColorPickerWrapper }}
	></ColorPicker>
	<input
		class="font-mono border rounded-lg py-2 px-4 w-48 text-xl"
		type="text"
		bind:value={target}
	/>
	<a
		class="ml-auto size-8 text-gray-800 hover:text-gray-600 transition-colors block"
		href="https://github.com/arumi-s/tailwindcss-color-matcher"
	>
		<svg
			role="img"
			viewBox="0 0 24 24"
			xmlns="http://www.w3.org/2000/svg"
			class="fill-current"
			><title>GitHub</title><path
				d="M12 .297c-6.63 0-12 5.373-12 12 0 5.303 3.438 9.8 8.205 11.385.6.113.82-.258.82-.577 0-.285-.01-1.04-.015-2.04-3.338.724-4.042-1.61-4.042-1.61C4.422 18.07 3.633 17.7 3.633 17.7c-1.087-.744.084-.729.084-.729 1.205.084 1.838 1.236 1.838 1.236 1.07 1.835 2.809 1.305 3.495.998.108-.776.417-1.305.76-1.605-2.665-.3-5.466-1.332-5.466-5.93 0-1.31.465-2.38 1.235-3.22-.135-.303-.54-1.523.105-3.176 0 0 1.005-.322 3.3 1.23.96-.267 1.98-.399 3-.405 1.02.006 2.04.138 3 .405 2.28-1.552 3.285-1.23 3.285-1.23.645 1.653.24 2.873.12 3.176.765.84 1.23 1.91 1.23 3.22 0 4.61-2.805 5.625-5.475 5.92.42.36.81 1.096.81 2.22 0 1.606-.015 2.896-.015 3.286 0 .315.21.69.825.57C20.565 22.092 24 17.592 24 12.297c0-6.627-5.373-12-12-12"
			/></svg
		>
	</a>
</div>
<div class="flex gap-4 flex-wrap mx-[-1px]">
	<ColorTable
		{colors}
		{target}
		distance={(a, b) => {
			const x = a.toRgb();
			const y = b.toRgb();
			return (
				Math.sqrt((x.r - y.r) ** 2 + (x.g - y.g) ** 2 + (x.b - y.b) ** 2) /
				Math.sqrt(3 * 255 * 255)
			);
		}}
		percent={true}
	>
		<svelte:fragment slot="title">RGB</svelte:fragment>
	</ColorTable>
	<ColorTable
		{colors}
		{target}
		distance={(a, b) => {
			const x = a.toHsl();
			const y = b.toHsl();
			const hueBias = 4;
			return (
				Math.sqrt(
					((hueBias * (x.h - y.h)) / 360) ** 2 +
						((x.s - y.s) / 100) ** 2 +
						((x.l - y.l) / 100) ** 2,
				) / Math.sqrt(hueBias ** 2 + 2)
			);
		}}
		percent={true}
	>
		<svelte:fragment slot="title">HSL</svelte:fragment>
	</ColorTable>
	<ColorTable
		{colors}
		{target}
		distance={(a, b) => {
			return a.delta(b);
		}}
	>
		<svelte:fragment slot="title">DeltaE</svelte:fragment>
	</ColorTable>
</div>

<style lang="postcss">
	:global(.color-picker) {
		display: block;
	}
</style>
