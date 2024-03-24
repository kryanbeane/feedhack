<script lang="ts">
	import { getContext } from "svelte";
	import type { Writable } from "svelte/store";
	import SettingsModule from "./settings_module.svelte";

	let adminSection: Writable<string> = getContext("adminSection");
	adminSection.set("settings");

	export let data;
	let { session, profile } = data;
</script>

<svelte:head>
	<title>Settings</title>
</svelte:head>

<h1 class="mb-6 text-2xl font-bold">Settings</h1>

<SettingsModule
	title="Profile"
	editable={false}
	fields={[
		{
			id: "fullName",
			label: "Name",
			initialValue: profile?.full_name ?? "",
		},
		{
			id: "companyName",
			label: "Company Name",
			initialValue: profile?.company_name ?? "",
		},
		{
			id: "website",
			label: "Company Website",
			initialValue: profile?.website ?? "",
		},
	]}
	editButtonTitle="Edit Profile"
	editLink="/home/settings/edit_profile"
/>

<SettingsModule
	title="Danger Zone"
	editable={false}
	dangerous={true}
	fields={[]}
	editButtonTitle="Delete Account"
	editLink="/home/settings/delete_account"
/>
