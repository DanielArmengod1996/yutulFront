<template>

  <div id="UploadBox"> 
    <h2>Video Uploader</h2>
    <form method="post" action="http://localhost:8050/uploadVideo" enctype="multipart/form-data">
      <input  type="file" id="file" name="file" ref="file" accept="video/*" v-on:change="handleFileUpload()">
      <button type="submit"> Upload</button>
    </form>
  </div>
</template>

<script>
// controller
  export default {
    data() {
      return {
        file:new Blob()
      }
    },
    methods: {
      uploadForm() {
        alert(this.file);
        const axios = require('axios');
        let formData = new FormData();
        alert(JSON.stringify(this.file));
        formData.append('file', this.file);
        console.log('>> formData >> ', formData);

        // You should have a server side REST API 
        axios.post('http://localhost:8050/uploadVideo',
          formData, {
            headers: {
              'Content-Type': 'video/mp4'
            }
          }
        ).then(function () {
          console.log('SUCCESS!!');
        })
        .catch(function () {
          console.log('FAILURE!!');
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
