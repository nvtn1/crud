<template>
  <div>
    <v-simpple-table>
    <form class="mx-auto mt-8 mb-8 pl-4" @submit.prevent="save">
      <input v-model="form.id" type="hidden" name="" value="">
      <input v-model="form.name" type="text" name="" value="" />
      <button v-show="!updateSubmit" type="submit" name="add">Simpan</button>
      <button v-show="updateSubmit" type="button" name="update" @click="update(form)">Update</button>
    </form>
    <header class="ml-4">
      <h2 class="text-3xl font-bold">Member List</h2>
      <p class="border border-black w-[350px] mt-2"></p>
      <div class="flex font-medium w-[350px]">
      <h3>No</h3>
      <h3 class="ml-10">Nama</h3>
      <h3 class="ml-48">Fungsi</h3>
      </div>
      <p class="border border-black w-[350px]"></p>
    </header>
    <div v-for="(users, index) in user" :key="users.id" class="mt-2 pl-4 font-medium  w-[363px]">
      <p class="flex justify-between">
        {{index + 1}}
        {{ users.name }}
        <span>
        <button  type="edit" name="edit" @click="edit(users)">edit</button> ||
        <button type="delete" name="delete" @click="hapus(users)">delete</button>
      </span>
      </p>
    </div>
    </v-simpple-table>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  data() {
    return {
      form: {
        id:'',
        name: '',
      },
      user: [],
      updateSubmit : false
    }
  },
  mounted() {
    this.load()
  },
  methods: {
    load() {
      this.$axios
        .get('http://localhost:3004/user')
        .then((res) => {
          this.user = res.data
        })
        .catch(() => {
          alert('error load data')
        })
    },
    save() {
      this.$axios
        .post('http://localhost:3004/user', this.form)
        .then(() => {
          this.load()
          this.form.name = ''
          alert('Saved..!')
        })
        .catch(() => {
          alert('Saving Error ... ?!')
        })
    },
    edit(users){
      this.updateSubmit = true
      this.form.id = users.id
      this.form.name = users.name
    },
    update(form){
      this.$axios
        .put('http://localhost:3004/user/'+ form.id , {
          name: this.form.name
        })
        .then(() => {
          this.load()
          this.form.name = ''
          this.updateSubmit = false
          alert('Updated..!')
        })
        .catch(() => {
          alert('Saving Error ... ?!')
        })
    },
    hapus(users){
      this.$axios.delete('http://localhost:3004/user/' + users.id)
      .then(() => {
          this.load()
          this.form.name = ''
          alert('Deleted..!')
        })
        .catch(() => {
          alert('Deleting Error ... ?!')
        })
    }
  },
}
</script>


//<div class="flex p-4 mb-4 text-sm text-green-700 bg-green-100 rounded-lg dark:bg-green-200 dark:text-green-800" role="alert">
  <svg aria-hidden="true" class="flex-shrink-0 inline w-5 h-5 mr-3" fill="currentColor" viewBox="0 0 20 20" xmlns="http://www.w3.org/2000/svg"><path fill-rule="evenodd" d="M18 10a8 8 0 11-16 0 8 8 0 0116 0zm-7-4a1 1 0 11-2 0 1 1 0 012 0zM9 9a1 1 0 000 2v3a1 1 0 001 1h1a1 1 0 100-2v-3a1 1 0 00-1-1H9z" clip-rule="evenodd"></path></svg>
  <span class="sr-only">Info</span>
  <div>
    <span class="font-medium">Success alert!</span> Change a few things up and try submitting again.
  </div>
</div>