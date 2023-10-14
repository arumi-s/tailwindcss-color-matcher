<script lang="ts">
	import defaultColors from "tailwindcss/colors";
	import { Colord, colord, extend } from "colord";
	import labPlugin from "colord/plugins/lab";
	import ColorTable from "./ColorTable.svelte";
	import ColorPicker from "svelte-awesome-color-picker";
	import ColorPickerInput from "./ColorPickerInput.svelte";
	import ColorPickerWrapper from "./ColorPickerWrapper.svelte";

	extend([labPlugin]);

	let target: string = "#76c6cc";
	//random({ alpha: 1 }).toHexString();

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

<div class="flex items-center space-x-4 px-2 mb-4">
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
