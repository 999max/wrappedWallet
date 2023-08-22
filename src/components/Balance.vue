<template>
  <div>
    <h2>Balance</h2>
    <p>Account: {{ accountAddress }}</p>
    <p>Your WETH balance: {{ tokenBalance }}</p>
    <p>Your Native balance: {{ nativeBalance }}</p>

  </div>
</template>

<script>
import { ethers } from 'ethers';

const WETH_ABI = [ "function balanceOf(address) view returns (uint256)" ];
const WETH_ADDRESS = "0x9c3C9283D3e44854697Cd22D3Faa240Cfb032889";

export default {
  name: 'Balance',
  data() {
    return {
      accountAddress: 'Please connect to Metamask on MumbaiTestnet',
      tokenBalance: 'loading...',
      nativeBalance: 'loading...'
    };
  },
  async mounted() {
    const provider = new ethers.providers.Web3Provider(window.ethereum);
    // @todo add network switcher

    await provider.send("eth_requestAccounts", []);
    const signer = provider.getSigner();
    // console.log("Account:", await signer.getAddress());
    this.accountAddress = window.ethereum.selectedAddress

    const contract = new ethers.Contract(WETH_ADDRESS, WETH_ABI, provider);
    const tokenBalance = await contract.balanceOf(window.ethereum.selectedAddress);
    this.tokenBalance = ethers.utils.formatEther(tokenBalance);
    
    const nativeBalance = await provider.getBalance(window.ethereum.selectedAddress);
    this.nativeBalance = ethers.utils.formatEther(nativeBalance);
  }
}
</script>
