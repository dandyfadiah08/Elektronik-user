<script setup>

import { KeepAlive, onMounted, onUnmounted } from "vue";
import NavbarDefault from "../../examples/navbars/NavbarDefault.vue";
import Header from "../../examples/Header.vue";
import Notfound from "../Page/404.vue"
import vueMkHeader from "@/assets/img/banner.jpg";
import { urlbase } from '../Page/global.js'
const body = document.getElementsByTagName("body")[0];
onMounted(() => {
  body.classList.add("tradein-page");
  body.classList.add("bg-gray-200");
});
onUnmounted(() => {
  body.classList.remove("tradein-page");
  body.classList.remove("bg-gray-200");
});
 const msg = 'Hello!'
const local = window.localStorage.getItem("jwt");
const log = ()=>{
  var myHeaders = new Headers();
myHeaders.append("Authorization",  local);
myHeaders.append("Cookie", "ci_session=qgkfai8g4fue32fc5loa740qhh2bd7a6");
myHeaders.append("Content-Type", "application/json");
myHeaders.append("Access-Control-Allow-Methods", "POST");
myHeaders.append("Access-Control-Allow-Origin", "*");
  var formdata = new FormData();
var requestOptions = {
  method: 'POST',
  headers: myHeaders,
  body: formdata,
  redirect: 'follow'
};
var url = urlbase.URL + "/api/createkoderegister"
fetch(url, requestOptions)
  .then(response => response.text())
  .then(result => console.log(result))
  .catch(error => console.log('error', error));

}


</script>
<script>
import {Axios} from "axios"
import Toastify from 'toastify-js'
import "toastify-js/src/toastify.css"
import { urlbase } from '../Page/global.js'
export default {
    name: 'tradein',
    data () {
      return {
        username : '',
        password : '',
      }
    },
    methods:{
      creatCode(){
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        var cookielocal = this.getCookie("token")
        // console.log(cookielocal)
        if (cookielocal == local) {
          var formdata = new FormData();
       var requestOptions = {
          method: 'POST',
          headers: {
            'Content-Type': 'application/json',
           'Authorization': 'Bearer '+local
          },
          body: formdata,
          redirect: 'follow'
       };
       var url = urlbase.URL + "/api/ProdukMenu/tambahregister/api/Creteregis"
       fetch(url, requestOptions)
        .then(response => response.json())
         .then(result => {
          window.localStorage.setItem("kode_register", result.data.kode_register)
          window.location.href= urlbase.ROUTES + '/pameran'
         }
         )
         .catch(error => console.log('error', error));
        } else {
          Toastify({
            text:"sorry your token invalid",
            duration: 3000,
            newWindow: true,
            close: true,
            gravity: "top", // `top` or `bottom`
            position: "left", // `left`, `center` or `right`
            stopOnFocus: true, // Prevents dismissing of toast on hover
            style: {
            background: "linear-gradient(to right, rgba(227,53,71,1) 34%, rgba(233,34,24,1) 100%)",
            },
          }).showToast();
          window.location.href= urlbase.ROUTES + '/'
        }

       
       },
      getCookie(name) {
         const value = `; ${document.cookie}`;
         const parts = value.split(`; ${name}=`);
        if (parts.length === 2) return parts.pop().split(';').shift();
      }
    }
    
}
</script>
<template  >
  <Notfound v-if="local == null"></Notfound>
  <div class="container position-sticky z-index-sticky top-0" v-if="local !== null">
    <div class="row">
      <div class="col-12">
        <NavbarDefault :sticky="true" />
      </div>
    </div>
  </div>
  <Header v-if="local !== null">
    <div
      class="page-header min-vh-75"
      :style="`background-image: url(${vueMkHeader})`"
      loading="lazy"
    >
      <div class="container">
        <div class="row">
          <div class="col-lg-7 text-center mx-auto position-relative">
            <h1
              class="text-white pt-3 mt-n5 me-2"
              :style="{ display: 'inline-block ' }"
            >
              SELEMAT DATANG
            </h1>
            <p class="lead text-white px-5 mt-3" :style="{ fontWeight: '500' }">
              Di Trade In Elektronik
            </p>
          </div>
        </div>
      </div>
    </div>
  </Header>

  <div class="card card-body blur shadow-blur mx-3 mx-md-4 mt-n9" v-if="local !== null">
   
    <div class="container" >
      <div class="row">
        <div class="d-flex flex-column w-100 text-center p-5 mb-2">
          <h3>Mulai Tradein in</h3>
          <div class="d-flex justify-content-center mt-3 flex-wrap">
            <button type="submit" @click="creatCode" class="btn bg-success text-white">
              mulai
            </button>
          
          </div>
        </div>
      </div>
    </div>
    
  </div>
</template>
