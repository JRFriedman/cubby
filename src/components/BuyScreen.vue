<template>
  <div class="action-screen buy-screen">
    <div class="screen-content">
      <h2>Buy Crypto</h2>
      <div class="crypto-grid">
        <button 
          v-for="crypto in cryptoOptions" 
          :key="crypto.symbol" 
          class="crypto-button"
          @click="selectCrypto(crypto)"
        >
          <span class="crypto-icon">{{ crypto.icon }}</span>
          <span class="crypto-name">{{ crypto.name }}</span>
          <span class="crypto-price">{{ crypto.price }} KidCoins</span>
        </button>
      </div>
      <div v-if="selectedCrypto" class="buy-form">
        <h3>Buy {{ selectedCrypto.name }}</h3>
        <div class="form-group">
          <label for="amount">Amount (in KidCoins):</label>
          <input type="number" id="amount" v-model="amount" min="1" :max="maxAmount">
        </div>
        <button @click="buyNow" class="action-button">Buy Now</button>
      </div>
      <button @click="$emit('close')" class="close-button">Back to Wallet</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'BuyScreen',
  data() {
    return {
      cryptoOptions: [
        { name: 'KidCoin', symbol: 'KDC', icon: '🧒', price: 1 },
        { name: 'BitKid', symbol: 'BTK', icon: '🔷', price: 10 },
        { name: 'EtherJunior', symbol: 'ETJ', icon: '💎', price: 5 },
        { name: 'LiteLearner', symbol: 'LTL', icon: '🔹', price: 2 },
      ],
      selectedCrypto: null,
      amount: 1,
      maxAmount: 100 // This should be dynamically set based on the user's balance
    }
  },
  methods: {
    selectCrypto(crypto) {
      this.selectedCrypto = crypto;
      this.amount = 1;
    },
    buyNow() {
      if (this.selectedCrypto && this.amount > 0) {
        console.log(`Buying ${this.amount} ${this.selectedCrypto.symbol} for ${this.amount * this.selectedCrypto.price} KidCoins`);
        this.$emit('buy', { 
          crypto: this.selectedCrypto.symbol, 
          amount: this.amount, 
          totalPrice: this.amount * this.selectedCrypto.price 
        });
        this.selectedCrypto = null;
        this.amount = 1;
      }
    }
  }
}
</script>

<style scoped>
.action-screen {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: #e5f2ff;
  z-index: 1000;
  overflow-y: auto;
  padding: 2rem;
}

.screen-content {
  max-width: 800px;
  margin: 0 auto;
}

h2, h3 {
  text-align: center;
  margin-bottom: 1.5rem;
  color: #4a4a4a;
}

.crypto-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.crypto-button {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 1rem;
  border: none;
  border-radius: 12px;
  background-color: #ffffff;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s, box-shadow 0.2s;
  cursor: pointer;
}

.crypto-button:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

.crypto-icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.crypto-name {
  font-size: 1.2rem;
  font-weight: bold;
  margin-bottom: 0.5rem;
}

.crypto-price {
  font-size: 1rem;
  color: #4CAF50;
}

.buy-form {
  background-color: #ffffff;
  padding: 1.5rem;
  border-radius: 12px;
  margin-bottom: 1.5rem;
}

.form-group {
  margin-bottom: 1rem;
}

label {
  display: block;
  margin-bottom: 0.5rem;
  font-weight: bold;
}

input {
  width: 100%;
  padding: 0.75rem;
  border: 2px solid #4CAF50;
  border-radius: 8px;
  font-size: 1rem;
}

.action-button, .close-button {
  display: block;
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  cursor: pointer;
  transition: background-color 0.3s;
}

.action-button {
  background-color: #4CAF50;
  color: white;
  margin-top: 1rem;
}

.close-button {
  background-color: #f44336;
  color: white;
  margin-top: 1rem;
}

button:hover {
  opacity: 0.9;
}
</style>
