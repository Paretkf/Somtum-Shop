<template lang="html">
  <div class="container-fluid">
    <div id="header">
      <div class="container">
        <div class="columns" >
          <div class="" id="header-item">
            <div class="column">
             <button @click="add()" class="is-success button"> ADD+ </button>
             <button @click="bill()" class="is-info button"> ออกบิล </button>
             <button class="is-danger button disabled" disabled="">ราคารวม {{total_price}} </button>
            </div>
          </div>
        </div>
      </div>
    </div>
    <br>
    <div class="container">
      <div id="wrapper">
        <div id="columns">
          <div class="pin" v-for="(show, index) in data" :key="index">
            <div>
                <button @click="stock(show)" class="is-warning is-outlined button is-small" style="float:right"> +Stock </button>
                <img :src="show.img" @click="store.dispatch('listPlayer', show)"/>
                <center> {{show.name}} <b class="card"> {{show.price}}บาท </b> </center>
                <center><b> เหลืออยู่ {{show.amout}} จาน</b> </center>
                <div v-if="show.amout != 0">
                  <center> <button class="button is-small is-success is-outlined" @click="purches(show)">สั่งซื้อ</button> </center>
                </div>
            </div>
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
      newData: [],
      cart: []
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
            price: parseInt(result.value[1]),
            amout: parseInt(result.value[2]),
            img: result.value[3]
          })
        }
      })
    },
    purches (data) {
      let newvalue = data.amout - 1
      dataRef.child(data['.key'] + '/amout').set(newvalue)
      this.cart.push(data)
    },
    bill () {
      console.log(this.cart)
      var bill = ''
      for (var i in this.cart) {
        bill += this.cart[i].name + ' '
        bill += this.cart[i].price + ' '
        bill += 'บาท | '
      }
      bill += 'รวม ' + this.total_price + 'บาท'
      this.$swal({
        title: 'ร้านร้านป้าตุ๊',
        text: bill,
        width: 600,
        padding: '3em',
        background: '#fff url(/images/trees.png)',
        backdrop: `
          rgba(0,0,123,0.4)
          url("https://media.giphy.com/media/sIIhZliB2McAo/giphy.gif")
          center left
          no-repeat
        `
      })
      this.cart = []
    },
    stock (data) {
      this.$swal({
        title: 'จำนวนสินค้าใน Stock ที่ค้องการเพิ่ม',
        input: 'text',
        showCancelButton: true,
        confirmButtonText: 'OK'
      }).then((result) => {
        if (result.value) {
          let newvalue = data.amout + parseInt(result.value)
          dataRef.child(data['.key'] + '/amout').set(newvalue)
        }
      })
    }
  },
  computed: {
    total_price () {
      return this.cart.reduce((sumPrice, cart) => sumPrice + cart.price, 0)
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
  #header-item {
   margin-top: 27px;
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
    height: 110px;
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
