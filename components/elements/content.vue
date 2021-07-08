<template>
  <div>
    <b-table striped hover :items="tabel"></b-table>
  </div>
</template>

<script>


  export default {
    modules: ['@nuxtjs/axios'],
    data() {
      return {
        tabel: []
      }
    },
    methods : {
      async getMahasiswa() {
        const URL = "http://localhost:8080/api/mahasiswa/";
        await this.$axios.$get(URL)
          .then(res => {
            console.log(res.data)
          })
      }
    },
    mounted() {
      this.getMahasiswa();
    }
  }
</script>