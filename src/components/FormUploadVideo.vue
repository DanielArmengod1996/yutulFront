<template>
    <b-container class="bv-example-row">
      <b-row>
        <b-col>
          <h2>Video Uploader</h2>
        </b-col>
      </b-row>

      <b-row class="mt-5">
        <b-col>
        </b-col>
        <b-col>
          <!--input para mostrar los videos, solo se muestra cuando el usuario esta por subir el video-->
          <b-form-file @focus="startUploadingVideo($event)" @change="startUploadingVideo($event)" v-if="!isUploaded && !isUploading" accept="video/*" v-model="file" :state="Boolean(file)" placeholder="Drop the video here ..." drop-placeholder="Drop file here..."></b-form-file>
          <!-- logging que se mostrará cuando el usuario quiera guardar los cambios y subir video-->
          <b-button variant="primary" disabled v-if="isUploading">
            <b-spinner small type="grow"></b-spinner>
            Uploading...
          </b-button>
        </b-col>
        <b-col>

        </b-col>
        
      </b-row>
      <b-row class="mt-3">
        <b-col>
          <b-button   type="button" class="btn-lg">Save Changes</b-button>
        </b-col>
      </b-row>
    </b-container>
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
      },
      startUploadingVideo(event){
        this.file = event.target.files[0];
        if(this.file){
          this.uploadVideo();
        }
      }
    
    }

  }
</script>
