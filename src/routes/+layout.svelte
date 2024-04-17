<script lang="ts">
	import '../app.postcss';
	import {
		AppShell,
		AppBar,
		Toast,
		AppRail,
		AppRailAnchor,
		AppRailTile
	} from '@skeletonlabs/skeleton';
	import { initializeStores } from '@skeletonlabs/skeleton';
	import Icon from '@iconify/svelte';
	import { page } from '$app/stores';

	initializeStores();

	let currentTile = 'coucou';
</script>

<!-- App Shell -->
<AppShell>
	<Toast />
	<svelte:fragment slot="header">
		<!-- App Bar -->
		<AppBar>
			<svelte:fragment slot="lead">
				<strong class="text-xl uppercase">Skools</strong>
			</svelte:fragment>
			<svelte:fragment slot="trail">
				<a
					class="btn btn-sm variant-ghost-surface"
					href="https://github.com/nihiloproxima"
					target="_blank"
					rel="noreferrer"
				>
					GitHub
				</a>
			</svelte:fragment>
		</AppBar>
	</svelte:fragment>
	<svelte:fragment slot="sidebarLeft"
		><AppRail>
			<AppRailAnchor href="/" selected={$page.url.pathname === '/'}
				><Icon
					icon="cil:home"
					style="margin: 0 auto; text-center; font-size: 36px"
				/></AppRailAnchor
			>
			<!-- --- -->
			<AppRailAnchor href="/count" selected={$page.url.pathname === '/count'}
				><Icon
					icon="cil:calculator"
					style="margin: 0 auto; text-center; font-size: 36px"
				/></AppRailAnchor
			>
			<AppRailAnchor href="/words" selected={$page.url.pathname === '/words'}
				><Icon
					icon="iwwa:text-asc"
					style="margin: 0 auto; text-center; font-size: 36px"
				/></AppRailAnchor
			>
		</AppRail></svelte:fragment
	>

	<!-- Page Route Content -->
	<slot />
</AppShell>
