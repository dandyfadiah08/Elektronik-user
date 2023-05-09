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
        listkuisioner: [],
        asuransivalid:0,
        asuransiData:0,
        dataHapusData:0,
        hapusData:0,
        hapusDatavalid:0,
        validation : false,
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
        var url = urlbase.URL + "/api/produkmenu/kategori/detail"
        fetch(url, requestOptions)
         .then(response => response.json())
          .then(result =>{
            var Dataasuransi = result.data[0].asuransi
            if (Dataasuransi == 1) {
              this.asuransivalid = 1
            }
            var Datahapus = result.data[0].hapusData
            if (Datahapus == 1) {
              this.hapusDatavalid = 1
            }
            if (this.asuransivalid == 0 && this.hapusDatavalid == 0) {
             this.validation = true
            }
          } )
          .catch(error => console.log('error', error));
      
        },
    methods:{
    save(){
      var name = document.getElementById('nameMember').value
      var telp = document.getElementById('telp').value
      var email = document.getElementById('email').value
      
      if (this.validation == false) {
        var tnc2 = document.getElementById("tnc").checked
      if (tnc2 == true) {
        if (this.asuransivalid == 1) {
            
            var asuransi2 = document.getElementById("asuransi").checked
            if (asuransi2 == true) {
              this.asuransiData = 1
            } else {
              this.asuransiData = 0
            }
          } else {
            this.asuransiData = 0
          }
          if (this.hapusDatavalid == 1) {
        
        var hapusData2 = document.getElementById("HapusData").checked
        if (hapusData2 == true) {
          this.dataHapusData = 1
        } else {
          this.dataHapusData = 0
        }
      } else {
        this.dataHapusData = 0
      }
          
          if (this.asuransiData == 0 && this.dataHapusData == 0) {
            alert('Pilih Benefit yang tersedia')
          } else {
            var myHeaders = new Headers();
            const local = window.localStorage.getItem("jwt");
            const kode_register = window.localStorage.getItem("kode_register");
            const user_id = window.localStorage.getItem("user_id");
            var nama = document.getElementById('nameMember').value
            var no_hp = document.getElementById('telp').value
            var email = document.getElementById('email').value
            var formdata = new FormData();
             formdata.append("kode_register", kode_register);
             formdata.append("id_user", user_id);
             formdata.append("nama", nama);
             formdata.append("no_hp", no_hp);
             formdata.append("email", email);
             formdata.append("asuransi", this.asuransiData);
             formdata.append("hapus_data", this.dataHapusData);
            var requestOptions = {
              method: 'POST',
              headers: {
                Authorization :  'Bearer '+local
              },
              body: formdata,
              redirect: 'follow'
            };
            if (document.getElementById('nameMember').value == '') {
              alert('Nama Member Tidak boleh kosong')
            }else if(document.getElementById('telp').value == ''){
              alert('Nomor Telephone Tidak boleh kosong')
            }else if(document.getElementById('email').value ==''){
              alert('email Tidak boleh kosong')
            }else{
              var url = urlbase.URL + "/api/customer/"
              fetch(url, requestOptions)
                  .then(response => response.json())
                  .then(result => {
                    window.localStorage.setItem("revew",JSON.stringify(result.data))
                    window.location.href= urlbase.ROUTES + '/review'
                  })
                  .catch(error => console.log('error', error));
            }
          }
      } else {
        alert("click Syarat untuk mesetujui!!")
      }

      } else {
        var myHeaders = new Headers();
            const local = window.localStorage.getItem("jwt");
            const kode_register = window.localStorage.getItem("kode_register");
            const user_id = window.localStorage.getItem("user_id");
            var nama = document.getElementById('nameMember').value
            var no_hp = document.getElementById('telp').value
            var email = document.getElementById('email').value
            var formdata = new FormData();
             formdata.append("kode_register", kode_register);
             formdata.append("id_user", user_id);
             formdata.append("nama", nama);
             formdata.append("no_hp", no_hp);
             formdata.append("email", email);
             formdata.append("asuransi", this.asuransiData);
             formdata.append("hapus_data", this.dataHapusData);
            var requestOptions = {
              method: 'POST',
              headers: {
                Authorization :  'Bearer '+local
              },
              body: formdata,
              redirect: 'follow'
            };
            if (document.getElementById('nameMember').value == '') {
              alert('Nama Member Tidak boleh kosong')
            }else if(document.getElementById('telp').value == ''){
              alert('Nomor Telephone Tidak boleh kosong')
            }else if(document.getElementById('email').value ==''){
              alert('email Tidak boleh kosong')
            }else{
              var url = urlbase.URL + "/api/customer/"
              fetch(url, requestOptions)
                  .then(response => response.json())
                  .then(result =>{
                    window.localStorage.setItem("revew",JSON.stringify(result.data))
                    window.location.href= urlbase.ROUTES + '/review'
                  } )
                  .catch(error => console.log('error', error));
            }
      }

      
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
                src="../../assets/img/step5.png"
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
                        <div class="col-md-11">
                            <h4 style="color: #4C84FF;">4. Identitas Customer</h4>
                        </div>
                        <div class="col-md-1 text-center">
                          <MaterialButton
                            variant="gradient"
                            color="danger"
                            >Batal</MaterialButton
                          >
                        </div>
                      </div>
                      <div class="card-body p-0 my-3">
                        <div class="row">
                          <div class="col-md-12 form-check">
                              <p style="font-size: small;">
                                  * input harus diisi
                              </p>
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="text"
                              id="nameMember"
                              label="Nama Customer* :"
                              placeholder="Isi Nama Lengkap"
                            />
                          </div>
                          <div class="col-md-12 form-check">
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="text"
                              id="telp"
                              label="No. Handphone* :"
                              placeholder="Isi Number Handphone"
                            />
                          </div>
                          <div class="col-md-12 form-check">
                            <MaterialInput
                              class="input-group-static mb-4"
                              type="email"
                              id="email"
                              label="Email* :"
                              placeholder="Isi alamat E-mail"
                            />
                          </div>
                        </div>
                        <div class="summary mt-2" v-if="this.validation == false">
                            <div >
                                <p class="judul-benefit text-center">
                                    Tambah Benefit
                                     <p class="text-center" style="color: #000000;">Silahkan pilih benefit di bawah jika customer ingin mengikutinya</p>
                                </p>
                                <div class="row" v-if="this.asuransivalid == 1">
                                    <div  class="  col-md-2 ml-2">
                                    </div>
                                    <div  class=" summary2 col-md-8 ml-3">
                                        <div class="form-check mt-2">
                                            <input class="form-check-input" type="checkbox"  id="asuransi" >
                                            <label class="form-check-label" for="flexCheckChecked">
                                              <b style="color: black;">
                                                Asuransi
                                              </b>
                                            </label>
                                          </div>
                                          <p class="obeject-benefit">
                                            Konsumen Anda juga dapat ikut serta
                                            dalam program Asuransi Kecelakaan Diri 
                                            yang mengakibatkan kematian dengan nilai
                                            santunan sebesar Rp 20,000,000 (Dua puluh juta Rupiah). 
                                            Konsumen akan diminta untuk melakukan registrasi/aktivasi
                                            pada link yang akan dikirimkan ke nomor WhatsApp konsumen. 
                                            Sertifikat Polis juga akan dikirimkan melalui 
                                            nomor WhatsApp Konsumen setelah proses aktivasi berhasil.
                                          </p>
                                    </div>
        
                                    <div  class="  col-md-2 ml-2">
                                    </div>
                                </div>
                                <br>
                                <div class="row" v-if="this.hapusDatavalid == 1">
                                    <div  class="  col-md-2 ml-2">
                                    </div>
                                    <div  class=" summary2 col-md-8 ml-3">
                                        <div class="form-check mt-2">
                                            <input class="form-check-input" type="checkbox"  id="HapusData" >
                                            <label class="form-check-label" for="flexCheckChecked">
                                              <b style="color: black;">
                                                Hapus Data
                                              </b>
                                            </label>
                                          </div>
                                          <p class="obeject-benefit">
                                            Seluruh data pada device lama konsumen Anda akan 
                                            dihapus secara permanen dan
                                            bersertifikasi dengan software khusus. 
                                            Ini merupakan layanan tambahan untuk meningkatkan
                                            keamanan dan kenyamanan konsumen Trade In Plus.
                                          </p>
                                    </div>
        
                                    <div  class="  col-md-2 ml-2">
                                    </div>
                                </div>
                            </div>
                            <br>
                            <div class="row">
                                <div class="col-md-1"></div>
                                <div class="col-md-10">
                                    <p class="form-check notes-benefit">
                                        *akan dikirmkan email kepada customer untuk proses register Free Asuransi Personal Accident  dan/atau Sertifikat penghapusan Data Permanen sesuai  dengan benefit 
                                        yang sudah dipilih customer maksimal 7(tujuh) hari kerja
                                    </p>
                                    <p class="form-check notes-benefit mt-2">
                                       <b class="mt-2">*Syarat dan Ketentuan</b><br>
                                       Syarat dan ketentuan promo FREE Benefit dapat berubah sewaktu-waktu tanpa pemberitahuan terlebih dahulu
                                    </p>
                                    <div class="form-check notes-benefit mb-2 mt-1">
                                        <input class="form-check-input" type="checkbox" value="" id="tnc" >
                                        <label class="form-check-label mt-1" for="flexCheckChecked"  style="color: black;">
                                            Saya setuju bahwa pada saat melakukan aktivasi benefit asuransi kecelakaan diri, saya memasukan data yang sama sesuai dengan data KTP yang di-input
                                        </label>
                                        <label class="form-check-label" for="flexCheckChecked"  style="color: black;">
                                             di aplikasi Trade In Plus pada saat melakukan transaksi tukar tambah. (Syarat & Ketentuan)
                                        </label>
                                      </div>
                                </div>
                                <div class="col-md-1"></div>
                            </div>
                        </div>
                        <div class="row">
                          <div class="col-md-12 text-center">
                            <MaterialButton
                              variant="gradient"
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
            
          </div>
        </div>
       
      </section>
</template>
<style>
.summary{
    background: #00FFFF;
    border-radius: 100px 0px 100px 0px;
}
.summary2{
    background: #D9D9D9;
    border-radius: 0px 40px 0px 40px;
}
.judul-benefit{
    font-family: 'Inria Serif';
font-style: normal;
font-weight: 700;
font-size: 26px;
line-height: 38px;

color: #000000;
}
.notes-benefit{
    font-family: 'Inria Serif';
font-style: normal;
font-weight: 400;
font-size: 13px;
line-height: 16px;

color: #000000;
}
.obeject-benefit{
    font-family: 'Inria Serif';
    font-style: normal;
    font-weight: 400;
    font-size: 13px;
    line-height: 16px;
    
    color: #000000;
}

</style>