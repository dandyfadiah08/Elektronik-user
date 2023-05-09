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
//material components
import MaterialInput from "@/components/MaterialInput.vue";
import MaterialInputdisable from "@/components/MaterialInputdisable.vue";
import MaterialTextArea from "@/components/MaterialTextArea.vue";
import MaterialButton from "@/components/MaterialButton.vue";
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
var activities = 'kosong';
var imgaearray = [];
function coba(params) {
  var testing = document.getElementById("activitySelector");
  activities = testing.options[testing.selectedIndex].value;
}
defineProps({
  headers: {
    type: Array,
    default: () => ["Merk", "Spesifikasi", "Status"],
  },
  rows: {
    type: Array,
    required: true,
    image: String,
    name: String,
    email: String,
    position: {
      type: Array,
      default: () => [],
    },
    status: Boolean,
    date: String,
    action: {
      type: Object,
      label: String,
      route: String,
    },
  },
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
      kode_tradein:'',
      harga:'',
      asuransi:'',
      hapusData:'',
      validasi:false,

      }
    },
    mounted () {
      
      var Identitas_rivew = window.localStorage.getItem("revew");
      var parsedObjectrivew = JSON.parse(Identitas_rivew);
      this.kode_tradein = parsedObjectrivew.kode_tradein
      this.harga = parsedObjectrivew.harga_total
      this.asuransi = parsedObjectrivew.benefit.asuransi
      this.hapusData = parsedObjectrivew.benefit.hapus_data
      
      if (this.asuransi == '' && this.hapusData == '') {
        this.validasi = true
      }
    },
    methods:{
      home(){
        localStorage.removeItem('kode_register')
        localStorage.removeItem('id_pameran')
        localStorage.removeItem('id_kategori')
        localStorage.removeItem('nama_kategori')
        localStorage.removeItem('id_product')
        localStorage.removeItem('sn')
        localStorage.removeItem('list_kuisioner')
        localStorage.removeItem('harga')
        localStorage.removeItem('identitas_tradein')
        localStorage.removeItem('Model-Product')
        localStorage.removeItem('Identitas_toko')
        localStorage.removeItem('revew')
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
    <br v-if="local !== null">
    <br v-if="local !== null">
    <div v-if="local !== null" class="row justify-content-center">
    <section class="pt-12 pb-4  col-lg-8" sty  id="count-stats">
        <div class="row coba justify-content-center ">
            <div class="card card-body blur2 shadow-blur bg- mx-3 mx-md-4 mt-n9">
            <div class="container" >
              <div class="row">
                <div class="d-flex flex-column text-center w-100  p-2 mb-2">
                    <h4  style="color: #4C84FF;" class="header-review">SELAMAT</h4>
                    <p  class="object-review">Anda telah terdaftar Trade In</p>
                    <div class="row">
                        <div class="col-md-12">
                            <img src="../../assets/img/sukses.png" class="w-20 border-radius-md " alt="">
                            <p style="color:black">Kode Trade In  Anda:</p>
                        </div>
                        <div class="col-md-12">
                            <p class="kode-tradein"><b>{{this.kode_tradein}}</b></p>
                        </div>
                        
                        <div class="col-md-12">
                            <img src="../../assets/img/barcode_PNG4.png" class="w-20 border-radius-md " alt="">
                        </div>
                        <div class="col-md-12">
                            <p class="kode-tradein"><b>Rp. {{this.harga}}</b></p>
                        </div>
                        <div class="col-md-12" v-if="this.validasi == false">
                            <p class="kode-tradein">Benefit: <br> <b>{{this.asuransi}}</b><br><b>{{this.hapusData}}</b></p>
                        </div>
                      </div>
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
                              @click="home"
                              color="info"
                              class="mt-3 mb-0 ml-2"
                              >Lanjut</MaterialButton
                            >
                            
                          </div>
                        </div>
                      </div>
                </div>
              </div>
            </div>
            
          </div>
        </div>
       
      </section>
    </div>
</template>
<style>
.header-review{
    font-family: 'Inria Serif';
font-style: normal;
font-weight: 400;
font-size: 32px;
line-height: 38px;

color: #0000FF;

}
.object-review{
    font-family: 'Inria Serif';
font-style: normal;
font-weight: 400;
font-size: 32px;
line-height: 38px;

color: #000000;
}
.kode-tradein{
    font-family: 'Inria Serif';
font-style: normal;
font-weight: 400;
font-size: 20px;
line-height: 38px;

color: #000000;
}
</style>