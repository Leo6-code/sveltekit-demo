<script>
	import { supabase } from '$lib/supabase';
	let promise = supabase.from('countries').select();
</script>

{#await promise}
	<p>Loading...</p>
{:then result}
	<div>{JSON.stringify(result)}</div>
{/await}
