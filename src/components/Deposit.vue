<template>
  <div>
    <h2>Deposit</h2>
    <input v-model="amount" placeholder="Amount in ETH">
    <button @click="deposit">Deposit</button>
  </div>
</template>

<script>
import { ethers } from 'ethers';
const WETH_ABI = [ "function deposit() payable" ];
const WETH_ADDRESS = "0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889";

export default {
  name: 'Deposit',
  data() {
    return {
      amount: ''
    };
  },
  methods: {
    async deposit() {
      const provider = new ethers.providers.Web3Provider(window.ethereum);
      const signer = provider.getSigner();

      const contract = new ethers.Contract(WETH_ADDRESS, WETH_ABI, signer);
      const weiAmount = ethers.utils.parseEther(this.amount);
      await contract.deposit({ value: weiAmount });
    }
  }
}
</script>
