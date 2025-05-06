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

    </div>
  </div>
</template>

