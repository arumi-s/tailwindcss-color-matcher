<script lang="ts">
	import { Colord, colord } from "colord";

	export let colors: { name: string; color: Colord }[] = [];
	export let target: string;
	export let distance: (a: Colord, b: Colord) => number;
	export let percent = false;

	let targetColor: Colord;
	let filteredColors: { name: string; color: Colord; distance: number }[] = [];

	$: {
		targetColor = colord(target);
	}
	$: {
		const dists = colors.map(({ name, color }) => ({
			name,
			distance: distance(targetColor, color),
		}));
		filteredColors = dists
			.sort((a, b) => a.distance - b.distance)
			.slice(0, 10)
			.map(({ name, distance }) => ({
				...colors.find((c) => c.name === name)!,
				distance,
			}));
	}

	function selectText(
		event: MouseEvent & {
			currentTarget: EventTarget & HTMLTableCellElement;
		},
	) {
		if (event.target instanceof HTMLElement) {
			const selection = window.getSelection();
			if (selection) {
				event.preventDefault();
				const range = document.createRange();
				range.selectNodeContents(event.target);
				selection.removeAllRanges();
				selection.addRange(range);
			}
		}
	}
</script>

<table class="border border-collapse">
	<thead>
		<tr class="border">
			<th class="py-1 px-2 font-bold select-none" colspan="4">
				<slot name="title" />
			</th>
		</tr>
	</thead>
	<tbody>
		{#each filteredColors as color}
			<tr>
				<td class="py-1 px-2">
					<div
						class="rounded-full w-12 h-12 border-12"
						style:border-color={color.color.toHex()}
						style:background-color={targetColor.toHex()}
					/>
				</td>
				<td class="py-1 px-2" on:dblclick={selectText}>
					<code>{color.name}</code>
				</td>
				<td class="py-1 px-2" on:dblclick={selectText}>
					<code>{color.color.toHex()}</code>
				</td>
				<td class="py-1 px-2 text-right" on:dblclick={selectText}>
					{percent
						? `${(100 * color.distance).toFixed(2)}%`
						: color.distance.toFixed(2)}
				</td>
			</tr>
		{/each}
	</tbody>
</table>
