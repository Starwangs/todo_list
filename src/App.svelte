<script>
	import Profile from "./Profile.svelte";
	import Todos from "./Todos.svelte";

	import { auth, googleProvider } from "./firebase";
	import { authState } from "rxfire/auth";
	import { scale } from "svelte/transition";

	let user;

	const unsubscribe = authState(auth).subscribe((u) => (user = u));

	function login() {
		auth.signInWithPopup(googleProvider);
	}
</script>

<main>
	{#if user}
		<div
			class="min-h-screen bg-gray-100 flex flex-col items-center justify-center"
		>
			<Profile {...user}
				><button
					class="h-10 w-20 text-white rounded-lg bg-red-500 hover:bg-red-600"
					on:click={() => auth.signOut()}
					transition:scale={{ delay: 1100 }}>Logout</button
				></Profile
			>
			<div class="my-3" />
			<Todos uid={user.uid} displayName={user.displayName} />
		</div>
	{:else}
		<div
			class="min-h-screen bg-gray-100 flex flex-col items-center justify-center"
		>
			<button
				class="h-10 w-40 text-white rounded-lg bg-green-500 hover:bg-green-600"
				on:click={login}>Signin with Google</button
			>
		</div>
	{/if}
</main>

<style></style>
