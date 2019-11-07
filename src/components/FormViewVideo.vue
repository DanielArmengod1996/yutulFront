<template>
    <!--
    <div class="col-md-3 col-md-offset-3">
      <video style="height: 400px; width:600px;" id="videoPlayer" controls muted="muted" autoplay>
        <source src="http://localhost:8050/getvideo" type="video/mp4">
      </video>
    </div>
    -->
  <b-embed type="video" aspect="4by3" controls poster="">
    <source src="http://localhost:8050/getvideo" type="video/mp4">
  </b-embed>
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
              'Content-Type': 'multipart/form-data'
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
