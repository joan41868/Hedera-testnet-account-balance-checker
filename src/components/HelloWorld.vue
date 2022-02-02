<template>
	<div class="hello bg-dark text-white" style="width: 60%; margin: auto;">
		<h3 class="mb-3">This app operates on TESTNET.</h3>
		<h4 class="text-info">Account ID: {{ accountId }}</h4>
		<h5 class="text-info">Hbar balance {{ hbarBalance }}</h5>
		<div class="token-balances text-info" v-if="tokenBalances.length > 0">
			<h5>Token balances:</h5>
			<h6 v-for="tokenBal in tokenBalances" v-bind:key="tokenBal">
				{{tokenBal}}
			</h6>
		</div>
		<p v-else class="text-warning">No HTS token balance found.</p>
		<div class="other-account-check">
			<h5>Check another account:</h5>
			<span style="display: flex; flex-direction: row; justify-content: space-between; width: 40%; margin: auto;">
				<b-input v-model="otherAccountId"/>
				<b-button variant="outline-info" class="submitButton" @click="loadAnotherAccount()">Check</b-button>
			</span>
		</div>
	</div>
</template>

<script>
import {AccountBalanceQuery, Client, HbarUnit} from "@hashgraph/sdk";

export default {
	name: 'HelloWorld',
	props: {
		accountId: String
	},
	data() {
		return {
			hbarBalance: '',
			tokenBalances: [],
			client: Client,

			otherAccountId: ''
		}
	},
	async mounted() {
		await this.loadAccountBalance();
	},
	methods:{
		async loadAccountBalance() {
			this.client = Client.forTestnet();
			const balance = await new AccountBalanceQuery()
				.setAccountId(this.accountId)
				.execute(this.client);
			this.hbarBalance = balance.hbars.toString(HbarUnit.Hbar);
			this.tokenBalances = balance.tokens;
			console.log(this.tokenBalances);
		},

		async loadAnotherAccount() {
			this.accountId = this.otherAccountId;
			await this.loadAccountBalance();
		}
	}
}
</script>
<style scoped>
h3 {
	margin: 40px 0 0;
}

ul {
	list-style-type: none;
	padding: 0;
}

li {
	display: inline-block;
	margin: 0 10px;
}

a {
	color: #42b983;
}
input{
	margin-right: 5px;
}
</style>
