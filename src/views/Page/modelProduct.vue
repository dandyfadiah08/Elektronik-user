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
        pameran:'',
        selectmodelproduct: [],
        gambarkategori:[],
        kodeproduct:'',
        brand:'',
        model:'',
        harga:'',
        tnt:'',
        id_modelproduct:'',

      }
    },
    mounted () {
      
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        const idmitra = window.localStorage.getItem("id_mitra");
        var formdata = new FormData();
        formdata.append("id_pameran", window.localStorage.getItem("id_pameran"));
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/Api/harga/getharga/api/"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            const data = result.data
            data.forEach(element => {
              this.selectmodelproduct.push({'id':element.id,'brand':element.brand,'kode':element.kode,'model':element.model, 'harga':element.harga})
            });
          } )
          .catch(error => console.log('error', error));
        
        },
    methods:{
      createmodel(Dataid,Datakode,Databrand,Datamodel,Dataharga){
        this.kodeproduct = Datakode
        this.brand = Databrand
        this.model = Datamodel
        this.id_modelproduct=Dataid
        this.harga = Dataharga
      },
      simpan(){
        if (this.tnt == false) {
          alert("maaf click kolom tnc untuk melanjutkan kehalam berikutnya")
        } else {
          if (this.kodeproduct =='') {
            alert("Kode Product tidak boleh kosong!")
          } else {
            var SeasonMproduct = []
            var myHeaders = new Headers();
             const local = window.localStorage.getItem("jwt");
             var formdata = new FormData();
              formdata.append("id", this.id_modelproduct);
              formdata.append("kode", this.kodeproduct);
              formdata.append("brand", this.brand);
              formdata.append("model", this.model);
              formdata.append("harga", this.harga);
              
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        SeasonMproduct.push({'id': this.id_modelproduct, 'kode':this.kodeproduct, 'brand':this.brand, 'model':this.model, 'harga':this.harga})
        var url = urlbase.URL + "/Api/harga/simpanmodel/api/"
        fetch(url, requestOptions)
            .then(response => response.text())
            .then(result => {
              console.log(result)
              window.localStorage.setItem("Model-Product",JSON.stringify(SeasonMproduct))
              window.location.href= urlbase.ROUTES + '/identitas-toko'
            })
            .catch(error => console.log('error', error));
          }
        }
      },
      back(){
        window.location.href= urlbase.ROUTES + '/kuisioner'
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
                v-for="(category, key) in selectmodelproduct"
                variant="gradient"
                color="info"
                @click="createmodel(category.id,category.kode,category.brand,category.model,category.harga)"
                data-bs-dismiss="modal"
                class=" w-100 "
                ><b style="color: black;">{{category.kode}}</b><br><b style="color: black;">{{category.brand}}</b><br><b style="color: black;">{{category.model}}</b></MaterialButton
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
                src="../../assets/img/step3.png"
             />
        </div>
    </section>
    <section v-if="local !== null" class="pt-10 pb-4" id="count-stats">
        <div class="row coba justify-content-center ">
            <div class="card card-body blur2 shadow-blur bg- mx-3 mx-md-4 mt-n9">
            <div class="container" >
              <div class="row">
                <div class="d-flex flex-column w-100  p-2 mb-2">
                    <h4 style="color: #4C84FF;">3. Trade In Dengan (Product yang diingkan)</h4>
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-5 form-check">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              aria-readonly="true"
                              :value="kodeproduct"
                              type="text"
                              label="Kode Product"
                              placeholder="Kode Product"
                            />
                          </div>
                          <div class="col-md-1 ps-md-2 form-check">
                            <MaterialButton
                              variant="gradient"
                              color="info"
                              class="mt-3 mb-0"
                              data-bs-toggle="modal"
                              data-bs-target="#exampleModal"
                              >Cari</MaterialButton
                            >
                          </div>
                          <div class="col-md-6 form-check">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              :value="brand"
                              label="Brand"
                              placeholder="Brand"
                            />
                          </div>
                          <div class="col-md-6 form-check">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              :value="model"
                              label="Model"
                              placeholder="Model"
                            />
                          </div>
                          <div class="col-md-12 form-check">
                            <input v-model="tnt" class="form-check-input"  type="checkbox" value="" id="flexCheckChecked" >
                            <label class="form-check-label" for="flexCheckChecked">
                               <b style="color: black;">Saya Setuju untuk melakukan Trade In ini dengan ketentuan yang berlaku</b> 
                            </label>
                          </div>
                        </div>
                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
                              color="danger"
                              @click="back"
                              class="mt-3 mb-0"
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