<template>
  <div class="action-screen spend-screen">
    <div class="screen-content">
      <h2>Spend Your KidCoins</h2>
      <div class="category-tabs">
        <button 
          v-for="category in categories" 
          :key="category"
          @click="selectedCategory = category"
          :class="['category-tab', { active: selectedCategory === category }]"
        >
          {{ category }}
        </button>
      </div>
      <div class="items-grid">
        <div 
          v-for="item in filteredItems" 
          :key="item.id" 
          class="item-card"
        >
          <div class="item-icon">{{ item.icon }}</div>
          <h3 class="item-name">{{ item.name }}</h3>
          <p class="item-price">{{ item.price }} KidCoins</p>
          <button @click="selectItem(item)" class="buy-button">Buy</button>
        </div>
      </div>
      <button @click="$emit('close')" class="close-button">Back to Wallet</button>
    </div>

    <!-- Purchase Confirmation Modal -->
    <div v-if="selectedItem" class="purchase-modal">
      <div class="modal-content">
        <h3>Confirm Purchase</h3>
        <p>Are you sure you want to buy {{ selectedItem.name }} for {{ selectedItem.price }} KidCoins?</p>
        <button @click="confirmPurchase" class="action-button">Yes, Buy It!</button>
        <button @click="selectedItem = null" class="close-button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'SpendScreen',
  data() {
    return {
      categories: ['Games', 'Education', 'Music', 'Movies'],
      selectedCategory: 'Games',
      items: [
        { id: 1, name: 'Math Adventure', icon: '🧮', price: 5, category: 'Games' },
        { id: 2, name: 'Space Explorer', icon: '🚀', price: 8, category: 'Games' },
        { id: 3, name: 'Language Lessons', icon: '🗣️', price: 10, category: 'Education' },
        { id: 4, name: 'Science Kit', icon: '🧪', price: 15, category: 'Education' },
        { id: 5, name: 'Kids Music Album', icon: '🎵', price: 3, category: 'Music' },
        { id: 6, name: 'Cartoon Movie', icon: '🎬', price: 7, category: 'Movies' },
        // Add more items as needed
      ],
      selectedItem: null
    }
  },
  computed: {
    filteredItems() {
      return this.items.filter(item => item.category === this.selectedCategory);
    }
  },
  methods: {
    selectItem(item) {
      this.selectedItem = item;
    },
    confirmPurchase() {
      if (this.selectedItem) {
        console.log(`Purchasing ${this.selectedItem.name} for ${this.selectedItem.price} KidCoins`);
        this.$emit('spend', { 
          item: this.selectedItem.name, 
          price: this.selectedItem.price 
        });
        this.selectedItem = null;
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
  background-color: #fff2e5;
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

.category-tabs {
  display: flex;
  justify-content: center;
  margin-bottom: 1.5rem;
}

.category-tab {
  padding: 0.5rem 1rem;
  margin: 0 0.5rem;
  border: none;
  border-radius: 20px;
  background-color: #e0e0e0;
  cursor: pointer;
  transition: background-color 0.3s;
}

.category-tab.active {
  background-color: #4CAF50;
  color: white;
}

.items-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(150px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.item-card {
  background-color: white;
  border-radius: 12px;
  padding: 1rem;
  text-align: center;
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
  transition: transform 0.2s;
}

.item-card:hover {
  transform: translateY(-5px);
}

.item-icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.item-name {
  font-size: 1.2rem;
  margin-bottom: 0.5rem;
}

.item-price {
  font-weight: bold;
  color: #4CAF50;
  margin-bottom: 1rem;
}

.buy-button {
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 20px;
  padding: 0.5rem 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.buy-button:hover {
  background-color: #45a049;
}

.close-button {
  display: block;
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  background-color: #9e9e9e;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
  margin-top: 1rem;
}

.close-button:hover {
  background-color: #8e8e8e;
}

.purchase-modal {
  position: fixed;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  display: flex;
  justify-content: center;
  align-items: center;
}

.modal-content {
  background-color: white;
  padding: 2rem;
  border-radius: 12px;
  text-align: center;
  max-width: 400px;
  width: 90%;
}

.action-button {
  display: block;
  width: 100%;
  padding: 1rem;
  border: none;
  border-radius: 8px;
  font-size: 1.1rem;
  font-weight: bold;
  background-color: #4CAF50;
  color: white;
  cursor: pointer;
  transition: background-color 0.3s;
  margin-top: 1rem;
}

.action-button:hover {
  background-color: #45a049;
}
</style>
