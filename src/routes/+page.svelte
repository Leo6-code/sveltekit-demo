<script>
	import { supabase } from '$lib/supabase';
	let promise = supabase.from('countries').select();
	let countries = [];
	promise.then((result) => {
		countries = result.data;
	});
</script>

<h1>Aufgaben:</h1>

<ul>
	{#each countries as country}
		<li>{country.name}</li>
	{/each}
</ul>

{#await promise}
	<p>
		<span class="loading loading-ring loading-lg" />
	</p>
{:then result}
	<div>{JSON.stringify(result)}</div>
{/await}
