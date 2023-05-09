<script setup>

import { onMounted, onUnmounted } from "vue";
import NavbarDefault from "../../examples/navbars/NavbarDefault.vue";
import Select from "../../components/MaterialSelect.vue"
import Header from "../../examples/Header.vue";
import DefaultCounterCard from "../../examples/cards/counterCards/DefaultCounterCard.vue";
import timeline from "../../assets/img/step1.png";
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
import Notfound from "../Page/404.vue"
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
        selected: "Choose Province",
        pameran:'',
        selectkodeproduct: [],
        id_product:'',
        kode_produk:'',
        kategori:'',
        size:'',
        tahun:'',
        merk:'',
        spec:'',
        subsidi :'',
        identitas_tradein : []

      }
    },
    mounted () {
      
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        const idmitra = window.localStorage.getItem("id_mitra");
        var formdata = new FormData();
        formdata.append("id_pameran", window.localStorage.getItem("id_pameran"));
        formdata.append("kategori", window.localStorage.getItem("nama_kategori"));
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/Api/ProdukMenu/kodeproduct/api/kodeproduk/"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            // console.log(result)
            const data = result.data
            data.forEach(element => {
              this.selectkodeproduct.push({'id_product':element.id_product,'kode_produk':element.kode_produk,'kategori':element.kategori,'size':element.size,'tahun':element.tahun,'merk':element.merk,'spec':element.spec, 'subsidi':element.subsidi})
            });
          } )
          .catch(error => console.log('error', error));
        
        },
    methods:{
      edit(id_product,kode_produk,kategori,size,tahun,merk,spec,subsidi){

        this.id_product = id_product
        this.kode_produk= kode_produk
        this.kategori= kategori
        this.size= size
        this.tahun= tahun
        this.merk= merk
        this.spec= spec
        this.subsidi = subsidi
      },
      simpan(){
        var sn = document.getElementById('sn').value
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        var formdata = new FormData();
        formdata.append("id_pameran", window.localStorage.getItem("id_pameran"));
        formdata.append("id_product", this.id_product);
        formdata.append("sn", sn);
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/Api/ProdukMenu/tambahproduk/api/createproduk/"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            if (result.success == true) {
              this.identitas_tradein.push({'id_product':this.id_product, 'kode_produk': this.kode_produk, 'kategori':this.kategori, 'size': this.size, 'tahun':this.tahun, 'merk': this.merk, 'spec': this.spec, 'subsidi' : this.subsidi})
              window.localStorage.setItem("identitas_tradein",JSON.stringify(this.identitas_tradein))
              window.localStorage.setItem("id_product",this.id_product)
              window.localStorage.setItem("sn",sn)
              window.location.href= urlbase.ROUTES + '/kuisioner'
            } else {
              Toastify({
            text:" Sereial Number and kode product not empty !",
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
      batal(){
        localStorage.removeItem('id_pameran')
        localStorage.removeItem('id_kategori')
        localStorage.removeItem('kode_register')
        localStorage.removeItem('nama_kategori')
        window.location.href= urlbase.ROUTES + '/tradein'
      },
      back(){
        window.location.href= urlbase.ROUTES + '/pameran'
      }
    }
    
}

</script>
<template>
  <Notfound v-if="local == null"></Notfound>
  <div
  class="modal fade bd-example-modal-lg"
  id="exampleModal"
  tabindex="-1"
  aria-labelledby="exampleModalLabel"
  aria-hidden="true"
  v-if="local !== null"
>
  <div class="modal-dialog modal-lg  modal-dialog-scrollable">
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
        <div class="table-responsive">
          <table id="dtBasicExample" class="table align-items-center mb-0">
            <thead>
              <tr>
                <th
                  v-for="(header, index) in headers"
                  :key="header"
                  class="text-uppercase text-secondary text-xxs text-center font-weight-bolder opacity-9"
                  :class="{ 'ps-2': index == 1, 'text-center': index > 1 }"
                >
                  {{ header }}
                </th>
              </tr>
            </thead>
            <tbody class="text-center">
             <tr v-for="(product, key) in selectkodeproduct">
              <td>{{product.merk}}</td>
              <td>{{product.spec}}</td>
              <td><MaterialButton
                variant="gradient"
                @click="edit(product.id_product,product.kode_produk,product.kategori,product.size,product.tahun,product.merk,product.spec,product.subsidi)"
                color="warning"
                data-bs-dismiss="modal"
                class="mt-3 mb-0 ml-2 "
                >Pilih</MaterialButton
              ></td>
             </tr>
            </tbody>
          </table>
        </div>
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
                    <h4 style="color: #4C84FF;">1. Identitas Trade In (MACBOOK)</h4>
                    
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-4">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              id="kodeproduct"
                              :value="this.kode_produk"
                              aria-readonly="true"
                              type="text"
                              label="Kode Product"
                              placeholder="Kode Product"
                            />
                          </div>
                          <div class="col-md-2 ps-md-2">
                            <MaterialButton
                              variant="gradient"
                              color="info"
                              class="mt-3 mb-0"
                              data-bs-toggle="modal"
                              data-bs-target="#exampleModal"
                              >Cari</MaterialButton
                            >
                          </div>
                          <div class="col-md-6 ps-md-2">
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="text"
                              id="sn"
                              label="Serial Number"
                              placeholder="Serial Number"
                            />
                          </div>
                          <div class="col-md-6">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              id="kategori"
                              :value="this.kategori"
                              type="text"
                              label="Kategori"
                              placeholder="Kategori"
                            />
                          </div>
                          <div class="col-md-3">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              id="size"
                              :value="this.size"
                              label="Ukuran Layar(inch)"
                              placeholder="Size"
                            />
                          </div>
                          <div class="col-md-3">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              id="tahun"
                              :value="this.tahun"
                              label="Tahun Produksi"
                              placeholder="Tahun"
                            />
                          </div>
                          <div class="col-md-6">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              id="merk"
                              :value="this.merk"
                              label="Merk"
                              placeholder="Merk"
                            />
                          </div>
                           <div class="col-md-6">
                            <MaterialInputdisable
                              class="input-group-static mb-4"
                              type="text"
                              id="spec"
                              :value="this.spec"
                              label="Model/Type"
                              placeholder="Spesifikasi"
                            />
                          </div>
                        </div>

                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
                              @click="back"
                              color="danger"
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
                              @click="simpan()"
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
</template>
<style>
table.dataTable thead .sorting:after,
table.dataTable thead .sorting:before,
table.dataTable thead .sorting_asc:after,
table.dataTable thead .sorting_asc:before,
table.dataTable thead .sorting_asc_disabled:after,
table.dataTable thead .sorting_asc_disabled:before,
table.dataTable thead .sorting_desc:after,
table.dataTable thead .sorting_desc:before,
table.dataTable thead .sorting_desc_disabled:after,
table.dataTable thead .sorting_desc_disabled:before {
  bottom: .5em;
}
</style>