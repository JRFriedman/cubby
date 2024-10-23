<template>
  <div class="wallet-container" :style="{ backgroundColor: bgColor }">
    <button @click="changeTheme" class="theme-change-btn" aria-label="Change theme">🎨</button>
    <BubblyTitle />
    <header>
      <div class="teddy-bear" aria-label="Teddy Bear">{{ currentEmoji }}</div>
      <h1>Welcome back, {{ username }}!</h1>
    </header>
    
    <main v-if="!activeScreen">
      <div class="balance-card">
        <h2>Your Balance</h2>
        <p class="balance">{{ balance }} {{ currency }}</p>
      </div>

      <div class="action-grid-container">
        <div class="action-grid">
          <action-button 
            v-for="action in actions" 
            :key="action.label" 
            v-bind="action"
            class="action-button"
            @click="openActionScreen(action.component)"
          />
        </div>
      </div>

      <div class="recent-activity cubby-style">
        <h3>Recent Activity</h3>
        <ul>
          <li v-for="(activity, index) in recentActivities" :key="index">
            {{ activity }}
          </li>
        </ul>
      </div>
    </main>

    <component 
      :is="activeScreen" 
      v-if="activeScreen" 
      @close="closeActionScreen"
      @buy="handleBuy"
      @sell="handleSell"
      @transfer="handleTransfer"
      @spend="handleSpend"
    />
  </div>
</template>

<script>
import { markRaw } from 'vue';
import ActionButton from './ActionButton.vue';
import BuyScreen from './BuyScreen.vue';
import SellScreen from './SellScreen.vue';
import TransferScreen from './TransferScreen.vue';
import SpendScreen from './SpendScreen.vue';
import BubblyTitle from './BubblyTitle.vue';

export default {
  name: 'KidWallet',
  components: {
    ActionButton,
    BuyScreen,
    SellScreen,
    TransferScreen,
    SpendScreen,
    BubblyTitle,
  },
  props: {
    username: {
      type: String,
      default: 'Kid'
    }
  },
  data() {
    return {
      balance: 50.25,
      currency: 'KidCoin',
      actions: [
        { icon: '🛒', label: 'Buy', component: markRaw(BuyScreen) },
        { icon: '💰', label: 'Sell', component: markRaw(SellScreen) },
        { icon: '🔄', label: 'Transfer', component: markRaw(TransferScreen) },
        { icon: '🛍️', label: 'Shop', component: markRaw(SpendScreen) },
      ],
      recentActivities: [
        'Received 5 KidCoin from Mom',
        'Spent 2 KidCoin on a game',
        'Saved 1 KidCoin in piggy bank',
      ],
      bgColor: '#f0f8ff',
      colorSchemes: [
        { bg: '#f0f8ff', text: '#4a4a4a', emoji: '🧸' },
        { bg: '#ffe5e5', text: '#ff4d4d', emoji: '🐻' },
        { bg: '#e5f2ff', text: '#4d94ff', emoji: '🐬' },
        { bg: '#e5ffe5', text: '#4dff4d', emoji: '🐸' },
        { bg: '#fff2e5', text: '#ffa64d', emoji: '🦁' },
        { bg: '#f2e5ff', text: '#9966ff', emoji: '🦄' },
      ],
      currentSchemeIndex: 0,
      activeScreen: null,
      currentEmoji: '🧸',
    };
  },
  methods: {
    changeTheme() {
      this.currentSchemeIndex = (this.currentSchemeIndex + 1) % this.colorSchemes.length;
      const newScheme = this.colorSchemes[this.currentSchemeIndex];
      this.bgColor = newScheme.bg;
      this.currentEmoji = newScheme.emoji;
      document.documentElement.style.setProperty('--color-text', newScheme.text);
    },
    openActionScreen(component) {
      this.activeScreen = component;
    },
    closeActionScreen() {
      this.activeScreen = null;
    },
    handleBuy(data) {
      console.log('Buy:', data);
      // Implement buy logic
      this.closeActionScreen();
    },
    handleSell(data) {
      console.log('Sell:', data);
      // Implement sell logic
      this.closeActionScreen();
    },
    handleTransfer(data) {
      console.log('Transfer:', data);
      // Implement transfer logic
      this.closeActionScreen();
    },
    handleSpend(data) {
      console.log('Spend:', data);
      // Implement spend logic
      this.closeActionScreen();
    },
  },
}
</script>

<style scoped>
.wallet-container {
  position: relative;
  max-width: 800px;
  margin: 0 auto;
  padding: 2rem;
  border-radius: 20px;
  box-shadow: 0 10px 20px rgba(0, 0, 0, 0.1);
  min-height: 100vh;
  box-sizing: border-box;
}

.theme-change-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  font-size: 30px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
  border-radius: 50%;
  transition: transform 0.3s, background-color 0.3s;
  z-index: 1000;
}

.theme-change-btn:hover {
  transform: rotate(180deg);
  background-color: rgba(255, 255, 255, 0.3);
}

.teddy-bear {
  font-size: 100px;
  text-align: center;
  margin-bottom: 1rem;
  animation: bounce 2s infinite;
}

@keyframes bounce {
  0%, 100% { transform: translateY(0); }
  50% { transform: translateY(-10px); }
}

header {
  text-align: center;
  margin-bottom: 2rem;
}

h1 {
  font-size: 2rem;
  font-weight: 700;
  color: #4a4a4a;
  margin-bottom: 0.5em;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

.balance-card {
  background: linear-gradient(135deg, #6dd5ed, #2193b0);
  border-radius: 20px;
  padding: 2rem;
  text-align: center;
  margin-bottom: 2rem;
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.balance-card h2 {
  margin: 0;
  font-size: 1.8rem;
  color: #ffffff;
}

.balance {
  font-size: 3rem;
  font-weight: bold;
  color: #ffffff;
  margin: 0.5rem 0 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.2);
}

.action-grid-container {
  max-width: 600px;
  margin: 0 auto;
  padding: 0 1rem;
}

.action-grid {
  display: grid;
  grid-template-columns: repeat(2, 1fr);
  gap: 1.5rem;
  margin-bottom: 2rem;
}

.recent-activity.cubby-style {
  background-color: #fff5e6;
  border: 8px solid #ffd700;
  border-radius: 20px;
  padding: 1.5rem;
  max-width: 600px;
  margin: 2rem auto;
  box-shadow: 0 8px 15px rgba(0, 0, 0, 0.1);
}

.recent-activity.cubby-style h3 {
  color: #ff6b6b;
  font-size: 1.8rem;
  margin-top: 0;
  text-align: center;
  text-transform: uppercase;
  letter-spacing: 1px;
}

.recent-activity.cubby-style ul {
  list-style-type: none;
  padding: 0;
}

.recent-activity.cubby-style li {
  background-color: #ffffff;
  border: 2px solid #ffd700;
  border-radius: 15px;
  margin-bottom: 1rem;
  padding: 1rem;
  font-size: 1.1rem;
  transition: transform 0.2s ease-in-out;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.05);
}

.recent-activity.cubby-style li:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.1);
}

.color-change-btn {
  position: absolute;
  top: 20px;
  left: 20px;
  font-size: 30px;
  background: none;
  border: none;
  cursor: pointer;
  padding: 10px;
  border-radius: 50%;
  transition: transform 0.3s, background-color 0.3s;
  z-index: 1000;
}

.color-change-btn:hover {
  transform: rotate(180deg);
  background-color: rgba(255, 255, 255, 0.3);
}

.action-button {
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  aspect-ratio: 1 / 1;
  width: 100%;
  padding: 1.5rem;
  border: none;
  border-radius: 20px;
  background-color: #ffffff;
  transition: transform 0.2s ease-in-out, box-shadow 0.2s ease-in-out;
  box-shadow: 0 6px 10px rgba(0, 0, 0, 0.1);
  cursor: pointer;
}

.action-button:hover {
  transform: translateY(-5px) scale(1.05);
  box-shadow: 0 10px 15px rgba(0, 0, 0, 0.15);
}

.action-button .icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.action-button .label {
  font-size: 1.2rem;
  font-weight: bold;
  color: #4a4a4a;
}

@media (max-width: 600px) {
  .wallet-container {
    padding: 1.5rem;
  }

  .teddy-bear {
    font-size: 80px;
  }

  h1 {
    font-size: 2rem;
  }

  .balance {
    font-size: 2.5rem;
  }

  .action-grid {
    gap: 1rem;
  }

  .action-button {
    padding: 1rem;
  }

  .action-button .icon {
    font-size: 2.5rem;
  }

  .action-button .label {
    font-size: 1rem;
  }
}

.action-screen {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: var(--color-bg);
  z-index: 1000;
  overflow-y: auto;
  padding: 2rem;
}

/* Add touch-friendly styles for mobile */
@media (max-width: 768px) {
  .theme-change-btn {
    top: 10px;
    left: 10px;
    font-size: 24px;
    padding: 8px;
  }

  .wallet-container {
    padding: 1rem;
    border-radius: 0;
  }

  .teddy-bear {
    font-size: 80px;
  }
}
</style>
