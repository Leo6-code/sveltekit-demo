<script>
	import { supabase } from '$lib/supabase';
	import { onMount } from 'svelte';

	let transactionsList = ['🦝'];
	async function getTransactions() {
		let { data: transactions, error } = await supabase.from('transactions').select('*');
		transactionsList = transactions;
	}

	onMount(() => {
		getTransactions();
	});
</script>

<div>{JSON.stringify(transactionsList)}</div>
