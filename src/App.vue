<template>
  <div class="container">
    <h1>Daftar Lagu Favorit</h1>

    <form class="form" @submit.prevent="tambahLagu">
      <input class="input" v-model="laguBaru" type="text" placeholder="Tambahkan lagu baru">
      <button class="button" type="submit">Tambah</button>
    </form>

    <ul class="lagu-list">
      <li v-for="(lagu, index) in laguList" :key="index" class="lagu-item">
        <input class="checkbox" type="checkbox" v-model="lagu.selesai">
        <span class="nama-lagu" :class="{ 'selesai': lagu.selesai }">{{ lagu.nama }}</span>
        <button class="button batalkan-button" @click="hapusLagu(index)">Hapus</button>
        <button class="button" @click="editLagu(index)">Edit</button>
      </li>
    </ul>

    <label class="filter-label">
      <input class="filter-checkbox" type="checkbox" v-model="filterSelesai"> Tampilkan yang Belum Selesai
    </label>

    <table v-if="editedIndex > -1">
      <tr>
        <td><input v-model="editedNama" class="input" type="text"></td>
        <td><button class="button" @click="simpanEdit">Simpan</button></td>
        <td><button class="button batalkan-button" @click="batalEdit">Batal</button></td>
      </tr>
    </table>
  </div>
</template>

<script>
export default {
  data() {
    return {
      laguBaru: '',
      laguList: [],
      filterSelesai: false,
      editedIndex: -1,
      editedNama: ''
    };
  },
  methods: {
    tambahLagu() {
      if (this.laguBaru.trim() !== '') {
        this.laguList.push({ nama: this.laguBaru, selesai: false });
        this.laguBaru = '';
      }
    },
    hapusLagu(index) {
      this.laguList.splice(index, 1);
    },
    editLagu(index) {
      this.editedIndex = index;
      this.editedNama = this.laguList[index].nama;
    },
    simpanEdit() {
      if (this.editedIndex > -1 && this.editedNama.trim() !== '') {
        this.laguList[this.editedIndex].nama = this.editedNama;
        this.batalEdit();
      }
    },
    batalEdit() {
      this.editedIndex = -1;
      this.editedNama = '';
    }
  },
  computed: {
    laguTidakSelesai() {
      if (this.filterSelesai) {
        return this.laguList.filter(lagu => !lagu.selesai);
      } else {
        return this.laguList.filter(lagu => lagu.selesai);
      }
    }
  }
};
</script>

<style>
.container {
  border: 2px solid aqua;
  background-color: rgba(225, 225, 225, 0.4);
  background-size: cover;
  box-shadow: 10px 10px 20px rgb(53, 50, 50);
  margin: auto;
  width: 500px;
  padding: 25px;
  border-radius: 10px;
  border-color: rgb(237, 237, 237);
}

body {
  background-image: url('./assets/bck.png');
  background-size: cover;
}

.form {
  margin-bottom: 20px;
}

.input {
  padding: 8px;
  font-size: 16px;
  width: 70%;
  border: 1px solid #ccc;
  border-radius: 4px;
}

.button {
  padding: 8px 16px;
  font-size: 16px;
  background-color: #4CAF50;
  color: white;
  border: none;
  border-radius: 4px;
  cursor: pointer;
}

.button:hover {
  background-color: #45a049;
}

.lagu-list {
  list-style-type: none;
  padding: 0;
}

.lagu-item {
  display: flex;
  justify-content: space-between;
  align-items: center;
  background-color: rgba(44, 42, 42, 0.8);
  padding: 10px;
  border-radius: 4px;
  margin-bottom: 10px;
}

.checkbox {
  margin-right: 10px;
}

.nama-lagu {
  font-size: 16px;
}

.nama-lagu.selesai {
  text-decoration: line-through;
}

.batalkan-button {
  background-color: #f44336;
}

.filter-label {
  display: inline-block;
  margin-top: 20px;
  font-size: 16px;
}

.filter-checkbox {
  margin-right: 5px;
}
</style>
