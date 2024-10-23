<template>
  <button @click="$emit('click')" class="action-button">
    <span class="icon">{{ icon }}</span>
    <span class="label">{{ label }}</span>
  </button>
</template>

<script>
export default {
  name: 'ActionButton',
  props: {
    icon: String,
    label: String,
  },
}
</script>

<style scoped>
.action-button {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  background-color: #fff;
  border: none;
  border-radius: 12px;
  padding: 1rem;
  cursor: pointer;
  color: #333;
  transition: transform 0.2s, box-shadow 0.2s;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}
.action-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
}
.icon {
  font-size: 2rem;
  margin-bottom: 0.5rem;
}
.label {
  font-size: 1rem;
  font-weight: bold;
}
</style>
