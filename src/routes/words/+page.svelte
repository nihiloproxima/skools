<script lang="ts">
	import _ from 'lodash';
	import diacritics from 'diacritics';

	let wordsCount = 3;
	let gridSize = 10;
	let words: string[] = [];
	let result: Array<Array<string | number>> = [];
	let modes: { [mode: string]: boolean } = {
		diagonal: true,
		horizontal: true,
		vertical: true,
		horizontalReverse: false,
		verticalReverse: false
	};

	const generate = () => {
		result = [];

		words = words
			.filter((word) => word !== undefined && word.length > 0)
			.map((word) => diacritics.remove(word).trim().toUpperCase());

		const longestWord = words.reduce((a, b) => (a.length > b.length ? a : b));
		gridSize = gridSize < longestWord.length ? longestWord.length : gridSize;

		console.log(`Grid size: ${gridSize} (${longestWord})`);

		generateGrid();

		for (let word of words) {
			placeWord(word);
		}

		cleanGrid();
		words = words.sort();
	};

	const generateGrid = () => {
		for (let i = 0; i < gridSize; i++) {
			result[i] = [];
			for (let j = 0; j < gridSize; j++) {
				result[i].push(0);
			}
		}
	};

	const placeWord = (word: string) => {
		let direction = _.sample(Object.keys(modes).filter((key) => modes[key] === true));

		let x = _.random(gridSize - 1);
		let y = _.random(gridSize - 1);

		if (direction === 'diagonal') {
			while (!canFitDiagonal(x, y, word)) {
				x = _.random(gridSize - 1, false);
				y = _.random(gridSize - 1);
			}
			return addDiagonalWord(x, y, word);
		} else if (direction === 'horizontal') {
			while (!canFitHorizontal(x, y, word)) {
				x = _.random(gridSize - 1);
				y = _.random(gridSize - 1);
			}
			return addHorizontal(x, y, word);
		} else if (direction === 'vertical') {
			while (!canFitVertical(x, y, word)) {
				x = _.random(gridSize - 1);
				y = _.random(gridSize - 1);
			}
			return addVertical(x, y, word);
		} else if (direction === 'verticalReverse') {
			while (!canFitVertical(x, y, word.split('').reverse().join(''))) {
				x = _.random(gridSize - 1);
				y = _.random(gridSize - 1);
			}
			return addVertical(x, y, word.split('').reverse().join(''));
		} else if (direction === 'horizontalReverse') {
			while (!canFitHorizontal(x, y, word.split('').reverse().join(''))) {
				x = _.random(gridSize - 1);
				y = _.random(gridSize - 1);
			}
			return addHorizontal(x, y, word.split('').reverse().join(''));
		}
		return false;
	};

	const addDiagonalWord = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			result[y + i][x + i] = word[i];
		}
		return true;
	};

	const canFitDiagonal = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			if (
				x + i >= gridSize ||
				y + i >= gridSize ||
				![0, word[i]].includes(result[y + i][x + i]) ||
				result[y + i] === undefined ||
				(result[y + i] && result[y + i][x + i] === undefined)
			) {
				return false;
			}
		}
		return true;
	};

	const addHorizontal = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			result[y][x + i] = word[i];
		}
		return true;
	};

	const canFitHorizontal = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			if (x + i >= gridSize || ![0, word[i]].includes(result[y][x + i])) {
				return false;
			}
		}
		return true;
	};

	const addVertical = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			result[y + i][x] = word[i];
		}
		return true;
	};

	const canFitVertical = (x: number, y: number, word: string) => {
		for (let i = 0; i < word.length; i++) {
			if (y + i >= gridSize || ![0, word[i]].includes(result[y + i][x])) {
				return false;
			}
		}
		return true;
	};

	const cleanGrid = () => {
		const alphabet = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ';
		for (let i = 0; i < result.length; i++) {
			for (let j = 0; j < result.length; j++) {
				if (result[i][j] === 0) {
					result[i][j] = _.sample(alphabet) as string;
				}
			}
		}
	};
</script>

<div class="h-full container mx-auto">
	<div class="text-center px-4">
		<h2 class="h2 mt-10">Générateur de mots fléchés</h2>
		<div class="mt-10 flex justify-center gap-4">
			<label class="">
				<p>Total mots</p>
				<input type="number" class="input w-32" bind:value={wordsCount} />
			</label>
			<label class="">
				<p>Taille de la grille</p>
				<input type="number" class="input w-32" bind:value={gridSize} />
			</label>
		</div>
		<div class="mt-10 flex gap-4 justify-center space-y-2">
			{#each Object.keys(modes) as mode}
				<label class="flex items-center space-x-2">
					<input class="checkbox" type="checkbox" bind:checked={modes[mode]} />
					<p>{mode}</p>
				</label>
			{/each}
		</div>

		<!-- / -->
		<div class=" space-x-2 mt-10">
			<div class="flex flex-wrap gap-4 justify-center">
				{#each Array(wordsCount) as _, i (i)}
					<div>
						<input class="input w-32" type="text" bind:value={words[i]} placeholder={'Mot ' + i} />
					</div>
				{/each}
			</div>
			<button class="btn variant-filled mt-5" on:click={generate}>Générer la grille</button>
		</div>

		{#if result.length > 0}
			<div class="mt-5 container">
				<div class="flex justify-between p-4 bg-white">
					<div>
						{#each words as word, i (i)}
							<h5 class="h5 text-black font-normal text-left">{word}</h5>
						{/each}
					</div>
					<table class="table-auto mx-auto border border-black text-black">
						{#each result as row, i (i)}
							<tr>
								{#each row as cell, j (j)}
									<td class="px-5 py-4 font-bold cell">{cell}</td>
								{/each}
							</tr>
						{/each}
					</table>
				</div>
			</div>
		{/if}
	</div>
</div>

<style scoped>
	.cell {
		font-family: Arial, 'Courier New', Courier, monospace;
	}
</style>
