<script lang="ts">
	import diacritics from 'diacritics';

	let input = '';
	let result: Array<{
		line: string;
		parts: Array<string>;
		total: number;
		cumulative: number;
	}> = [];
	let total = 0;

	const count = () => {
		total = 0;
		result = [];

		const lines = input.split('\n');

		for (const line of lines) {
			// split lines by spaces
			if (!line.length) continue;

			const formatted = diacritics
				.remove(line)
				.replace(/[^0-9a-zA-Z]+/g, ' ')
				.trim();

			const parts = formatted.split(/\s+/);

			result.push({
				line,
				parts,
				total: parts.length,
				cumulative: total + parts.length
			});

			total += parts.length;
		}
	};
</script>

<div class="h-full container mx-auto">
	<div class="text-center px-4">
		<h2 class="h2 mt-10">Compteur de mots</h2>
		<p class="p">Entrez le texte dans la zone.</p>
		<!-- Animated Logo -->

		<!-- / -->
		<div class=" space-x-2 mt-10">
			<textarea
				class="textarea w-full"
				rows="10"
				placeholder="Lorem ipsum dolor sit amet consectetur adipisicing elit."
				bind:value={input}
				on:keyup={count}
			/>
		</div>

		<!-- Result -->
		{#if result.length > 0}
			<div class="mt-10">
				<h3 class="h3">
					Résultat: {total} mots sur {input.split('\n').filter((l) => l.length > 0).length} lignes.
				</h3>

				<table class="table table-compact table-hover table-auto text-left mt-5">
					<thead>
						<tr>
							<th>Ligne</th>
							<th class="table-cell-fit">Mots</th>
							<th class="table-cell-fit">Cumulés</th>
						</tr>
					</thead>
					<tbody>
						{#each result as { line, parts, total, cumulative }, i}
							<tr>
								<td class="w-[90%]">
									<p class="text-lg font-bold">{line}</p>
									<p class="text-xs opacity-55">{parts.join(' | ')}</p>
								</td>
								<td class="font-bold text-lg table-cell-fit">{total}</td>
								<td class="font-bold text-lg table-cell-fit">{cumulative}</td>
							</tr>
						{/each}
					</tbody>
				</table>
			</div>
		{/if}
	</div>
</div>
