<!-- src/routes/account/+page.svelte -->
<script lang="ts">
	import { enhance, type SubmitFunction } from '$app/forms';
	import Avatar from './Avatar.svelte';

	import { Section } from 'flowbite-svelte-blocks';
	import { Button, Fileupload, Input, Label } from 'flowbite-svelte';
	import { EnvelopeSolid, UserSolid } from 'flowbite-svelte-icons';

	export let data;
	export let form;

	let { session, supabase, profile } = data;
	$: ({ session, supabase, profile } = data);

	let profileForm: HTMLFormElement;
	let loading = false;
	let fullName: string = profile?.full_name ?? '';
	let username: string = profile?.username ?? '';
	let website: string = profile?.website ?? '';
	let avatarUrl: string = profile?.avatar_url ?? '';

	const handleSubmit: SubmitFunction = () => {
		loading = true;
		return async () => {
			loading = false;
		};
	};

	const handleSignOut: SubmitFunction = () => {
		loading = true;
		return async ({ update }) => {
			loading = false;
			update();
		};
	};
</script>

<Section name="portfolio">
	<h2 class="mb-4 text-xl font-bold text-gray-900 dark:text-white">Add a new product</h2>
	<form method="post" action="?/update" use:enhance={handleSubmit} bind:this={profileForm}>
		<Avatar
			{supabase}
			bind:url={avatarUrl}
			size={10}
			on:upload={() => {
				profileForm.requestSubmit();
			}}
		/>
		<div class="grid gap-4 sm:grid-cols-2 sm:gap-6">
			<div class="w-full">
				<Label for="email" class="mb-2">Email</Label>
				<Input id="email" type="email" value={session.user.email} required disabled>
					<EnvelopeSolid slot="left" />
				</Input>
			</div>
			<div class="w-full">
				<Label for="fullName" class="mb-2">Full Name</Label>
				<Input id="fullName" name="fullName" type="text" value={form?.fullName ?? fullName}>
					<UserSolid slot="left" />
				</Input>
			</div>
			<!--  -->
			<div class="w-full">
				<Label for="website" class="mb-2">Website</Label>
				<Input id="website" name="website" type="url" value={form?.website ?? website} />
			</div>
			<div class="w-full">
				<Label for="username" class="mb-2">Username</Label>
				<Input id="username" name="username" type="text" value={form?.username ?? username}>
					<UserSolid slot="left" />
				</Input>
			</div>
		</div>
		<Button type="submit">Submit</Button>
	</form>
</Section>
