 <script setup>
import { ref } from 'vue';

const items = ref([]);
const newItem = ref('');
const showOnlyAvailable = ref(false);
const editingIndex = ref(null);
const editedName = ref('');

const addItem = () => {
  if (newItem.value.trim()) {
    items.value.push({ name: newItem.value, available: true });
    newItem.value = '';
  }
};

const removeItem = (index) => {
  items.value.splice(index, 1);
};

const toggleAvailability = (index) => {
  items.value[index].available = !items.value[index].available;
};

const startEditing = (index) => {
  editingIndex.value = index;
  editedName.value = items.value[index].name;
};

const saveEdit = (index) => {
  if (editedName.value.trim()) {
    items.value[index].name = editedName.value;
    editingIndex.value = null;
  }
};

const cancelEdit = () => {
  editingIndex.value = null;
};

const filteredItems = () => {
  return showOnlyAvailable.value
    ? items.value.filter(item => item.available)
    : items.value;
};

const totalItems = () => items.value.length;
const availableItems = () =>
  items.value.filter(i => i.available).length;
</script>

<template>
  <div class="page-wrapper">
    <div class="container">
      <h2 class="title">🎡Manajemen Inventaris Gudang🎡</h2>

      <div class="input-container">
        <input v-model="newItem" class="input-field" placeholder="Nama barang baru" />
        <button @click="addItem" class="add-button">Tambah</button>
      </div>

      <label class="filter-label">
        <input type="checkbox" v-model="showOnlyAvailable" />
        Tampilkan hanya yang tersedia
      </label>

      <div class="summary">
        <p>Total Barang: {{ totalItems() }} | Tersedia: {{ availableItems() }}</p>
      </div>

      <ul class="activity-list">
        <li v-if="filteredItems().length === 0" class="activity-item empty-item">
          Tidak ada barang untuk ditampilkan.
        </li>

        <li v-for="(item, index) in filteredItems()" :key="index" class="activity-item">
          <input type="checkbox" :checked="item.available" @change="toggleAvailability(index)" />

          <template v-if="editingIndex === index">
            <input v-model="editedName" class="input-field small" />
            <button @click="saveEdit(index)" class="add-button small">Simpan</button>
            <button @click="cancelEdit" class="delete-button small">Batal</button>
          </template>

          <template v-else>
            <span class="activity-text" :class="{ completed: !item.available }">
              {{ item.name }}
            </span>
            <div class="button-group">
              <button @click="startEditing(index)" class="add-button small">Edit</button>
              <button @click="removeItem(index)" class="delete-button small">Hapus</button>
            </div>
          </template>
        </li>
      </ul>

      <!-- Google Fonts -->
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=Cal+Sans&display=swap" rel="stylesheet">
    </div>
  </div>
</template>

<style scoped>
.page-wrapper {
  font-family: 'Cal Sans', sans-serif;
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
  background: linear-gradient(to right, #98cee4, #98cee4);
  padding: 20px;
}

.container {
  background: linear-gradient(to bottom right, #c8f1e9);
  padding: 24px;
  border-radius: 16px;
  box-shadow: 0 6px 20px rgba(0, 0, 0, 0.15);
  max-width: 540px;
  width: 100%;
  color: #333;
}

.title {
  text-align: center;
  margin-bottom: 20px;
  font-weight: normal;
  font-size: 1.6rem;
  color: #333;
}

.input-container {
  display: flex;
  gap: 10px;
  justify-content: center;
}

.input-field {
  flex-grow: 1;
  padding: 10px;
  border-radius: 8px;
  border: 1.5px solid #bbb;
  background: #fff;
  color: #333;
  font-family: 'Cal Sans', sans-serif;
  font-size: 1rem;
}

.input-field.small {
  flex-grow: 0;
  width: 50%;
}

.add-button, .delete-button {
  background: linear-gradient(to right, #61abef);
  color: #333;
  cursor: pointer;
  padding: 8px 14px;
  border-radius: 8px;
  border: none;
  font-weight: normal;
  font-family: 'Cal Sans', sans-serif;
  font-size: 0.95rem;
  transition: all 0.2s ease;
}

.add-button:hover, .delete-button:hover {
  background: linear-gradient(to right, #7bc56a, #1aa34a);
}

.add-button.small, .delete-button.small {
  padding: 6px 10px;
  font-size: 0.85rem;
}

.filter-label {
  display: flex;
  align-items: center;
  margin-top: 15px;
  gap: 8px;
  font-weight: normal;
  color: #444;
}

.summary {
  margin-top: 10px;
  text-align: center;
  font-weight: normal;
  color: #555;
}

.activity-list {
  list-style: none;
  padding: 0;
  margin-top: 15px;
}

.activity-item {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: linear-gradient(to right, #c3cfbb, #e4d9f7);
  padding: 12px;
  border-radius: 10px;
  margin-bottom: 10px;
  gap: 10px;
}

.activity-text {
  flex-grow: 1;
  font-weight: normal;
  font-family: 'Cal Sans', sans-serif;
  font-size: 1rem;
}

.activity-text.completed {
  text-decoration: line-through;
  color: #999;
}

.button-group {
  display: flex;
  gap: 6px;
}

.empty-item {
  justify-content: center;
  font-style: italic;
  font-weight: normal;
  color: #777;
}
</style>