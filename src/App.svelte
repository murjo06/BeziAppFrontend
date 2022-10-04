<script lang="ts">
	import {Router, Route} from "svelte-navigator";
	import Drawer from "./Drawer.svelte";
	import {AppContent, Scrim} from "@smui/drawer";
	import Error from "./Widgets/Error.svelte";
	import {Icon, Label} from "@smui/button";
	import isMobile from "is-mobile";
	import Button from "@smui/button";

	const mobile = isMobile();
	let open = !mobile;
</script>

<div class="drawer-container">
	<Router let:location>
		<Drawer open={open} statusFunction={(o) => open=o} />
		<AppContent class="app-content">
			<main class="main-content">
				{#if mobile && !(document.location.pathname === "/login")}
					<Button on:click={() => open = !open}>
						<Icon class="material-icons">menu_open</Icon>
						{#if open}
							<Label>Zapri navigacijo</Label>
						{:else}
							<Label>Odpri navigacijo</Label>
						{/if}
					</Button>
				{/if}
				<div>
					<Route path="/login">
						{#await import('./Login.svelte') then Login}
							<Login.default />
						{:catch e}
							<Error err={e} />
						{/await}
					</Route>
					<Route path="/lopolis/login">
						{#await import('./Login.svelte') then Login}
							<Login.default loginType="lopolis" />
						{:catch e}
							<Error err={e} />
						{/await}
					</Route>
					<Route path="/lopolis">
						{#await import('./LoPolis.svelte') then LoPolis}
							<LoPolis.default />
						{:catch e}
							<Error err={e} />
						{/await}
					</Route>
					<Route path="/napaka">
						{#await import('./ErrorPage.svelte') then ErrorPage}
							<ErrorPage.default />
						{:catch e}
							<Error err={e} />
						{/await}
					</Route>
					<Route path="/">
						{#await import('./Home.svelte') then Home}
							<Home.default />
						{:catch e}
							<Error err={e} />
						{/await}
					</Route>
				</div>
			</main>
		</AppContent>
	</Router>
</div>
