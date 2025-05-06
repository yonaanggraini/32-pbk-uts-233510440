<script setup>
import { ref } from 'vue';

const activities = ref([]);
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
  </div>
</template>