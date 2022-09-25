<template lang="html">
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.2.1/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-iYQeCzEYFbKjA/T2uDLTpkwGzCiq6soy8tYaI1GyVh/UjpbCx/TYkiZhlZB6+fzT" crossorigin="anonymous">
<div class="">
  <h1 class="text-center">TUGAS MEMBUAT CRUD PROYEK 3</h1>
  <form @submit.prevent="save">
    <input type="hidden" v-model="form.id" name="" >
    <input type="text" class="form-control" placeholder="Input nama" v-model="form.name" name="" ><br>
    <button type="submit" v-show="!updateSubmit" name="button" class="btn btn-primary">Save</button>
    <button type="button" v-show="updateSubmit" v-on:click="update(form)" name="button" class="btn btn-primary">Update</button>
  </form>

<br><h4 class="text-center">LIST USERS</h4></div>

<ul v-for="user in users" :key="user.id">
  <table class="table">
  <thead>
    <tr>
      <th scope="col">ID</th>
      <th scope="col">Name</th>
      <th scope="col">Action</th>
    </tr>
  </thead>
  <tbody>
    <tr>
      <td>
        <li>{{ user.id }} <br>
        </li>
      </td>
      <td>
        <li>{{ user.name }} <br>
        </li>
      </td>
      <td>
        <li><button type="button" v-on:click="edit(user)" name="button" class="btn btn-success">Edit</button> ||
        <button type="button" v-on:click="hapus(user)" name="button" class="btn btn-danger">Delete</button>
        </li>
      </td>
    </tr>
  </tbody>
</table>
</ul>

</template>

<script>

import axios from 'axios'

export default {
  data(){
    return{
      form:{
        id: '',
        name:''
      },
      users: [],
      updateSubmit: false
    }
  },

  methods:{
    load(){
      axios.get('http://localhost:3000/users')
      .then((res)=>{
        this.users = res.data
      })
      .catch(()=>{
        alert('eror load data')
      })
    },

    save(){
      axios.post('http://localhost:3000/users', this.form)
      .then(()=>{
        this.load()
        this.form.name = ''
        alert('saving...')
      })
      .catch(()=>{
        alert('saving error')
      })
    },
    edit(user){
      this.updateSubmit = true
      this.form.id = user.id
      this.form.name = user.name
    },
    update(form){
      axios.put('http://localhost:3000/users/' + form.id , {
        name: this.form.name
      })
      .then(()=>{
        this.load()
        this.form.name = ''
        this.updateSubmit = false
        alert('updated...')
      })
      .catch(()=>{
        alert('error update')
      })
    },
    hapus(user){
      axios.delete('http://localhost:3000/users/' + user.id)
      .then(()=>{
        this.load()
        this.form.name = ''
        alert('deleted...')
      })
      .catch(()=>{
        alert('deleted error')
      })
    }
  },

  mounted() {
    this.load()
  }
}

</script>

<style lang="css" scoped>

</style>