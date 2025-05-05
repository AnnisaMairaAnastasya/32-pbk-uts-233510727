<template>
  <div class="wadah">
    <h1> Daftar Tugas Harian</h1>

    <form @submit.prevent="tambahTugas" class="formulir">
      <input v-model="tugasBaru" placeholder="Ketik kegiatan baru..." />
      <button type="submit">➕ Tambah</button>
    </form>

    <!-- Dropdown Filter -->
    <div class="filter-dropdown">
      <label for="filter">Tampilkan:</label>
      <select id="filter" v-model="filter">
        <option value="semua"> Semua</option>
        <option value="belum"> Belum Selesai</option>
        <option value="selesai"> Selesai</option>
      </select>
    </div>

    <ul>
      <transition-group name="fade" tag="div">
        <li
          v-for="(tugas, index) in daftarTugasTersaring"
          :key="'tugas-' + index"
          :class="{ selesai: tugas.selesai }"
        >
          <input type="checkbox" v-model="tugas.selesai" />
          <span>{{ tugas.teks }}</span>
          <button class="hapus" @click="hapusTugas(index)">❌</button>
        </li>
      </transition-group>
      <li v-if="daftarTugasTersaring.length === 0" class="kosong">
        Tidak ada tugas untuk ditampilkan.
      </li>
    </ul>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue'

const tugasBaru = ref('')
const filter = ref('semua')

const daftarTugas = ref([
  { teks: 'Sarapan pagi', selesai: false },
  { teks: 'Mengerjakan tugas kuliah', selesai: false },
  { teks: 'Tidur siang', selesai: true }
])

const tambahTugas = () => {
  if (tugasBaru.value.trim() === '') return
  daftarTugas.value.push({ teks: tugasBaru.value.trim(), selesai: false })
  tugasBaru.value = ''
}

const hapusTugas = (index) => {
  daftarTugas.value.splice(index, 1)
}

const daftarTugasTersaring = computed(() => {
  if (filter.value === 'belum') {
    return daftarTugas.value.filter(t => !t.selesai)
  } else if (filter.value === 'selesai') {
    return daftarTugas.value.filter(t => t.selesai)
  } else {
    return daftarTugas.value
  }
})
</script>

<style scoped>
.wadah {
  max-width: 600px;
  margin: 40px auto;
  background: #f9f9f9;
  border-radius: 18px;
  padding: 28px;
  box-shadow: 0 6px 16px rgba(0, 0, 0, 0.12);
  font-family: 'Segoe UI', sans-serif;
}

h1 {
  text-align: center;
  color: #34495e;
  margin-bottom: 20px;
}

.formulir {
  display: flex;
  gap: 12px;
  margin-bottom: 20px;
}

input[type="text"] {
  flex: 1;
  padding: 10px;
  font-size: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
}

button {
  background-color: #3498db;
  color: white;
  border: none;
  padding: 10px 16px;
  border-radius: 8px;
  cursor: pointer;
  transition: background-color 0.2s;
}

button:hover {
  background-color: #2980b9;
}

.filter-dropdown {
  display: flex;
  align-items: center;
  justify-content: center;
  margin-bottom: 20px;
  gap: 12px;
  font-size: 16px;
  color: #2c3e50;
}

select {
  padding: 8px 12px;
  font-size: 16px;
  border-radius: 8px;
  border: 1px solid #ccc;
  cursor: pointer;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  display: flex;
  align-items: center;
  background: white;
  margin-bottom: 10px;
  padding: 12px;
  border-radius: 10px;
  transition: transform 0.2s;
}

li:hover {
  transform: scale(1.01);
}

li.selesai span {
  text-decoration: line-through;
  color: gray;
}

li span {
  flex: 1;
  margin-left: 12px;
  font-size: 16px;
}

.hapus {
  background: transparent;
  color: red;
  font-size: 20px;
  border: none;
  cursor: pointer;
}

.kosong {
  text-align: center;
  color: #888;
  font-style: italic;
  margin-top: 20px;
}

/* Animasi */
.fade-enter-active,
.fade-leave-active {
  transition: all 0.4s ease;
}
.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(10px);
}
</style>
