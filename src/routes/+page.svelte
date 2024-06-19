<script>
	import { supabase } from '$lib/supabase';

	let name = '';
	let errorMessage = '';
	let successMessage = '';
	let namesPromise = supabase.from('rabbits').select();

	async function sendName() {
		try {
			const response = await fetch('/api/sendName', {
				method: 'POST',
				headers: {
					'Content-Type': 'application/json'
				},
				body: JSON.stringify({ name })
			});

			if (!response.ok) {
				throw new Error(await response.text());
			}

			const data = await response.json();
			successMessage = `The rabbit got added with the name: ${data[0].name}`;
			setTimeout(() => {
				successMessage = '';
			}, 5000); // Clear message after 5 seconds

			name = '';
			namesPromise = supabase.from('rabbits').select(); // Refresh the rabbit names
		} catch (error) {
			errorMessage = error.message;
			console.error(errorMessage); // Log the error to console for debugging
		}
	}

	function handleNameChange(event) {
		name = event.target.value;
		errorMessage = ''; // Reset error message on name change
	}
</script>

<div class="prose">
	<h1>APis</h1>

	<form>
		<h2>All my rabbits</h2>
		<input type="text" bind:value={name} on:input={handleNameChange} /><br />
		<button class="btn" on:click={sendName}>Add rabbit!</button>

		{#if errorMessage}
			<div role="alert" class="alert alert-error">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="stroke-current shrink-0 h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M10 14l2-2m0 0l2-2m-2 2l-2-2m2 2l2 2m7-2a9 9 0 11-18 0 9 9 0 0118 0z"
					/>
				</svg>
				<span>{errorMessage}</span>
			</div>
		{/if}

		{#if successMessage}
			<div role="alert" class="alert alert-success">
				<svg
					xmlns="http://www.w3.org/2000/svg"
					class="stroke-current shrink-0 h-6 w-6"
					fill="none"
					viewBox="0 0 24 24"
				>
					<path
						stroke-linecap="round"
						stroke-linejoin="round"
						stroke-width="2"
						d="M9 12l2 2 4-4m6 2a9 9 0 11-18 0 9 9 0 0118 0z"
					/>
				</svg>
				<span>{successMessage}</span>
			</div>
		{/if}
	</form>

	<h2>Rabbit names</h2>
	{#await namesPromise}
		<span class="loading loading-bars loading-lg text-primary" />
	{:then { data: rabbitNames }}
		<ul>
			{#each rabbitNames as rabbit (rabbit.id)}
				<li>{rabbit.name}</li>
			{/each}
		</ul>
	{/await}
</div>
