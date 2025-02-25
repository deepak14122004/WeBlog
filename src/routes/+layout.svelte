<script lang="ts">
		import { ModeWatcher } from "mode-watcher";
	import '../app.css';
	
	import { onMount } from 'svelte';
	import { session } from '$lib/session';
	import { goto } from '$app/navigation';
	import { signOut } from 'firebase/auth';
	import { auth } from '$lib/firebase.client';

	import type { LayoutData, LayoutProps, PageProps } from './$types';
	import { Toaster } from "svelte-sonner";
	let { children,data }:LayoutProps = $props();

	let loading: boolean = true;
	let loggedIn: boolean = false;

	session.subscribe((cur: any) => {
	loading = cur?.loading;
	loggedIn = cur?.loggedIn;
	});

	onMount(async () => {
	const user: any = await data.getAuthUser();

	const loggedIn = !!user && user?.emailVerified;
	session.update((cur: any) => {
	loading = false;
	return {
		...cur,
		user,
		loggedIn,
		loading: false
	};
	});

	if (loggedIn) {
	goto('/');
	}
	});
</script>



{#if loading}
 <div>Loading...</div>
{:else}
   {@render children()}
{/if}

<Toaster/>
<ModeWatcher />


