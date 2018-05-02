<template lang="html">
  <div class="container-fluid">
    <div id="header">
      <div class="container">
        <br>
        <a @click="add()">Add Item</a>
      </div>
    </div>
    <br>
    <div class="container">
      <div id="wrapper">
        <div id="columns">
          <div class="pin" v-for="(show, index) in data" :key="index">
            <img :src="show.img" @click="store.dispatch('listPlayer', show)"/>
            <p>
              <center><b> {{show.name}} </b> </center><br>
            </p>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>

import Firebase from 'firebase'
var config = {
  apiKey: 'AIzaSyDfQV8DRodRvq18kzKQpKKRyeI0CGJfFuo',
  authDomain: 'premidterm.firebaseapp.com',
  databaseURL: 'https://premidterm.firebaseio.com',
  projectId: 'premidterm',
  storageBucket: 'premidterm.appspot.com',
  messagingSenderId: '292350490774'
}
let app = Firebase.initializeApp(config)
let db = app.database()
let dataRef = db.ref('data')
export default {
  name: 'HelloWorld',
  firebase: {
    data: dataRef
  },
  data () {
    return {
      msg: 'Welcome to Your Vue.js App',
      newData: []
    }
  },
  methods: {
    add () {
      this.$swal.mixin({
        input: 'text',
        confirmButtonText: 'Next &rarr;',
        showCancelButton: true,
        progressSteps: ['1', '2', '3', '4']
      }).queue([
        {
          title: 'ชื่อสินค้า',
          text: 'ชื่อของสินค้าที่ต้องการเพิ่ม'
        },
        {
          title: 'ราคา',
          text: 'Chaining swal2 modals is easy'
        },
        {
          title: 'จำนวน',
          text: 'Chaining swal2 modals is easy'
        },
        {
          title: 'link รูปอาหาร',
          text: 'Chaining swal2 modals is easy'
        }
      ]).then((result) => {
        if (result.value) {
          this.$swal({
            title: 'All done!',
            text: 'เพิ่มอาหารใหม่สำเร็จ',
            type: 'success',
            confirmButtonText: 'น่ารัก'
          })
          dataRef.push({
            name: result.value[0],
            price: result.value[1],
            amout: result.value[2],
            img: result.value[3]
          })
        }
      })
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped>
  h1, h2 {
    font-weight: normal;
  }
  ul {
    list-style-type: none;
    padding: 0;
  }
  li {
    display: inline-block;
    margin: 0 10px;
  }
  a {
    color: #42b983;
  }
  .hello {
    margin-top: 20px;
  }
  #wrapper {
    width: 90%;
    max-width: 1100px;
    min-width: 800px;
    margin: 50px auto;
  }
  #columns {
      -webkit-column-count: 3;
      -webkit-column-gap: 10px;
      -webkit-column-fill: auto;
      -moz-column-count: 3;
      -moz-column-gap: 10px;
      -moz-column-fill: auto;
      column-count: 3;
      column-gap: 15px;
      column-fill: auto;
  }
  .pin {
      display: inline-block;
      background: #FEFEFE;
      border: 2px solid #FAFAFA;
      box-shadow: 0 1px 2px rgba(34, 25, 25, 0.4);
      margin: 0 2px 15px;
      -webkit-column-break-inside: avoid;
      -moz-column-break-inside: avoid;
      padding: 15px;
      padding-bottom: 5px;
      background: -webkit-linear-gradient(45deg, #FFF, #F9F9F9);
      opacity: 1;
      -webkit-transition: all .2s ease;
      -moz-transition: all .2s ease;
      -o-transition: all .2s ease;
      transition: all .2s ease;
  }
  .pin img {
    width: 100%;
    border-bottom: 1px solid #ccc;
    padding-bottom: 15px;
    margin-bottom: 5px;
    cursor: pointer;
  }
  #header {
    background-color: #3543;
    height: 100px;
  }
  .pin p {
    font: 12px/18px Arial, sans-serif;
    color: #333;
    margin: 0;
    cursor: pointer;
  }
  @media (min-width: 960px) {
    #columns {
      -webkit-column-count: 4;
      -moz-column-count: 4;
      column-count: 4;
    }
  }
  @media (min-width: 1100px) {
    #columns {
      -webkit-column-count: 5;
      -moz-column-count: 5;
      column-count: 5;
    }
  }
</style>
