<template>
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="8">
          <h2>Title Video Here</h2>
        </b-col>
        <b-col>3 of 3</b-col>
      </b-row>
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
  import axios from 'axios';

// controller
  export default {
    data() {
      return{

      }
    },
    methods: {
      uploadForm() {
        let formData = new FormData();
        formData.append('file', this.file);

        // You should have a server side REST API 
        axios.post('http://localhost:8050/uploadVideo',
          formData, {
            headers: {
              'Content-Type': 'multipart/form-data'
            }
          }
        ).then(function () {
          //do somethiing when the video is loaded
        })
        .catch(function () {
          // throw the error
        });
      },

    
    },


  }
</script>
