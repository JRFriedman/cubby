<template>
  <div class="action-screen transfer-screen">
    <div class="screen-content">
      <h2>Transfer KidCoins</h2>
      <div class="address-book">
        <h3>Choose a friend:</h3>
        <div class="friend-grid">
          <button 
            v-for="friend in addressBook" 
            :key="friend.id" 
            class="friend-button"
            @click="selectFriend(friend)"
          >
            <span class="friend-icon">{{ friend.icon }}</span>
            <span class="friend-name">{{ friend.name }}</span>
          </button>
          <button class="friend-button add-friend" @click="showAddFriend = true">
            <span class="friend-icon">➕</span>
            <span class="friend-name">Add Friend</span>
          </button>
        </div>
      </div>
      <div v-if="selectedFriend" class="transfer-form">
        <h3>Send to {{ selectedFriend.name }}</h3>
        <div class="form-group">
          <label for="amount">Amount to send:</label>
          <input type="number" id="amount" v-model="amount" min="0.01" step="0.01">
        </div>
        <button @click="sendNow" class="action-button">Send Now</button>
      </div>
      <button @click="$emit('close')" class="close-button">Back to Wallet</button>
    </div>

    <!-- Add Friend Modal -->
    <div v-if="showAddFriend" class="add-friend-modal">
      <div class="modal-content">
        <h3>Add a New Friend</h3>
        <div class="form-group">
          <label for="friendName">Friend's Name:</label>
          <input type="text" id="friendName" v-model="newFriend.name">
        </div>
        <div class="form-group">
          <label for="friendAddress">Friend's Address:</label>
          <input type="text" id="friendAddress" v-model="newFriend.address">
        </div>
        <div class="form-group">
          <label for="friendIcon">Choose an Icon:</label>
          <select id="friendIcon" v-model="newFriend.icon">
            <option value="👧">👧</option>
            <option value="👦">👦</option>
            <option value="🐶">🐶</option>
            <option value="🐱">🐱</option>
            <option value="🦄">🦄</option>
          </select>
        </div>
        <button @click="addFriend" class="action-button">Add Friend</button>
        <button @click="showAddFriend = false" class="close-button">Cancel</button>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'TransferScreen',
  data() {
    return {
      addressBook: [
        { id: 1, name: 'Mom', icon: '👩', address: '0x1234...' },
        { id: 2, name: 'Dad', icon: '👨', address: '0x5678...' },
        { id: 3, name: 'Sister', icon: '👧', address: '0x9abc...' },
        { id: 4, name: 'Brother', icon: '👦', address: '0xdef0...' },
      ],
      selectedFriend: null,
      amount: 0,
      showAddFriend: false,
      newFriend: {
        name: '',
        address: '',
        icon: '👧'
      }
    }
  },
  methods: {
    selectFriend(friend) {
      this.selectedFriend = friend;
      this.amount = 0;
    },
    sendNow() {
      if (this.selectedFriend && this.amount > 0) {
        console.log(`Sending ${this.amount} KidCoins to ${this.selectedFriend.name} (${this.selectedFriend.address})`);
        this.$emit('transfer', { 
          to: this.selectedFriend.address, 
          amount: this.amount
        });
        this.selectedFriend = null;
        this.amount = 0;
      }
    },
    addFriend() {
      if (this.newFriend.name && this.newFriend.address) {
        this.addressBook.push({
          id: this.addressBook.length + 1,
          ...this.newFriend
        });
        this.showAddFriend = false;
        this.newFriend = { name: '', address: '', icon: '👧' };
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
  background-color: #e5ffe5;
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

.friend-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(120px, 1fr));
  gap: 1rem;
  margin-bottom: 2rem;
}

.friend-button {
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

.friend-button:hover {
  transform: translateY(-5px);
  box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
}

.friend-icon {
  font-size: 3rem;
  margin-bottom: 0.5rem;
}

.friend-name {
  font-size: 1.2rem;
  font-weight: bold;
}

.add-friend {
  background-color: #e0e0e0;
}

.transfer-form {
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

input, select {
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
  background-color: #9e9e9e;
  color: white;
  margin-top: 1rem;
}

button:hover {
  opacity: 0.9;
}

.add-friend-modal {
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
  background-color: #ffffff;
  padding: 2rem;
  border-radius: 12px;
  width: 90%;
  max-width: 400px;
}
</style>
