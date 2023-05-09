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
import post4 from "@/assets/img/examples/laptopmati.jpg";
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
        selected: "Choose Province",
        pameran:'',
        listkuisioner: [],
      }
    },
    mounted () {
      
        var myHeaders = new Headers();
        const local = window.localStorage.getItem("jwt");
        const idmitra = window.localStorage.getItem("id_kategori");
        var formdata = new FormData();
        formdata.append("id_kategori", idmitra);
        var requestOptions = {
           method: 'POST',
           headers: {
            Authorization :  'Bearer '+local
           },
           body: formdata,
           redirect: 'follow'
        };
        var url = urlbase.URL + "/api/kuisioner/list/api/kuisioner"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            const data = result.data
            var number=1
            data.forEach(element => {
              this.listkuisioner.push({'numberquisioner':number,'id_kuisioner':element.id,'kuisioner':element.kuisioner,'listkuisioner':element.listkuisioner})
              number = number +1 
            });
          } )
          .catch(error => console.log('error', error));
        
        },
    methods:{
     save(){
      var checkbox = document.getElementsByName("Datalistkuisioner");
        var valuelistkuisioner = "";
        var Datafixedkuisioner = [];
        for(var i = 0; i < checkbox.length; i++){
            if(checkbox[i].checked){
              var Data_kuisioner = checkbox[i].value.split(',')
              Datafixedkuisioner.push({'id_mkuisioner':Data_kuisioner[0],'id_liskuisioner':Data_kuisioner[1],'list':Data_kuisioner[2]});
            }
        }
       var length = Datafixedkuisioner.length
       if (length <= 0) {
        Toastify({
            text:" Data List Kuisioner tidak boleh kosong !",
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
       } else {
        
        var datakuisioner =[];
        var listkuisioner =[];
         Datafixedkuisioner.forEach(Data => {
           const local = window.localStorage.getItem("jwt");
           var formdata = new FormData();
           formdata.append("id_mkuisioner", Data['id_mkuisioner']);
           formdata.append("id_liskuisioner", Data['id_liskuisioner']);
           formdata.append("list", Data['list']);
             var requestOptions = {
               method: 'POST',
               headers: {
                 Authorization :  'Bearer '+local
               },
               body: formdata,
               redirect: 'follow'
             };
             listkuisioner.push({'id_mkuisioner': Data['id_mkuisioner'], 'id_liskuisioner':Data['id_liskuisioner'], 'list':Data['list']})
             var url = urlbase.URL + "/api/kuisioner/simpan/api/savekuisioner"
             fetch(url, requestOptions)
               .then(response => response.json())
               .then(result =>{
                datakuisioner.push(result.data)
                window.localStorage.setItem("list_kuisioner",JSON.stringify(listkuisioner))
               } )
               .catch(error => console.log('error', error));
             }); 
             var delayInMilliseconds = 1000; //1 second
             const local = window.localStorage.getItem("jwt");
             setTimeout(function() {
              var array = []
              datakuisioner.forEach(element => {
                  array.push(element)
              }); 
              var formdata = new FormData();
              formdata.append("id_product", window.localStorage.getItem("id_product"));
              var lisdata = window.localStorage.getItem("list_kuisioner")
              formdata.append("list_kuisioner",lisdata);
              var requestOptions = {
                method: 'POST',
                headers:  {
                  Authorization :  'Bearer '+local,
                },
                body: formdata,
                redirect: 'follow'
              };
              var url = urlbase.URL + "/api/kuisioner/simpankuisioner/api/"
              fetch(url, requestOptions)
                .then(response => response.json())
                .then(result =>{ 
                  window.localStorage.setItem("harga", result.data.harga)
                  window.location.href= urlbase.ROUTES + '/model-product'
                })
                .catch(error => console.log('error', error));
            }, delayInMilliseconds);
       }
     },
     batal(){
      localStorage.removeItem('id_pameran')
        localStorage.removeItem('id_kategori')
        localStorage.removeItem('kode_register')
        localStorage.removeItem('nama_kategori')
        localStorage.removeItem('identitas_tradein')
        localStorage.removeItem('id_product')
        localStorage.removeItem('sn')
        window.location.href= urlbase.ROUTES + '/tradein'
     },
     back(){
      window.location.href= urlbase.ROUTES + '/identitas'
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
                src="../../assets/img/step2.png"
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
                            <button @click="batal" class="btn btn-primary" type="button">Batal</button>
                        </div>
                        
                    </div>
                    <h4 style="color: #4C84FF;">2. Penilain Product (List Kuisioner)</h4>
                    <div v-for="(Datakuisioner, key) in listkuisioner">

                      <h5 style="color: black" class="mt-3">Q{{ Datakuisioner.numberquisioner }}. {{Datakuisioner.kuisioner}}</h5>
                      <div class="mt-6">
                          <div class="row ">
                          <div v-for="(list, key) in Datakuisioner.listkuisioner" class="col-lg-4 col-sm-6 mb-2 mr-2">
                            <div class="card card-plain">
                              <div class="card-header p-0 position-relative">
                                <a :href="list.gambar" v-if="list.gambar !== null" class="d-block blur-shadow-image">
                                  <img
                                  style="width: 100%;height: 160px"
                                    :src="list.gambar"
                                    alt="Macbook"
                                    id="coba"
                                    class="img-fluid shadow border-radius-lg"
                                    loading="lazy"
                                  />
                                </a>
                              </div>
                              <div class="card-body px-0">
                                <h5>
                                  <div class="form-check">
                                      <input class="form-check-input" type="checkbox" :value="[Datakuisioner.id_kuisioner,list.id_listkuisioner,list.list]" name="Datalistkuisioner">
                                      <label class="form-check-label" for="flexCheckDefault">
                                        <b style="color: black;">
                                          {{ list.list }}
                                        </b>
                                      </label>
                                    </div>
                                </h5>
                              </div>
                            </div>
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
                        @click="save"
                        class="mt-3 mb-0 ml-2"
                        >Lanjut</MaterialButton
                      >
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