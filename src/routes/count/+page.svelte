<script lang="ts">
	import { getToastStore, type ToastSettings } from '@skeletonlabs/skeleton';
	import diacritics from 'diacritics';

	const toastStore = getToastStore();

	const t: ToastSettings = {
		message: 'Copié dans le presse-papier !'
	};

	let input = '';
	let splitted = [];

	const count = () => {
		const lines = input.split('\n');

		let total = 0;
		let result = '';

		for (const line of lines) {
			// split lines by spaces
			if (!line.length) continue;

			const formatted = diacritics
				.remove(line)
				.replace(/[^0-9a-zA-Z]+/g, ' ')
				.trim();
			console.log(formatted);
			const parts = formatted.split(/\s+/);
			console.log(parts);

			total += parts.length;
			const newLine = `${line} | ${total}\n`;

			result += newLine;
			console.log(newLine);
		}

		input = result;
		navigator.clipboard.writeText(input);
		toastStore.trigger(t);
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
			/>
			<button class="btn variant-filled mt-5" on:click={count}>Compter</button>
		</div>
	</div>
</div>
