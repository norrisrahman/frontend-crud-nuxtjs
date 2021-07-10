<template>
  <div>
    <!-- Tambah Data -->
    <div class="header-tab">
    <div class="search-form">
      <b-input-group size="sm">
            <b-form-input
              id="filter-input"
              v-model="filter"
              type="search"
              placeholder="Type to Search"
            ></b-form-input>

            <b-input-group-append>
              <b-button :disabled="!filter" @click="filter = ''" varian="primary">Clear</b-button>
            </b-input-group-append>
          </b-input-group>
    </div>
    <div class="addData-btn">
      <b-button variant="primary" v-b-modal.modal-prevent-closing>Tambah Data</b-button>
    </div>
    </div>
    <b-modal id="modal-prevent-closing" ref="modal" title="Masukkan Data Baru" @show="resetModal" @hidden="resetModal" @ok="handleOk">
      <form ref="form" @submit.stop.prevent="inputData">
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

    <!-- Update Data -->

    <!-- Tabel Data -->
    <b-table :filter="filter" striped hover :items="users" :fields="fields" show-empty>
        <template v-slot:cell(actions)="row" >
        <b-button variant="primary" @click="$bvModal.show('modal-prevent-closing-2'), dataID(row)" ><font-awesome-icon :icon="['fas', 'pencil-alt']"/></b-button>
        <b-button  variant="danger" @click="deletePost(row)"><font-awesome-icon :icon="['fas', 'times']"/></b-button>
      </template>
    </b-table>
      <b-modal id="modal-prevent-closing-2" ref="modal" title="Update Data" @show="resetModal" @hidden="resetModal" @ok="updateOk">
        <form ref="form" @submit.stop.prevent="updateData">
          <b-form-group id="input-group-2" label="Nama :" label-for="update-1" invalid-feedback="Name is required" :state="nameState">
                  <b-form-input
                  id="input-1"
                  v-model="form.nama"
                  placeholder="Masukkan Nama"
                  required
                  ></b-form-input>
              </b-form-group>

              <b-form-group id="input-group-2" label="NIM :" label-for="update-2" invalid-feedback="Name is required" :state="nameState">
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
import axios from 'axios'
import addData from '../../pages/inputData.vue'

  export default {
    data() {
      return {
        fields: ['nama', 'NIM', 'jurusan','actions'],
        users: [],
        modalShow: false,
        form: {
          nama: '',
          NIM: '',
          jurusan: '',
        },
        validation: [],
        show:true,
        nameState: null,
        idData : [],
        filter: null,
        filterOn: [],
      }
    },
    methods : {
      async getMahasiswa() {
        axios.get('http://localhost:8080/api/mahasiswa')
          .then(response => {
            this.users = response.data
          })
      },
      async deletePost(row) {
        console.log(row.item._id)
        await this.$axios.delete(`http://localhost:8080/api/mahasiswa/${row.item._id}`)
          .then(() => {
            this.users.splice(row.index, 1);
          })
      },
      checkFormValidity() {
        const valid = this.$refs.form.checkValidity()
        this.nameState = valid
        return valid
      },
      resetModal() {
        this.form.nama = ''
        this.form.NIM = ''
        this.form.jurusan = ''
        this.nameState = null
      },
      handleOk(bvModalEvt) {
        // Prevent modal from closing
        bvModalEvt.preventDefault()
        // Trigger submit handler
        this.inputData()

      },
      updateOk(bvModalEvt) {
        // Prevent modal from closing
        bvModalEvt.preventDefault()
        // Trigger submit handler
        this.updateData()
      },
      async inputData() {
        if (!this.checkFormValidity()) {
          return
        }

        await this.$axios.$post('http://localhost:8080/api/mahasiswa', {
            nama: this.form.nama,
            NIM: this.form.NIM,
            jurusan: this.form.jurusan
        })
        .then(()=> {
            this.$router.push({
                nama: 'form'
            })
        })
        .catch(error => {
            this.validation = console.response.error;
        })
        this.show = false
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing')
        })
        this.getMahasiswa();
      },
      async dataID(row){
        this.idData = row.item._id
      },
      async updateData() {
        if (!this.checkFormValidity()) {
          return
        }
        await this.$axios.$put(`http://localhost:8080/api/mahasiswa/${this.idData}`, {
            nama: this.form.nama,
            NIM: this.form.NIM,
            jurusan: this.form.jurusan
        })
        .then(()=> {
            this.$router.push({
                _id: 'form'
            })
        })
        .catch(error => {
            this.validation = console.response.data;
        })

        this.show = false
        this.$nextTick(() => {
          this.$bvModal.hide('modal-prevent-closing-2')
        })
        this.getMahasiswa();
      }
    },
    mounted() {
      this.getMahasiswa();

    },
    components :{
      'input-app': addData
    }
  }
</script>