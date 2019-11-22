<template>
  <div class="card mx-xl-5">

    <!-- Card body -->
    <div class="card-body">
        <!-- Default form subscription -->
        <form>
            <h2>Upload Video</h2>

              <!--para la subida del video-->
              <b-container class="bv-example-row">
                <b-row>
                  <b-col>
                    <!--para la subida del video-->
                    <b-form-file required v-if="!isUploadedVideo && !isUploadingVideo" accept="video/*" v-model="fileVideo" :state="Boolean(fileVideo)" placeholder="Drop video here..." drop-placeholder="Drop file here..."></b-form-file>
                    <b-button variant="primary" disabled v-if="isUploadingVideo">
                      <b-spinner small type="grow"></b-spinner>
                      Uploading video...
                    </b-button>
                    <div class="alert alert-success form-custom m-1" v-if="isUploadedVideo" role="alert">
                      Video uploaded sucessfully!!
                    </div>
                  </b-col>
                  <b-col>
                    <!--para la subida de la portada-->
                    <b-form-file required v-if="!isUploadedImage && !isUploadingImage" accept="image/*" v-model="fileImage" :state="Boolean(fileImage)" placeholder="Drop image here..." drop-placeholder="Drop file here..."></b-form-file>
                    <b-button variant="primary" disabled v-if="isUploadingImage">
                      <b-spinner small type="grow"></b-spinner>
                      Uploading image...
                    </b-button>
                    <div class="alert alert-success form-custom m-1" v-if="isUploadedImage" role="alert">
                      Image uploaded sucessfully!!
                    </div>
                    <!-- logging que se mostrará cuando el usuario quiera guardar los cambios y subir video-->
                  </b-col>
                </b-row>
                <b-row>
                  <b-col>
                    <!-- title -->
                    <input type="text" :value="this.form.title" required placeholder="title" id="title" class="form-control form-custom m-1"/>
                  </b-col>
                  <b-col>
                    <!-- tags -->
                    <input type="text" :value="this.form.tags" required placeholder="tags : separated with comma ('tag1,tag2,...')" id="tags" class="form-control form-custom m-1"/>
                  </b-col>
                </b-row>
                <b-row rows="5">
                  <b-col>
                    <b-form-textarea  :value="this.form.description" type="text-area" placeholder="description" id="description" class="form-control m-1" height="15rem"/>
                  </b-col>
                  <b-col>

                  </b-col>
                </b-row>
                  <div class="text-center py-4 mt-3">
                    <button type="submit" class="btn btn-primary btn-lg btn-block">Send<i class="far fa-paper-plane ml-2"></i></button>

                  </div>
    
              </b-container>

            <!-- Default input title -->
     
        </form>
        <!-- Default form subscription -->

    </div>
    <!-- Card body -->
    <div id="app">
    <div v-if="showModal">
      <transition name="modal">
        <div class="modal-mask">
          <div class="modal-wrapper" >
            <div class="modal-dialog  " role="document">
              <div class="modal-content w-100">
                  <CropImage 
                    :srcImage="this.uploadImageUrl"
                    v-on:uploadImage="uploadImage"
                    v-on:closeModal="closeModal"/>
              </div>
            </div>
          </div>
        </div>
      </transition>
    </div>
  </div>

  </div>
</template>

<script>
// controller
import axios from 'axios';
import CropImage from './CropImage.vue';
  export default {
    components:{
      CropImage
    },
    props: {
      userId: {
        type: Number
      }
    },
    data() {
      return {
        fileVideo:null,
        fileImage:null,
        isUploadingVideo:false,
        isUploadedVideo:false,
        isUploadedImage: false,
        isUploadingImage: false,
        form:{
          url_video: null,
          url_image: null,
          title: null,
          tags: null,
          description: null,
          tags: null,
          id_usuario : this.userId
        },
        showModal: false,
        uploadImageUrl: null
      }
    },
    methods: {
      closeModal(){
        this.showModal=false;
      },
      uploadVideo() {
        this.isUploadingVideo = true;
        let formData = new FormData();
        formData.append('file', this.fileVideo);
        var config = {
          onUploadProgress : (progressEvent) => {
            let progress = Math.round( (progressEvent.loaded * 100) / progressEvent.total )
          }
        };

        // You should have a server side REST API 
        axios.post('http://localhost:8020/uploadMedia', formData, {
            'Content-type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          })
        .then(res =>{
          this.isUploadingVideo = false;
          this.isUploadedVideo = true;
        });

      },
      uploadImage(image) {
        console.log(image);
        this.showModal = false;
        this.isUploadingImage = true;

        let formData = new FormData();
        formData.append('file', image);
        var config = {
          onUploadProgress : (progressEvent) => {
            let progress = Math.round( (progressEvent.loaded * 100) / progressEvent.total )
          }
        };

        // You should have a server side REST API 
        axios.post('http://localhost:8020/uploadMedia', formData, {
            'Content-type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          })
        .then(res =>{
          this.isUploadingImage = false;
          this.isUploadedImage = true;
        });

      },
      handleFileUpload() {
        
        this.fileVideo = this.$refs.fileVideo ? this.$refs.fileVideo.files[0] : null;
        this.fileImage = this.$refs.fileImage ? this.$refs.fileImage.files[0] : null;

      }
    
    },
    watch:{
      'fileVideo':function(file){
        if(file){
          this.uploadVideo();
        }
      },
      'fileImage':function(file){
        if(file){
          console.log('fichero que se abre con el explorador : ' + JSON.stringify(file));
          this.uploadImageUrl = URL.createObjectURL(file);
          this.showModal=true;
        }
      }
    }

  }
</script>