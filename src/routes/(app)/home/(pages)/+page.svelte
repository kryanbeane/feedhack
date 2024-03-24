<script lang="ts">
	import * as Card from "$lib/components/ui/card";
	import { getContext } from "svelte";
	import type { Writable } from "svelte/store";

	let adminSection: Writable<string> = getContext("adminSection");
	adminSection.set("home");
</script>

<svelte:head>
	<title>Home</title>
</svelte:head>

<h1 class="mb-1 text-2xl font-bold">Dashboard</h1>

<Card.Root>
	<Card.Header>
		<Card.Title>Card Title</Card.Title>
		<Card.Description>Card Description</Card.Description>
	</Card.Header>
	<Card.Content>
		<p>Card Content</p>
	</Card.Content>
	<Card.Footer>
		<p>Card Footer</p>
	</Card.Footer>
</Card.Root>
