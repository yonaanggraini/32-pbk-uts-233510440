<script setup>
import { ref } from 'vue';

const activities = ref([]);

const newActivity = ref('');

const addActivity = () => {
  if (newActivity.value.trim()) {
    activities.value.push({ text: newActivity.value, completed: false });
    newActivity.value = '';
  }
};
</script>

<template>
  <div class="page-wrapper">
    <div class="container">
      <h2 class="title">Daftar Kegiatan</h2>
      <ul class="activity-list">
        <li v-if="activities.length === 0" class="activity-item empty-item">
          Tidak ada kegiatan untuk ditampilkan.
        </li>
      </ul>
    </div>
    <div class="input-container">
    <input v-model="newActivity" class="input-field" placeholder="Tambahkan kegiatan baru" />
    <button @click="addActivity" class="add-button">Tambah</button>
  </div>
  </div>
</template>