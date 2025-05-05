<template>
  <div class="wadah">
    <h1> Todo List </h1>

    <form @submit.prevent="tambahKegiatan" class="formulir">
      <div class="input-grup">
        <input v-model="kegiatanBaru" placeholder="Tulis aktivitas baru..." />
        <button type="submit">➕ Tambah</button>
      </div>
    </form>

    <div class="filter">
      <select id="filter" v-model="filter">
        <option value="semua">📄 Semua</option>
        <option value="belum">⏳ Belum Selesai</option>
        <option value="selesai">✅ Selesai</option>
      </select>
    </div>

    <transition-group name="fade" tag="ul" class="daftar">
      <li v-for="(kegiatan, index) in kegiatanTerseleksi" :key="'kegiatan-' + index" :class="{ selesai: kegiatan.selesai }">
        <input type="checkbox" v-model="kegiatan.selesai" />
        <span>{{ kegiatan.teks }}</span>
        <button @click="hapusKegiatan(index)">🗑️</button>
      </li>
    </transition-group>
  </div>
</template>

<script setup>
import { ref, computed } from 'vue';

const kegiatanBaru = ref('');
const filter = ref('semua');

const daftarKegiatan = ref([
  { teks: 'Sarapan pagi', selesai: false },
  { teks: 'Belajar Vue 3', selesai: true },
  { teks: 'Istirahat siang', selesai: false },
]);

const tambahKegiatan = () => {
  if (!kegiatanBaru.value.trim()) return;
  daftarKegiatan.value.push({
    teks: kegiatanBaru.value.trim(),
    selesai: false,
  });
  kegiatanBaru.value = '';
};

const hapusKegiatan = (index) => {
  daftarKegiatan.value.splice(index, 1);
};

const kegiatanTerseleksi = computed(() => {
  if (filter.value === 'belum') {
    return daftarKegiatan.value.filter((k) => !k.selesai);
  } else if (filter.value === 'selesai') {
    return daftarKegiatan.value.filter((k) => k.selesai);
  }
  return daftarKegiatan.value;
});
</script>

<style scoped>
@keyframes backgroundShift {
  0% {
    background-position: 0% 50%;
  }
  50% {
    background-position: 100% 50%;
  }
  100% {
    background-position: 0% 50%;
  }
}

body {
  background: linear-gradient(135deg, #502f4c, #70587c, #c8b8db);
  background-size: 300% 300%;
  animation: backgroundShift 12s ease infinite;
  font-family: 'Poppins', sans-serif;
  margin: 0;
  padding: 0;
  color: #f9f4f5;
  transition: background 0.5s ease;
}

.wadah {
  max-width: 600px;
  margin: 40px auto;
  padding: 30px;
  border-radius: 20px;
  background: rgba(0, 0, 0, 0.4);
  backdrop-filter: blur(18px);
  box-shadow: 0 0 20px rgba(80, 47, 76, 0.7);
  border: 1px solid #c8b8db;
  text-align: center;
  transition: transform 0.3s ease;
}

.wadah:hover {
  transform: scale(1.01);
}

h1 {
  font-size: 28px;
  margin-bottom: 24px;
  background: linear-gradient(to right, #f9f4f5, #c8b8db);
  -webkit-background-clip: text;
  -webkit-text-fill-color: transparent;
}

.formulir {
  margin-bottom: 20px;
}

.input-grup {
  display: flex;
  gap: 12px;
  justify-content: center;
  flex-wrap: wrap;
}

.input-grup input[type='text'] {
  flex: 1 1 300px;
  min-width: 200px;
  padding: 12px;
  border-radius: 10px;
  border: 1px solid #70587c;
  font-size: 16px;
  background-color: rgba(200, 184, 219, 0.15);
  color: #f9f4f5;
  transition: all 0.3s ease;
}

.input-grup input[type='text']::placeholder {
  color: #d2c6dd;
}

.input-grup input[type='text']:focus {
  box-shadow: 0 0 8px #c8b8db;
  outline: none;
  transform: scale(1.02);
}

.input-grup button {
  flex-shrink: 0;
  min-width: 100px;
  background: linear-gradient(to right, #70587c, #c8b8db);
  color: #000;
  font-weight: bold;
  border: none;
  border-radius: 10px;
  padding: 12px 18px;
  cursor: pointer;
  transition: all 0.3s ease;
}

.input-grup button:hover {
  background: linear-gradient(to right, #c8b8db, #70587c);
  transform: scale(1.05);
}

.input-grup button:active {
  transform: scale(0.95);
  box-shadow: 0 0 10px #c8b8db;
}

.filter {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 12px;
  margin-bottom: 20px;
  flex-wrap: wrap;
}

.filter select {
  background-color: #70587c;
  color: #fff;
  border: 1px solid #502f4c;
  border-radius: 8px;
  padding: 10px 14px;
  font-weight: bold;
  transition: all 0.3s ease;
}

.filter select:focus {
  box-shadow: 0 0 8px #c8b8db;
  outline: none;
  transform: scale(1.02);
}

.daftar {
  display: flex;
  flex-direction: column;
  align-items: center;
  padding: 0;
}

.daftar li {
  background: rgba(0, 0, 0, 0.3);
  border: 1px solid #70587c;
  padding: 14px 16px;
  margin-bottom: 12px;
  border-radius: 12px;
  box-shadow: 0 4px 12px rgba(0, 0, 0, 0.5);
  display: flex;
  align-items: center;
  gap: 10px;
  color: #f9f4f5;
  width: 90%;
  max-width: 450px;
  justify-content: space-between;
  transition: all 0.3s ease;
}

.daftar li:hover {
  transform: scale(1.02);
  box-shadow: 0 6px 16px rgba(255, 255, 255, 0.2);
}

.daftar li input[type='checkbox'] {
  transform: scale(1.2);
  cursor: pointer;
  transition: transform 0.2s;
}

.daftar li input[type='checkbox']:hover {
  transform: scale(1.4);
}

.daftar li span {
  flex: 1;
  text-align: left;
}

.daftar li.selesai span {
  text-decoration: line-through;
  color: #aaa;
}

.daftar li button {
  color: #ff6b6b;
  background: none;
  border: none;
  font-size: 18px;
  cursor: pointer;
  transition: transform 0.2s ease, color 0.2s;
}

.daftar li button:hover {
  transform: scale(1.2);
  color: #ff4d4d;
}

.fade-enter-active,
.fade-leave-active {
  transition: all 0.5s ease;
}

.fade-enter-from,
.fade-leave-to {
  opacity: 0;
  transform: translateY(20px);
}
</style>
