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
export default {
    name: 'my-component',
    data () {
      return {
        username : '',
        password : '',
      }
    },
    methods:{
      testing(){
        console.log(username.value)
        console.log(password.value)
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
                    
                    v-model="username"
                    class="input-group-outline my-3"
                    :label="{ text: 'username', class: 'form-label' }"
                    type="text"
                  />
                    <MaterialInput
                    aria-valuetext=""
                    id="password"
                    v-model="password"
                    class="input-group-outline mb-3"
                    :label="{ text: 'Password', class: 'form-label' }"
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
      <!-- <footer class="footer position-absolute bottom-2 py-2 w-100">
        <div class="container">
          <div class="row align-items-center justify-content-lg-between">
            <div class="col-12 col-md-6 my-auto">
              <div
                class="copyright text-center text-sm text-white text-lg-start"
              >
                © {{ new Date().getFullYear() }}, made with
                <i class="fa fa-heart" aria-hidden="true"></i> by
                <a
                  href="https://www.creative-tim.com"
                  class="font-weight-bold text-white"
                  target="_blank"
                  >Creative Tim</a
                >
                for a better web.
              </div>
            </div>
            <div class="col-12 col-md-6">
              <ul
                class="nav nav-footer justify-content-center justify-content-lg-end"
              >
                <li class="nav-item">
                  <a
                    href="https://www.creative-tim.com"
                    class="nav-link text-white"
                    target="_blank"
                    >Creative Tim</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    href="https://www.creative-tim.com/presentation"
                    class="nav-link text-white"
                    target="_blank"
                    >About Us</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    href="https://www.creative-tim.com/blog"
                    class="nav-link text-white"
                    target="_blank"
                    >Blog</a
                  >
                </li>
                <li class="nav-item">
                  <a
                    href="https://www.creative-tim.com/license"
                    class="nav-link pe-0 text-white"
                    target="_blank"
                    >License</a
                  >
                </li>
              </ul>
            </div>
          </div>
        </div>
      </footer> -->
    </div>
  </Header>
</template>

