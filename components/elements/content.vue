<template>
  <div>
    <b-table striped hover :items="users" :fields="fields" show-empty></b-table>
  </div>
</template>

<script>
import axios from 'axios'

  export default {
    data() {
      return {
        fields: ['nama', 'NIM', 'jurusan'],
        users: []
      }
    },
    methods : {
      async getMahasiswa() {
        axios.get('http://localhost:8080/api/mahasiswa')
          .then(response => {
            this.users = response.data,
            console.log(response.data)
          })
      }
    },
    mounted() {
      this.getMahasiswa();
    }
  }
</script>