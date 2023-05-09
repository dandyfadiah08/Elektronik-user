<script setup> 
import { onMounted, onUnmounted } from "vue";
import NavbarDefault from "../../examples/navbars/NavbarDefault.vue";
import Select from "../../components/MaterialSelect.vue"
import Header from "../../examples/Header.vue";
import DefaultCounterCard from "../../examples/cards/counterCards/DefaultCounterCard.vue";
import timeline from "../../assets/img/step1.png";
import Notfound from "../Page/404.vue"
// example components
import TransparentBlogCard from "../../examples/cards/blogCards/TransparentBlogCard.vue";
import BackgroundBlogCard from "../../examples/cards/blogCards/BackgroundBlogCard.vue";
//Vue Material Kit 2 components
import post1 from "@/assets/img/examples/mac.png";
import post2 from "@/assets/img/examples/drone1.png";
import post3 from "@/assets/img/examples/MesinCuci.png";
import post4 from "@/assets/img/examples/lap-n1.png";
const body = document.getElementsByTagName("body")[0];
const local = window.localStorage.getItem("jwt");
onMounted(() => {
  body.classList.add("tradein-page");
  body.classList.add("bg-gray-200");
});
onUnmounted(() => {
  body.classList.remove("tradein-page");
  body.classList.remove("bg-gray-200");
});
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
        selected: "Choose Province",
        pameran:'',
        selectCategories: [],
        gambarkategori:[],

      }
    },
    mounted () {
      
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        const idmitra = window.localStorage.getItem("id_mitra");
        var formdata = new FormData();
        formdata.append("id_mitra", idmitra);
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/api/mitra/"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            const data = result.data
            data.forEach(element => {
              this.selectCategories.push({'id_pameran':element.id_pameran,'nama_pameran':element.nama_pameran})
            });
          } )
          .catch(error => console.log('error', error));
        
        },
    methods:{
      createCartegori(){
        if (this.pameran !== '') {
          const local = window.localStorage.getItem("jwt");
          var myHeaders = new Headers();
          this.gambarkategori=[];
       var formdata = new FormData();
       formdata.append("id_pameran", this.pameran);
       var requestOptions = {
          method: 'POST',
          headers: {
           Authorization :  'Bearer '+local
          },
          body: formdata,
          redirect: 'follow'
       };
       var url = urlbase.URL + "/api/produkmenu/"
       fetch(url, requestOptions)
        .then(response => response.json())
         .then(result =>{
           const data = result.data
           data.forEach(element => {
             this.gambarkategori.push({'id_kategori':element.id_kategori,'Kategori':element.Kategori,'gambar':element.gambar})
           });
         } )
         .catch(error => console.log('error', error));
        }else{
          this.gambarkategori=[];
        }

      },
      kategori(id_pameran,kategori,id_kategori){
        window.localStorage.setItem("id_pameran",id_pameran)
        window.localStorage.setItem("id_kategori",id_kategori)
        window.localStorage.setItem("nama_kategori",kategori)
        window.location.href= urlbase.ROUTES + '/identitas'
      },
      batal(){
        localStorage.removeItem('kode_register')
        window.location.href= urlbase.ROUTES + '/tradein'
      }
    }
    
}

</script>
<template>
  <Notfound v-if="local == null"></Notfound>
    <div v-if="local !== null" class="container position-sticky z-index-sticky top-0">
      <div class="row">
        <div class="col-12">
          <NavbarDefault :sticky="true" />
        </div>
      </div>
    </div>
    <section v-if="local !== null" class="pt-8 "  id="count-stats">
        <div class="row justify-content-center ">

            <img
                style="width: auto;"
                src="../../assets/img/step1.png"
             />
        </div>
    </section>
    <section v-if="local !== null" class="pt-10 pb-4" id="count-stats">
        <div class="row coba justify-content-center ">
            <div class="card card-body blur2 shadow-blur bg- mx-3 mx-md-4 mt-n9">
            <div class="container" >
              <div class="row">
                <div class="d-flex flex-column w-100  p-2 mb-2">
                    <div class="row">
                        <div class="d-grid gap-2 d-md-flex justify-content-md-end">
                            <button class="btn btn-primary" @click="batal" type="button">Batal</button>
                        </div>
                        
                    </div>
                    <h4 style="color: #4C84FF;">1. Kategori</h4>
                  <p><b>PAMERAN/PROMO</b></p>
                  <select style="background-color: cornsilk;" v-model="pameran" class="form-select form-select-lg mb-3" :required="true" @change="createCartegori(pameran)">
                    <option :selected="true" value="">Nama Pameran</option>
                    <option v-for="(category, key) in selectCategories" 

                    v-bind:value="category.id_pameran">{{category.nama_pameran}}</option>
                   </select>
                  <div  class="  mt-6 ">
                    <div class="row">
                    <div v-for="(gambarData, key) in gambarkategori" class="col-lg-3 col-sm-6 mb-2 mr-2">
                      <div class="card card-plain">
                        <div class="card-header p-0 position-flexsibles">
                          <a :href="gambarData.gambar" class="d-block blur-shadow-image">
                            <img
                            style="width: 100%;height: 160px"
                              :src="gambarData.gambar"
                              alt="Macbook"
                              id="coba"
                              class="img-fluid shadow border-radius-lg"
                              loading="lazy"
                            />
                          </a>
                        </div>
                        <div class="card-body px-0">
                          <h5>
                            <a href="#" @click="kategori(this.pameran,gambarData.Kategori,gambarData.id_kategori)" class="text-dark font-weight-bold">
                              {{ gambarData.Kategori }}
                            </a>
                          </h5>
                          <a
                            href="#"
                            @click="kategori(this.pameran,gambarData.Kategori,gambarData.id_kategori)"
                            class="text-sm icon-move-right"
                            style="color:goldenrod"
                            >Pilih Kategori
                            <i class="fa fa-arrow-right" aria-hidden="true"></i>
                    
                          </a>
                        </div>
                      </div>
                        
                    </div>
         
                    
                </div>
                  </div>
                  
                </div>
              </div>
            </div>
            
          </div>
        </div>
       
      </section>
</template>
<style>
</style>