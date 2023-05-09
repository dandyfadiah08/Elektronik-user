<script setup>
import { onMounted, onUnmounted } from "vue";
import NavbarDefault from "../../examples/navbars/NavbarDefault.vue";
import Select from "../../components/MaterialSelect.vue"
import Header from "../../examples/Header.vue";
import Notfound from "../Page/404.vue"
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
        barang:'',
        merk:'',
        kategori:'',
        model:'',
        sn:'',
        size:'',
        tahun:'',
        harga:'',
        subsidi:'',
        subsidi:'',
        total:'',
        


      }
    },
    mounted () {
      this.harga = window.localStorage.getItem("harga")
      this.barang = window.localStorage.getItem("nama_kategori")
      this.kategori = window.localStorage.getItem("nama_kategori")
      this.sn = window.localStorage.getItem("sn")
      var itemtradein=JSON.parse(localStorage.getItem("identitas_tradein")) || [];
      itemtradein.forEach(element => {
        this.merk = element.merk
        this.tahun = element.tahun
        this.size = element.size
        this.subsidi = element. subsidi
      });
      this.total =  parseInt(this.harga)+ parseInt(this.subsidi)
      var itemmodel=JSON.parse(localStorage.getItem("Model-Product")) || [];
      itemmodel.forEach(element => {
        this.model = element.model
      });
    },
    methods:{
     lanjut(){
      var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        var formdata = new FormData();
        var kode_register = window.localStorage.getItem("kode_register");
        var idmitra = window.localStorage.getItem("id_mitra");
        var id_pameran = window.localStorage.getItem("id_pameran");
        var id_user = window.localStorage.getItem("id_toko");
        var subsidi = window.localStorage.getItem("identitas_tradein");
        var parsedObjectsubsidi = JSON.parse(subsidi);
        var subsidifix = parsedObjectsubsidi[0]['subsidi'];
        var harga = window.localStorage.getItem("harga");
        var user_id = window.localStorage.getItem("user_id");
        var user_type = window.localStorage.getItem("user_type");
        var nama_te = window.localStorage.getItem("nama_te");
        var alamat_te = window.localStorage.getItem("alamat_te");
        var identitas_tradein = window.localStorage.getItem("identitas_tradein");
        var parsedObjecttradein = JSON.parse(identitas_tradein);
        var kode_product = parsedObjecttradein[0]['kode_produk'];
        var nama_kategori = parsedObjecttradein[0]['kategori'];
        var merk = parsedObjecttradein[0]['merk'];
        var size = parsedObjecttradein[0]['size'];
        var spec = parsedObjecttradein[0]['spec'];
        var tahun = parsedObjecttradein[0]['tahun'];
        var sn = window.localStorage.getItem("sn");
        var harga = window.localStorage.getItem("hargax");
        var Identitas_toko = window.localStorage.getItem("Identitas_toko");
        var parsedObjecttoko = JSON.parse(Identitas_toko);
        var nama_mitra = parsedObjecttoko[0]['nama_mitra'];
        var Device_checker = parsedObjecttoko[0]['Device_checker'];
        var no_telp = parsedObjecttoko[0]['no_telp'];
        var ModelProduct = window.localStorage.getItem("Model-Product");
        var parsedObjectmodelproduct = JSON.parse(ModelProduct);
        var kode = parsedObjectmodelproduct[0]['kode']
        var list_kuisioner = window.localStorage.getItem("list_kuisioner");
        var name_user = window.localStorage.getItem("user");
        formdata.append("kode_register", kode_register);
        formdata.append("idmitra", idmitra);
        formdata.append("id_pameran", id_pameran);
        formdata.append("id_user", id_user);
        formdata.append("subsidi", subsidifix);
        formdata.append("harga", harga);
        formdata.append("user_id", user_id);
        formdata.append("user_type", user_type);
        formdata.append("nama_te", nama_te);
        formdata.append("alamat_te", alamat_te);
        formdata.append("nama_kategori", nama_kategori);
        formdata.append("kode_product", kode_product);
        formdata.append("merk", merk);
        formdata.append("size", size);
        formdata.append("spec", spec);
        formdata.append("tahun", tahun);
        formdata.append("sn", sn);
        formdata.append("harga", harga);
        formdata.append("nama_mitra", nama_mitra);
        formdata.append("Device_checker", Device_checker);
        formdata.append("no_telp", no_telp);
        formdata.append("kodemodelproduct", kode);
        formdata.append("list_kuisioner", list_kuisioner);
        formdata.append("nama", name_user);
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/api/Summary_tradein/"
        fetch(url, requestOptions)
        .then(response => response.text())
        .then(result =>{ 
          console.log(result)
        window.location.href= urlbase.ROUTES + '/identitas-member'
        })
        .catch(error => console.log('error', error));
      
     },
     batal(){
      localStorage.removeItem('id_pameran')
        localStorage.removeItem('id_kategori')
        localStorage.removeItem('kode_register')
        localStorage.removeItem('nama_kategori')
        localStorage.removeItem('identitas_tradein')
        localStorage.removeItem('id_product')
        localStorage.removeItem('sn')
        localStorage.removeItem('list_kuisioner')
        localStorage.removeItem('harga')
        localStorage.removeItem('Model-Product')
        localStorage.removeItem('Identitas_toko')
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
                    <h4 style="color: #4C84FF;">4.  Summary Product Trade In</h4>
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-12 summary form-check">
                            <div class=" text-summary mt-2">
                                <p class="text-center" style="font-size: 22px;"><b>Summary</b></p>
                                <div class="row mt-3">
                                    <div class="col-md-3">
                                        
                                        <table>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;"><b>Harga</b></p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;"><b>:</b></p>
                                                </td>
                                                <td><p style="font-size: 18px;"><b>Rp   {{this.harga}}</b></p></td>
                                               
                                            </tr>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;"><b>Subsidi</b></p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;"><b>:</b></p>
                                                </td>
                                                <td><p style="font-size: 18px;"><b>Rp {{this.subsidi}}</b></p></td>
                                               
                                            </tr>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;"><b>Total</b></p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;"><b>:</b></p>
                                                </td>
                                                <td><p style="font-size: 18px;"><b>Rp {{this.total}}</b></p></td>
                                                
                                            </tr>
                                        </table>
                                    </div>
                                    <div class="col-md-5">
                                        <table>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Barang</p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;" >{{this.barang}}</p></td>
                                                <td><p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Kategori</p></td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.barang}}</p></td>
                                            </tr>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Merk</p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.merk}}</p></td>
                                                <td><p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model</p></td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.model}}</p></td>
                                            </tr>
                                            <tr>
                                                <td>
                                                    <p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;SN</p>
                                                </td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.sn}}</p></td>
                                                <td><p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Size</p></td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.size}}</p></td>
                                            </tr>
                                            <tr>
                                                <td>
                                                </td>
                                                <td>
                                                </td>
                                                <td></td>
                                                <td><p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Tahun</p></td>
                                                <td>
                                                    <p style="font-size: 18px;">:</p>
                                                </td>
                                                <td><p style="font-size: 18px;">{{this.tahun}}</p></td>
                                            </tr>
                                        </table>
                                    </div>
                               
                                </div>
                            </div>
                          </div>
                          <div class="col-md-12 mt-2 tradein form-check">
                            <div class="text-center text-summary mt-2">
                                <p style="font-size: 22px;"><b>Trade In dengan</b></p>
                                <div class="row justify-content-center">
                                    <div class="col-md-5">
                                    <table class="content-center">
                                        <tr>
                                            <td>
                                                <p style="font-size: 18px;">Merk</p>
                                            </td>
                                            <td>
                                                <p style="font-size: 18px;">:</p>
                                            </td>
                                            <td><p style="font-size: 18px;">{{this.merk}}</p></td>
                                            <td>&nbsp;&nbsp;&nbsp;&nbsp;</td>
                                            <td><p style="font-size: 18px;">&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;Model/Type</p></td>
                                            <td>
                                                <p style="font-size: 18px;">:</p>
                                            </td>
                                            <td><p style="font-size: 18px;">{{this.model}}</p></td>
                                        </tr>
                                    </table>
                                    </div>
                                </div>
                            </div>
                          </div>
                        </div>
                       
                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
                              color="danger"
                              class="mt-3 mb-0"
                              @click="batal"
                              >Batal</MaterialButton
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
                              @click="lanjut"
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
.summary{
    background: #00FFFF; 
    border-radius: 20px;
}
.tradein{
    background: #00FFFF;
    border-radius: 20px;
}
.text-summary{
    color: black;
}
</style>