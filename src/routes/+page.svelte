<!-- YOU CAN DELETE EVERYTHING IN THIS PAGE -->

<div class="container h-full mx-auto flex justify-center items-center">
	<div class="space-y-10 text-center flex flex-col items-center">
		<h2 class="h2">Bienvenue sur Skools</h2>
		<!-- Animated Logo -->

		<!-- / -->
		<div class="flex flex-col gap-6 max-w-2xl">
			<h3 class="h3">Votre boîte à outils numérique pour l'enseignement</h3>
			<p class="text-lg">
				Skools rassemble une collection d'outils pratiques conçus spécifiquement pour les
				enseignants, facilitant la gestion de classe et l'organisation pédagogique au quotidien.
			</p>

			<div class="grid grid-cols-1 md:grid-cols-2 gap-4">
				<a href="/count" class="card p-4 variant-soft hover:variant-filled-primary">
					<h4 class="h4">Compteur de mots</h4>
					<p>Entrez votre texte et obtenez le nombre de mots par ligne</p>
				</a>
				<a href="/words" class="card p-4 variant-soft hover:variant-filled-primary">
					<h4 class="h4">Mots cachés</h4>
					<p>Générez des grilles de mots cachés personnalisées</p>
				</a>
			</div>
		</div>
	</div>
</div>

<style lang="postcss">
	figure {
		@apply flex relative flex-col;
	}
	figure svg,
	.img-bg {
		@apply w-64 h-64 md:w-80 md:h-80;
	}
	.img-bg {
		@apply absolute z-[-1] rounded-full blur-[50px] transition-all;
		animation:
			pulse 5s cubic-bezier(0, 0, 0, 0.5) infinite,
			glow 5s linear infinite;
	}
	@keyframes glow {
		0% {
			@apply bg-primary-400/50;
		}
		33% {
			@apply bg-secondary-400/50;
		}
		66% {
			@apply bg-tertiary-400/50;
		}
		100% {
			@apply bg-primary-400/50;
		}
	}
	@keyframes pulse {
		50% {
			transform: scale(1.5);
		}
	}
</style>
