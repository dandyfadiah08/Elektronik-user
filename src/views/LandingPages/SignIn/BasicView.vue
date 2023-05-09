<script setup>
import { onMounted } from "vue";

// example components
import DefaultNavbar from "@/examples/navbars/NavbarDefault.vue";
import Header from "@/examples/Header.vue";

//Vue Material Kit 2 components
import MaterialInput from "@/components/MaterialInput.vue";
import MaterialSwitch from "@/components/MaterialSwitch.vue";
import MaterialButton from "@/components/MaterialButton.vue";

// material-input
import setMaterialInput from "@/assets/js/material-input";
onMounted(() => {
  setMaterialInput();
  
});

</script>
<script>
import {Axios} from "axios"
import jwt_decode from "jwt-decode";
import Toastify from 'toastify-js'
import "toastify-js/src/toastify.css"
import { urlbase } from '../../Page/global.js'
export default {
    name: 'login',
    data () {
      return {
        username : '',
        password : '',
      }
    },
    mounted () {
      localStorage.clear();
    },
    methods:{
      testing(){
        var myHeaders = new Headers();
        myHeaders.append("Cookie", "ci_session=dkkl00dheeihlh0u48vetnopra2mhqen");
        var formdata = new FormData();
        formdata.append("username", username.value);
        formdata.append("password", password.value);

        var requestOptions = {
          method: 'POST',
           headers: myHeaders,
           body: formdata,
           redirect: 'follow'
};
      var url = urlbase.URL + "/api/LoginUser"
      fetch(url, requestOptions)
      .then(response => response.json())
     .then(result => {
      if (result.success == true) {
        var decode = jwt_decode(result.data.token)
        console.log(decode)
        document.cookie = "token="+result.data.token
        window.localStorage.setItem("jwt",result.data.token)
        window.localStorage.setItem("alamat_te",decode.data.alamat_te)
        window.localStorage.setItem("fullname",decode.data.fullname)
        window.localStorage.setItem("id_mitra",decode.data.id_mitra)
        window.localStorage.setItem("id_toko",decode.data.id_toko)
        window.localStorage.setItem("nama_te",decode.data.nama_te)
        window.localStorage.setItem("parent_id",decode.data.parent_id)
        window.localStorage.setItem("status",decode.data.status)
        window.localStorage.setItem("user",decode.data.user)
        window.localStorage.setItem("user_type",decode.data.user_type)
        window.localStorage.setItem("id_mitra",decode.data.id_mitra)
        window.localStorage.setItem("user_id",decode.data.user_id)
         window.location.href= urlbase.ROUTES + '/tradein'
      } else {
        if (username.value != '' && password.value != '') {
          Toastify({
            text:" "+ result.message + " !",
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
          console.log(localStorage.key("jwt"))
        } else {
          if (username.value == '') {
            Toastify({
            text:" Username tidak boleh kosong !",
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
          }
          if (password.value == '') {
            Toastify({
            text:" password tidak boleh kosong !",
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
          }
        }
      }
    })
     .catch(error => console.log('error', error));
 
      }
    }
    
}

</script>
<template>
  <Header>
    <div
      class="page-header align-items-start min-vh-100"
      :style="{
        backgroundImage:
          'url(https://cdn.eraspace.com/pub/media/mageplaza/blog/post/i/t/it3-5d9cd401097f364d63608c72.jpg)'
      }"
      loading="lazy"
    >
      <span class="mask bg-gradient-dark opacity-6"></span>
      <div class="container my-auto">
        <div class="row">
          <div class="col-lg-4 col-md-8 col-12 mx-auto">
            <div class="card z-index-0 fadeIn3 fadeInBottom">
              <div
                class="card-header p-0 position-relative mt-n4 mx-5 z-index-2"
              >
                <div
                  class="bg-gradient-info shadow-info border-radius-lg py-3 pe-1  text-center"
                >
                 <img src="../../../assets/img/Logo-Hitam.png" class="w-80 border-radius-md " alt="">

                </div>
              </div>
              <div class="card-body">
                
                  <MaterialInput
                    id="username"
                    autcomplate="false"
                    v-model="username"
                    class="input-group-outline my-3"
                    placeholder="Username"
                    type="text"
                  />
                    <MaterialInput
                    id="password"
                    v-model="password"
                    placeholder="password"
                    class="input-group-outline mb-3"
                    type="password"
                  />
                  <div class="text-center">
                    <MaterialButton
                      class="my-4 mb-2"
                      variant="gradient"
                      color="info"
                      fullWidth
                      @click="testing"
                      >Sign in</MaterialButton
                    >
                  </div>
                  <p class="mt-4 text-sm text-center">
                    © PT. ENB Mobile Care {{ new Date().getFullYear() }} - All Rights Reserved.
                  </p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  </Header>
</template>

