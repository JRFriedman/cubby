<template>
  <div class="action-screen sell-screen">
    <div class="screen-content">
      <h2>Sell Crypto</h2>
      <div class="crypto-grid">
        <button 
          v-for="holding in userHoldings" 
          :key="holding.symbol" 
          class="crypto-button"
          @click="selectCrypto(holding)"
        >
          <span class="crypto-icon">{{ holding.icon }}</span>
          <span class="crypto-name">{{ holding.name }}</span>
          <span class="crypto-amount">You have: {{ holding.amount }}</span>
        </button>
      </div>
      <div v-if="selectedCrypto" class="sell-form">
        <h3>Sell {{ selectedCrypto.name }}</h3>
        <div class="form-group">
          <label for="amount">Amount to sell:</label>
          <input type="number" id="amount" v-model="amount" min="0.01" :max="selectedCrypto.amount" step="0.01">
        </div>
        <p class="estimated-return">You'll get: {{ estimatedReturn }} KidCoins</p>
        <button @click="sellNow" class="action-button">Sell Now</button>
      </div>
      <button @click="$emit('close')" class="close-button">Back to Wallet</button>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SellScreen',
  data() {
    return {
      userHoldings: [
        { name: 'KidCoin', symbol: 'KDC', icon: '🧒', amount: 100, price: 1 },
        { name: 'BitKid', symbol: 'BTK', icon: '🔷', amount: 5, price: 10 },
        { name: 'EtherJunior', symbol: 'ETJ', icon: '💎', amount: 2, price: 5 },
        { name: 'LiteLearner', symbol: 'LTL', icon: '🔹', amount: 10, price: 2 },
      ],
      selectedCrypto: null,
      amount: 0
    }
  },
  computed: {
    estimatedReturn() {
      if (this.selectedCrypto && this.amount) {
        return (this.amount * this.selectedCrypto.price).toFixed(2);
      }
      return '0.00';
    }
  },
  methods: {
    selectCrypto(crypto) {
      this.selectedCrypto = crypto;
      this.amount = 0;
    },
    sellNow() {
      if (this.selectedCrypto && this.amount > 0 && this.amount <= this.selectedCrypto.amount) {
        console.log(`Selling ${this.amount} ${this.selectedCrypto.symbol} for ${this.estimatedReturn} KidCoins`);
        this.$emit('sell', { 
          crypto: this.selectedCrypto.symbol, 
          amount: this.amount, 
          totalReturn: parseFloat(this.estimatedReturn)
        });
        this.selectedCrypto = null;
        this.amount = 0;
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
  background-color: #ffe5e5;
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

.crypto-amount {
  font-size: 1rem;
  color: #4CAF50;
}

.sell-form {
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
  border: 2px solid #f44336;
  border-radius: 8px;
  font-size: 1rem;
}

.estimated-return {
  font-size: 1.2rem;
  font-weight: bold;
  text-align: center;
  margin-bottom: 1rem;
  color: #4CAF50;
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
  background-color: #f44336;
  color: white;
  margin-top: 1rem;
}

.close-button {
  background-color: #9e9e9e;
  color: white;
  margin-top: 1rem;
}

button:hover {
  opacity: 0.9;
}
</style>
