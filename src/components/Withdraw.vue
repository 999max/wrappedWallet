<template>
  <div>
    <h2>Withdraw</h2>
    <input v-model="amount" placeholder="Amount in WETH">
    <button @click="withdraw">Withdraw</button>
  </div>
</template>

<script>
import { ethers } from 'ethers';
const WETH_ABI = [ "function withdraw(uint256)" ];
const WETH_ADDRESS = "0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889";

export default {
  name: 'Withdraw',
  data() {
    return {
      amount: ''
    };
  },
  methods: {
    async withdraw() {
      const provider = new ethers.providers.Web3Provider(window.ethereum);
      const signer = provider.getSigner();
      console.log('withdraw')
      const contract = new ethers.Contract(WETH_ADDRESS, WETH_ABI, signer);
      console.log('amount', this.amount)
      const weiAmount = ethers.utils.parseEther(this.amount);
      await contract.withdraw(weiAmount);
    }
  }
}
</script>
