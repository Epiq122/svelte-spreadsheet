<script lang="ts">
	import { numberToAlphabet, type Cell } from './sheet-utils';

	let { data }: { data: Cell[][] } = $props();

	let numRows = $derived(data.length > 10 ? data.length : 10);
	let numCols = $derived.by(() => {
		const largestRow = Math.max(...data.map((row) => row.length));
		return largestRow > 10 ? largestRow : 10;
	});
</script>

<table class="border-collapse font-sans">
	<tbody>
		{#each { length: numRows + 1 }, row}
			<tr>
				{#each { length: numCols + 1 }, column}
					{@const cellData = data[row - 1]?.[column - 1]?.value}
					<svelte:element
						this={row === 0 || column === 0 ? 'th' : 'td'}
						class="box-border h-7.5 min-w-25 border border-[#393939] {row === 0 || column === 0
							? 'bg-[#191919]'
							: 'bg-[#222]'}"
						scope={row === 0 ? 'col' : column === 0 ? 'row' : undefined}
					>
						{#if row === 0 && column > 0}
							{numberToAlphabet(column)}
						{/if}

						{#if row > 0 && column === 0}
							{row}
						{/if}
						{cellData || ''}
						{#if row > 0 && column > 0}{/if}
					</svelte:element>
				{/each}
			</tr>
		{/each}
	</tbody>
</table>
