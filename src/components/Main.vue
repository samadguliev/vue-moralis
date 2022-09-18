<template>
  <div v-if="!isLoggedIn">
    <button class="connect-button" v-if="isMetamaskAvailable" @click="connectMetamask">
      Connect metamask
    </button>
    <span v-else>
      Install metamask
    </span>
  </div>

  <div v-else>
    <Wallet :address="address" />
  </div>
</template>

<script>
import Wallet from "./Wallet.vue";
export default {
  name: "Main",
  components: {
    Wallet
  },

  data() {
    return {
      isLoggedIn: ethereum.isConnected() && ethereum.selectedAddress !== null,
      address: ethereum.selectedAddress
    }
  },
  computed: {
    isMetamaskAvailable() {
      return Boolean(window.ethereum)
    },
  },
  methods: {
    async connectMetamask() {
      const accounts = await ethereum.request({ method: 'eth_requestAccounts' });
      this.address = accounts[0];
      this.isLoggedIn = true;
    }
  },
  mounted() {
    // console.log('window.ethereum', window.ethereum)
    // console.log('ethereum.selectedAddress', ethereum.selectedAddress)
  }
}
</script>

<style scoped>
  .connect-button {
    background-color: orange;
    color: #fff;
    padding: 15px 30px;
    font-size: 36px;
    border: 0;
  }
</style>