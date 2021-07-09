<template>
  <div>
    <b-button v-b-modal.modal-prevent-closing>Open Modal</b-button>

    <b-modal id="modal-prevent-closing" ref="modal" title="Masukkan Data Baru" @show="resetModal" @hidden="resetModal" @ok="handleOk">
      <form ref="form" @submit.stop.prevent="handleSubmit">
        <b-form-group id="input-group-2" label="Nama :" label-for="input-1" invalid-feedback="Name is required" :state="nameState">
                <b-form-input
                id="input-1"
                v-model="form.nama"
                placeholder="Masukkan Nama"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="NIM :" label-for="input-2" invalid-feedback="Name is required" :state="nameState">
                <b-form-input
                id="input-2"
                v-model="form.NIM"
                placeholder="Masukkan NIM"
                required
                ></b-form-input>
            </b-form-group>

            <b-form-group id="input-group-2" label="Jurusan :" label-for="input-3" invalid-feedback="Name is required" :state="nameState">
                <b-form-input
                id="input-3"
                v-model="form.jurusan"
                placeholder="Masukkan Jurusan"
                required
                ></b-form-input>
            </b-form-group>

      </form>
    </b-modal>
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
        nameState: null,
        submittedNames: []
      }
    },
    methods: {
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.nameState = valid
        return valid
      },
      resetModal() {
        this.name = ''
        this.nameState = null
      },
      handleOk(bvModalEvt) {
        // Prevent modal from closing
        bvModalEvt.preventDefault()
        // Trigger submit handler
        this.handleSubmit()
      },
      async handleSubmit() {
        // event.preventDefault()
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
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
        this.getMahasiswa()
      },
    },
    mounted() {
      this.handleSubmit();
    },
  }
</script>