<template>
  <section class="section">
	<table class="table is-bordered is-striped is-narrow is-hoverable is-fullwidth">
		<thead>
			<tr class="is-selected">
			  <th>#</th>
			  <th>Name</th>
			  <th>Market Cap</th>
			  <th>Price</th>
			  <th>Volume (24h)</th>
			  <th>Change (1h)</th>
			  <th>Circulating Supply</th>
			  <th>Open (24h)</th>
			</tr>
		</thead>
		<tfoot>
			<tr>
			  <th>#</th>
			  <th>Name</th>
			  <th>Market Cap</th>
			  <th>Price</th>
			  <th>Volume (24h)</th>
			  <th>Change (1h)</th>
			  <th>Circulating Supply</th>
			  <th>Open (24h)</th>
			</tr>
		</tfoot>
		<tbody>
			<tr v-for="(value, key, index) in coins">
			  <th>{{index+1}}</th>
			  <td>{{key}}</td>
			  <td>{{value.USD.MKTCAP}}</td>
			  <td>{{value.USD.PRICE}}</td>
			  <td>{{value.USD.VOLUME24HOUR}}</td>
			  <td v-bind:style="getColor(value.USD.CHANGEPCTHOUR)">{{value.USD.CHANGEPCTHOUR}}</td>
			  <td>{{value.USD.SUPPLY}}</td>
			  <td>{{value.USD.OPEN24HOUR}}</td>
			</tr>
		</tbody>
	</table>
  </section>
</template>

<script>

import axios from 'axios'

export default {
  name: 'home',
  data: () => ({
    coins: [],
    errors: []
  }),

  created () {
    //https://min-api.cryptocompare.com/data/pricemulti?fsyms=BTC,ETH,IOT&tsyms=USD
    //this.cryptos = response.data
		this.getData();
			setInterval(() => {
				this.getData();
			}, 2 * 1000);
	},
	
	methods: {
		getData() {
 		axios.get('https://min-api.cryptocompare.com/data/pricemultifull?fsyms=BTC,ETH,XRP,LTC,DASH&tsyms=USD,EUR')
      .then(response => {
      	this.coins = response.data.DISPLAY
        console.log(response)
      })
      .catch(e => {
        this.errors.push(e)
      })
		},
		getColor: (num) => {
      return num > 0 ? "background:green;color:white;" : "background:red;color:white;";
    },
	},

}
</script>
