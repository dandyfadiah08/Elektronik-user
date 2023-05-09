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
    name: 'toko',
    data () {
      return {
        nama_toko : '',
        nama_mitra :'',
        identitastoko:[],
      }
    },
    mounted () {
      this.nama_toko = window.localStorage.getItem("nama_te");
      this.nama_mitra = window.localStorage.getItem("nama_te");
        
        },
    methods:{
      simpan(){
        var telp = document.getElementById('telp').value
        var nameDevice = document.getElementById('nameDevice').value
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        var formdata = new FormData();
        formdata.append("nama_mitra", this.nama_mitra);
        formdata.append("nama_toko", this.nama_toko);
        formdata.append("Device_checker", nameDevice);
        formdata.append("no_telp", telp);
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/api/toko/simpantoko/api/"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            if (result.success == true) {
              this.identitastoko.push({'nama_mitra':this.nama_mitra,'nama_toko':this.nama_toko, 'Device_checker':nameDevice, 'no_telp':telp})
              window.localStorage.setItem("Identitas_toko",JSON.stringify(this.identitastoko))
              window.location.href= urlbase.ROUTES + '/summary'
            } else {
              Toastify({
            text: result.message,
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
          } )
          .catch(error => console.log('error', error));
      },
      back(){
        window.location.href= urlbase.ROUTES + '/model-product'
      }
    }
    
}

</script>
<template>
  <Notfound v-if="local == null"></Notfound>
  <div
  class="modal fade bd-example-modal-sm"
  id="exampleModal"
  tabindex="-1"
  aria-labelledby="exampleModalLabel"
  aria-hidden="true"
  v-if="local !== null"
>
  <div class="modal-dialog modal-sm  modal-dialog-scrollable">
    <div class="modal-content">
      <div class="modal-header">
        <h5 class="modal-title" id="exampleModalLabel">
          Cari Product
        </h5>
        <MaterialButton
          color="none"
          class="btn-close"
          data-bs-dismiss="modal"
          aria-label="Close">
        </MaterialButton>
      </div>
      <div class="modal-body">
        <MaterialButton
                variant="gradient"
                color="info"
                data-bs-dismiss="modal"
                class=" w-100 "
                ><b style="color: black;">Samsung</b><br><b style="color: black;">Galaxy A32</b></MaterialButton
              >
        <MaterialButton
              variant="gradient"
              color="info"
              data-bs-dismiss="modal"
              class=" w-100 "
              ><b style="color: black;">Samsung</b><br><b style="color: black;">Galaxy A31</b></MaterialButton
            >
            <MaterialButton
              variant="gradient"
              color="info"
              data-bs-dismiss="modal"
              class=" w-100 "
              ><b style="color: black;">Samsung</b><br><b style="color: black;">S20</b></MaterialButton
            >
      </div>
    </div>
  </div>
</div>
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
                src="../../assets/img/step4.png"
             />
        </div>
    </section>
    <section v-if="local !== null" class="pt-10 pb-4" id="count-stats">
        <div class="row coba justify-content-center ">
            <div class="card card-body blur2 shadow-blur bg- mx-3 mx-md-4 mt-n9">
            <div class="container" >
              <div class="row">
                <div class="d-flex flex-column w-100  p-2 mb-2">
                    <h4 style="color: #4C84FF;">4. Identitas Toko</h4>
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-6 form-check">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              aria-readonly="true"
                              :value="nama_mitra"
                              type="text"
                              label="Nama Mitra"
                              placeholder="Nama Mitra"
                            />
                          </div>
                          <div class="col-md-6 form-check">
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="text"
                              id="nameDevice"
                              label="Device Checker*"
                              placeholder="Nama Device Cheker"
                            />  
                          </div>
                          <div class="col-md-6 form-check">
                            <MaterialInputdisable
                              class="input-group-static mb-4" 
                              type="text"
                              :value="nama_toko"
                              label="Nama Toko"
                              placeholder="Nama Toko"
                            />
                          </div>
                          <div class="col-md-6 form-check">
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="text"
                              id="telp"
                              label="No. Telp Device Checker*"
                              placeholder="Nomot Telepon"
                            />
                          </div>
                        </div>
                        <p class="form-check" style="font-size: small;">
                            * input harus diisi
                        </p>
                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
                              color="danger"
                              class="mt-3 mb-0"
                              @click="back"
                              >Kembali</MaterialButton
                            >
                            <MaterialButton
                              variant="gradient"
                              disabled
                              color="white"
                              class="mt-3 mb-0 ml-2"
                              > </MaterialButton
                            >
                            <MaterialButton
                              variant="gradient"
                              color="info"
                              @click="simpan()"
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
</template>
<style>
</style>