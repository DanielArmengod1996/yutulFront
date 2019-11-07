<template>

  <div id="UploadBox"> 
    <h2>Video Uploader</h2>
    {{this.uploadPercentage}}
    <form>
      <input  type="file" id="file" name="file" ref="file" accept="video/*" v-on:change="handleFileUpload()">
      <button type="button" v-on:click="uploadForm"> Upload</button>
    </form>
  </div>
</template>

<script>
// controller
import axios from 'axios';

  export default {
    data() {
      return {
        file:new Blob(),
        uploadPercentage:0
      }
    },
    methods: {
      uploadForm() {
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
          alert('sucessfully called');
        });

      },
      handleFileUpload() {
        
        this.file = this.$refs.file.files[0];
        console.log(JSON.stringify( this.$refs.file.files[0] ) );
        console.log('>>>> 1st element in files array >>>> ', this.file);
      }
    
    }

  }
</script>
