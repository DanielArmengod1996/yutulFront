<template>
  <div class="card mx-xl-5">

    <!-- Card body -->
    <div class="card-body">

        <!-- Default form subscription -->
        <form>
            <p class="h4 text-center py-4">Subscribe</p>

              <!--input para mostrar los videos, solo se muestra cuando el usuario esta por subir el video-->
              <b-form-file class="py-4" v-if="!isUploaded && !isUploading" accept="video/*" v-model="file" :state="Boolean(file)" placeholder="Drop the video here ..." drop-placeholder="Drop file here..."></b-form-file>
              <!-- logging que se mostrará cuando el usuario quiera guardar los cambios y subir video-->
              <b-button variant="primary" disabled v-if="isUploading">
                <b-spinner small type="grow"></b-spinner>
                Uploading...
              </b-button>
            <br>

            <!-- Default input email -->
            <label for="defaultFormCardEmailEx" class="grey-text font-weight-light">Your email</label>
            <input type="email" id="defaultFormCardEmailEx" class="form-control">
            <label for="defaultFormCardEmailEx" class="grey-text font-weight-light">Password</label>
            <input type="password" id="defaultFormCardEmailEx" class="form-control">

            <div class="text-center py-4 mt-3">
                <button class="btn btn-outline-purple" type="submit">Send<i class="far fa-paper-plane ml-2"></i></button>
            </div>
        </form>
        <!-- Default form subscription -->

    </div>
    <!-- Card body -->

  </div>
</template>

<script>
// controller
import axios from 'axios';

  export default {
    data() {
      return {
        file:null,
        isUploading:false,
        isUploaded:false
      }
    },
    methods: {
      uploadVideo() {
        this.isUploading = true;
        let formData = new FormData();
        formData.append('file', this.file);
        console.log('>> formData >> ', formData);
        var config = {
          onUploadProgress : (progressEvent) => {
            let progress = Math.round( (progressEvent.loaded * 100) / progressEvent.total )
            console.log(progress);
          }
        };

        // You should have a server side REST API 
        axios.post('http://localhost:8020/uploadVideo', formData, {
            'Content-type': 'multipart/form-data',
            'Access-Control-Allow-Origin': '*'
          })
        .then(res =>{
          this.isUploading = false;
          this.isUploaded = true;
        });

      },
      handleFileUpload() {
        
        this.file = this.$refs.file.files[0];
        console.log(JSON.stringify( this.$refs.file.files[0] ) );
        console.log('>>>> 1st element in files array >>>> ', this.file);
      }
    
    },
    watch:{
      'file':function(file){
        if(file){
          this.uploadVideo();
        }
      }
    }

  }
</script>
