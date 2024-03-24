<script lang="ts">
	import { goto } from "$app/navigation";
	import { onMount } from "svelte";
	import { page } from "$app/stores";
	import Button from "$lib/components/ui/button/button.svelte";

	export let data;
	let { supabase } = data;

	onMount(() => {
		supabase.auth.onAuthStateChange((event) => {
			// Redirect to account after sucessful login
			if (event == "SIGNED_IN") {
				// Delay needed because order of callback not guaranteed.
				// Give the layout callback priority to update state or
				// we'll just bounch back to login when /home tries to load
				setTimeout(() => {
					goto("/home");
				}, 1);
			}
		});
	});

	const handleSignIn = async () => {
		await supabase.auth
			.signInWithOAuth({
				provider: "google",
				options: {
					redirectTo: `${data.url}/auth/callback`,
					queryParams: {
						access_type: "offline",
						prompt: "consent",
					},
				},
			})
			.catch((err) => {
				console.log("Login error", err);
				throw err;
			});
	};
</script>

<svelte:head>
	<title>Log In</title>
</svelte:head>

<div>
	<h1 class="text-xl font-bold">Get Started</h1>

	{#if $page.url.searchParams.get("verified") == "true"}
		<div role="alert" class="alert alert-success mb-5">
			<svg
				xmlns="http://www.w3.org/2000/svg"
				class="h-6 w-6 shrink-0 stroke-current"
				fill="none"
				viewBox="0 0 24 24"
				><path
					stroke-linecap="round"
					stroke-linejoin="round"
					stroke-width="2"
					d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
				/></svg
			>
			<span>Email verified! Please sign in.</span>
		</div>
	{/if}
	<h1 class="mb-6 text-2xl font-bold">Sign In</h1>
	<Button
		variant="secondary"
		type="button"
		class="flex items-center justify-center"
		on:click={handleSignIn}
		>Google
	</Button>
	<!-- redirectTo={`https://jsnmvywebfzrolwnzyax.supabase.co/auth/v1/callback`} -->
</div>
