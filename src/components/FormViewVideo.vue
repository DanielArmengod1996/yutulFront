<template>
    <!--
    <div class="col-md-3 col-md-offset-3">
      <video style="height: 400px; width:600px;" id="videoPlayer" controls muted="muted" autoplay>
        <source src="http://localhost:8050/getvideo" type="video/mp4">
      </video>
    </div>
    -->
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="8">
          <b-embed type="video" controls allowfullscreen width="500">
            <source src="http://localhost:8020/getvideo" type="video/mp4">
          </b-embed>
        </b-col>
        <b-col>3 of 3</b-col>
      </b-row>
      <b-row class="mt-4">
        <b-col cols="8">
          comentarios
        </b-col>
        <b-col>
        </b-col>
      </b-row>
    </b-container>

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
