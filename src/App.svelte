<script>
import { onMount } from "svelte";


	let titles=[
		"#",
		"Coin",
		"Price",
		"Price Change",
		"24 hours change",
	]
	let coins=[];
	let filterCoins=[];
	let reference=null;

	const loadCoins=async()=>{
		const response=await fetch('https://api.coingecko.com/api/v3/coins/markets?vs_currency=usd&order=market_cap_desc&per_page=100&page=1&sparkline=false');
		const data=await response.json();
		coins= data;
		filterCoins=data;
	}

	loadCoins();

	const searchCoin=(value)=>{
		filterCoins=coins.filter(coin=>coin.name.toLowerCase().includes(value.toLowerCase())||coin.symbol.toLowerCase().includes(value.toLowerCase()));
	}
	onMount(()=>{
		reference.focus();
	});
</script>

<div class="container">
	<div class="row">
	<h1 class="text-center">CoinMarket</h1>
	<input type="text" class="form-control bg-dark text-light my-4 border-0" placeholder="Search..." on:keyup={({target:{value}})=>searchCoin(value)} bind:this={reference}>
	<table class="table table-dark">
		<thead class="text-center">
			<tr>
			{#each titles as title}
			<th>{title}</th>
			{/each}
			</tr>
		</thead>
		<tbody>
			{#each filterCoins as coin,i}
				<tr>
					<td class="text-secondary fw-bold">{i+1}</td>
					<td>
						<img src={coin.image} alt={coin.name} width="25" class="img-fluid me-2">
						<span>
							{coin.name}
						</span>
						<span class="text-secondary text-uppercase ms-2">
							{coin.symbol}
						</span>
					</td>
					<td>
						{coin.current_price.toLocaleString()} <span class="text-primary">$USD</span>
					</td>
					<td class={coin.price_change_percentage_24h>0?"text-success":"text-danger"}>
						{coin.price_change_percentage_24h} %
					</td>
					<td>
						$ {coin.total_volume.toLocaleString()}
					</td>
				</tr>
			{/each}
		</tbody>
	</table>
	</div>
</div>
<style>
	:global(body){
		background-color: #222;
		color: #fff;
	}
</style>