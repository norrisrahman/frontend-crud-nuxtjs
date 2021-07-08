<template>
  <div id="wrapper">
    <div id="content-wrapper">
      <div class="content-title"> <h1>Input Data Mahasiswa</h1></div>
          <b-alert variant="success" v-model="alertscs" dismissible>Penambahan Data Berhasil</b-alert>

         <div>
            <b-form @submit="onSubmit" v-if="show">
            <b-form-group id="input-group-2" label="Nama :" label-for="input-2">
                <b-form-input
                id="input-1"
                v-model="form.nama"
                placeholder="Masukkan Nama"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="NIM :" label-for="input-2">
                <b-form-input
                id="input-2"
                v-model="form.NIM"
                placeholder="Masukkan NIM"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Jurusan :" label-for="input-2">
                <b-form-input
                id="input-3"
                v-model="form.jurusan"
                placeholder="Masukkan Jurusan"
                required
                ></b-form-input>
            </b-form-group>

            <b-button type="submit" variant="primary">Submit</b-button>
            <!-- <b-button type="reset" variant="danger">Reset</b-button> -->
            </b-form>
            <!-- <b-card class="mt-3" header="Form Data Result"> -->
            <!-- <pre class="m-0">{{ form }}</pre>
            </b-card> -->
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    data() {
      return {
        form: {
          nama: '',
          NIM: '',
          jurusan: '',
        },
        validation: [],
        show:true,
        alertscs: false
      }
    },
    methods: {
    async onSubmit(event) {
        event.preventDefault()
        // alert(JSON.stringify(this.form))

        await this.$axios.$post('http://localhost:8080/api/mahasiswa', {
            nama: this.form.nama,
            NIM: this.form.NIM,
            jurusan: this.form.jurusan
        })
        .then(()=> {
            this.$router.push({
                nama: 'nama'
            })
        })
        .catch(error => {
            this.validation = console.response.data;
        })
        this.form.nama = ''
        this.form.NIM = ''
        this.form.jurusan = ''
        this.alertscs = true
        // Trick to reset/clear native browser form validation state
        this.show = false
        this.$nextTick(() => {
          this.show = true
        })
      },
    }
  }
</script>