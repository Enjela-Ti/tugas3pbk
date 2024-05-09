<template>
    <div>
      <div class="background">
        <img src="https://wallpaperwaifu.com/wp-content/uploads/2023/11/texas-exusiai-arknights-police-chase-thumb.jpg" />
      </div>
  
      <div class="filter-icon" @click="toggleFilter">
        <button class="filter-button">
          <svg xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="30" height="30" viewBox="0 0 256 256" xml:space="preserve">
            <g>
              <path d="M85,25.887H33.91c-2.761,0-5-2.239-5-5s2.239-5,5-5H85c2.762,0,5,2.239,5,5S87.762,25.887,85,25.887z"/>
              <path d="M72.634,41.962H33.91c-2.761,0-5-2.239-5-5s2.239-5,5-5h38.724c2.762,0,5,2.239,5,5S75.396,41.962,72.634,41.962z"/>
              <path d="M60.269,58.038H33.91c-2.761,0-5-2.238-5-5s2.239-5,5-5h26.358c2.762,0,5,2.238,5,5S63.03,58.038,60.269,58.038z"/>
              <path d="M47.902,74.113H33.91c-2.761,0-5-2.238-5-5s2.239-5,5-5h13.992c2.762,0,5,2.238,5,5S50.664,74.113,47.902,74.113z"/>
              <path d="M13.219,74.113c-2.761,0-5-2.238-5-5V20.887c0-2.761,2.239-5,5-5s5,2.239,5,5v48.227C18.219,71.875,15.98,74.113,13.219,74.113z"/>
              <path d="M21.438,34.105c-1.28,0-2.559-0.488-3.536-1.464l-4.683-4.683l-4.683,4.683c-1.953,1.952-5.118,1.952-7.071,0
                c-1.953-1.953-1.953-5.119,0-7.071l8.219-8.219c1.953-1.952,5.118-1.952,7.071,0l8.219,8.219c1.953,1.953,1.953,5.119,0,7.071
                C23.997,33.617,22.717,34.105,21.438,34.105z"/>
            </g>
          </svg>
          <!-- Tambahkan logika untuk menampilkan ikon filter -->
          <svg v-if="filterActive" xmlns="http://www.w3.org/2000/svg" xmlns:xlink="http://www.w3.org/1999/xlink" version="1.1" width="30" height="30" viewBox="0 0 256 256" xml:space="preserve" fill="black">
            <g>
              <path d="M89.09,65.887c-1.381,0-2.655-0.759-3.302-1.971L63.29,15.326c-0.802-1.61,0.307-3.512,2.043-3.512H153.68c1.735,0,2.845,1.902,2.043,3.512L92.393,63.916c-0.648,1.212-1.922,1.971-3.303,1.971Z"/>
              <path d="M89.09,67.887c-1.922,0-3.5-1.578-3.5-3.5V12.326c0-1.922,1.578-3.5,3.5-3.5s3.5,1.578,3.5,3.5v52.061C92.59,66.308,90.012,67.887,89.09,67.887Z"/>
            </g>
          </svg>
        </button>
      </div>
  
      <div class="add-activity">
        <input type="text" v-model="newActivity" placeholder="Tambah kegiatan" @keyup.enter="tambahKegiatan">
        <button @click="tambahKegiatan">Tambah</button>
      </div>
  
      <table>
        <thead>
          <tr>
            <th>Kegiatan</th>
            <th>Status</th>
            <th>Opsional</th>
          </tr>
        </thead>
        <tbody>
            <tr v-for="(kegiatan, index) in kegiatanList" :key="index">
          <td :class="{ 'completed': kegiatan.status === 'Selesai' }">
            <span v-if="editedIndex !== index">{{ kegiatan.nama }}</span>
            <input v-else v-model="editedActivity" @keyup.enter="saveKegiatan" @keyup.esc="cancelEdit" autofocus>
          </td>
          <td>
            <select v-if="editedIndex !== index" v-model="kegiatan.status" @change="saveKegiatan">
              <option value="Belum Selesai">Belum Selesai</option>
              <option value="Selesai">Selesai</option>
            </select>
            <span v-else>{{ kegiatan.status }}</span>
          </td>
          <td>
            <button @click="editKegiatan(index)" v-if="editedIndex !== index">Edit</button>
            <button @click="saveKegiatan" v-else>Simpan</button>
            <button @click="cancelEdit" v-if="editedIndex === index">Batal</button>
            <button @click="hapus(index)">Hapus</button>
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </template>
  
  <script>
  export default {
    data() {
      return {
        kegiatanList: [],
        editedIndex: null,
        editedActivity: '',
        newActivity: '', 
        filterActive: false 
      };
    },
    methods: {
      toggleFilter() {
        this.filterActive = !this.filterActive;
      },
      editKegiatan(index) {
        this.editedIndex = index;
        this.editedActivity = this.kegiatanList[index].nama;
      },
      saveKegiatan() {
        if (this.editedIndex !== null && this.editedActivity.trim() !== '') {
          this.kegiatanList[this.editedIndex].nama = this.editedActivity.trim();
          this.editedIndex = null;
          this.editedActivity = '';
        }
      },
      cancelEdit() {
        this.editedIndex = null;
        this.editedActivity = '';
      },
      hapus(index) {
        this.kegiatanList.splice(index, 1);
      },
      tambahKegiatan() {
        if (this.newActivity.trim() !== '') {
          this.kegiatanList.push({ nama: this.newActivity.trim(), completed: false });
          this.newActivity = '';
        }
      },
      toggleStatus(index) {
        this.kegiatanList[index].status = this.kegiatanList[index].completed ? 'Selesai' : 'Belum Selesai';
      }
    }
  };
  </script>
  
  <style scoped>
  .completed {
    text-decoration: line-through;
  }
  
  .background img {
    position: fixed;
    top: 0;
    left: 0;
    min-height: 90%;
    min-width: 1500px;
    width: 100%;
    height: 100%;
    overflow: hidden;
    z-index: -2;
    object-fit: cover;
    -webkit-background-size: cover;
    -moz-background-size: cover;
    -o-background-size: cover;
    background-size: cover;
    filter: brightness(0.8);
  }
  
  .table-container {
    display: inline;
    position: relative;
    width: 300%;
    z-index: 1;
    overflow: auto;
    margin-left: -20%;
  }
  
  table {
    z-index: 1;
    border-collapse: collapse;
    width: 100%;
    display: table-column;
    position: fixed;
    margin: 0px -10%;
    top: 50%;
  }
  
  th,
  td {
    border: 1px solid black;
    padding: 8px;
    text-align: left;
    color: wheat;
    text-shadow: 0 0 10px black;
    background-color: transparent;
    backdrop-filter: blur(5px);
  }
  
  th {
    color: black;
    text-shadow: none;
    background-color: gray;
  }
  
  .filter-icon {
    position: fixed;
    top: 43%;
    left: 60%;
    cursor: pointer;
    z-index: 9;
  }
  
  .add-activity {
    position: fixed;
    top: 45%;
    left: 38%;
  }
  
  .add-activity input,
  .add-activity button {
    margin-right: 10px;
    background-color: transparent;
    backdrop-filter: blur(10px);
    z-index: 10;
    position: relative;
  }
  
  .completed {
    text-decoration: line-through;
  }
  </style>
  