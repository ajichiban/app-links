<template>
  <div id="app" class="container">
    <h1 class="text-center">Vue and Firebase</h1>
    <div class="row">

      <div class="col-md-6"> <!-- Add links -->
        <div class="card p-3">
          <div class="card-header">
            <h3>Add Links</h3>
          </div>
          <div class="card-bod">
            <form @submit.prevent="addLink">

              <div class="form-group pb-2 pt-3">
                <input 
                  v-model="newLink.title"
                  type="text"
                  class="form-control" 
                  placeholder="title">
              </div>

              <div class="form-group pb-2">
                <input 
                  v-model="newLink.author"
                  type="text"
                  class="form-control" 
                  placeholder="author">
              </div>

              <div class="form-group">
                <input 
                  v-model="newLink.url"
                  type="text"
                  class="form-control" 
                  placeholder="url">
              </div>
              <button class="btn btn-success">add link</button>
            </form>
          </div>
        </div>
      </div>

      <div class="col-md-6"> <!-- List links -->
        <div class="card">
          <div class="card-header">
            <h3>Links List</h3>
          </div>
          <div class="card-body">
            <table class="table table-striped">
              <thead>
                <tr>
                  <th>Title</th>
                  <th>Author</th>
                  <th>Options</th>
                </tr>
              </thead>

              <tbody>
                <tr v-for="link in links">
                  <template v-if="link.edit == false">
                    <td ><a :href="link.url">{{ link.title }}</a></td>
                    <td>{{ link.author }}</td>
                    <td>
                      <button class="btn btn-warning"
                      @click="edLink(link)" >edit</button>
                      <button 
                        class="btn btn-danger"
                        @click="removeLink(link)"
                        >&times;</button>

                    </td>
                  </template>
                  <template v-else> <!-- Update links -->
                      <td>
                          <input 
                            type="text"
                            class="form-control"
                            v-model="link.title"
                            >
                            
                      </td>
                      <td>
                        <input 
                            type="text"
                            class="form-control"
                            v-model="link.author"
                            >
                      </td>
                      <td>
                        <input 
                              type="text"
                              class="form-control"
                              v-model="link.url"
                              >
                      </td>
                      <td>
                        <button class="btn btn-primary"
                        @click="updateLink(link)" >up</button>
                        <button class="btn btn-danger"
                        @click="cancelUp(link)" >x</button>
                      </td>
                  </template>
                </tr>

              </tbody>

            </table>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import firebase from 'firebase'

// inicializando configuracion de firebase

let config = {
    apiKey: "AIzaSyBS6hZTARXjkcnMbyFKKNaJwP0SMYQgKsA",
    authDomain: "app-links-c9509.firebaseapp.com",
    databaseURL: "https://app-links-c9509.firebaseio.com",
    projectId: "app-links-c9509",
    storageBucket: "app-links-c9509.appspot.com",
    messagingSenderId: "718288390543"
  }
 
const app = firebase.initializeApp(config)
const db = app.database()

// reference to collection links 
const linksRef = db.ref('links')

export default {
  firebase:{
    links: linksRef
  },
  data(){
    return{
      newLink:{
        title: null,
        author: '',
        url: '',
        edit : false
      },
      editLink:{
        title: "",
        author:"",
        url: "",
        edit: false
      }
    }
  },
  methods:{
    addLink(){
      linksRef.push(this.newLink)
      this.newLink.title = " "
      this.newLink.author = " "
      this.newLink.url = " "
      
    },
    removeLink(link){
      linksRef.child(link['.key']).remove()
     
    },
    edLink(link){
      link.edit = true
      console.log(link.edit);
    },
    updateLink(link){
      link.edit = false
      linksRef.child(link['.key']).update({
        title : link.title,
        author: link.author,
        url: link.url
      }).then(()=>{
        console.log('link updated !');
        
      })
      
    },
    cancelUp(link){
      link.edit = false
    }
  }
  
}
</script>

<style>

</style>
