<template>
    <b-container class="bv-example-row">
      <b-row>
        <b-col cols="8">
          <h2>{{videoData.tituloVideo}}</h2>
        </b-col>
        <b-col>videos relacionados</b-col>
      </b-row>
      <b-row>
        <b-col cols="8">
          <b-embed type="video" controls allowfullscreen width="500">
            <source v-bind:src="'http://localhost:8020/getvideo?urlVideo=' + videoData.localizacionVideo"  type="video/mp4">
          </b-embed>
        </b-col>
        
      </b-row>
            
      <b-row class="mt-4" >
        <b-col cols="8"  >{{videoData.descripcionVideo}}</b-col>

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
    props: {
        videoData: Object,
    },
    data() {
      return{
          urlVideo :''
      }
    },
    methods: {
      uploadForm() {
        let formData = new FormData();
        formData.append('file', this.file);

        // You should have a server side REST API 
        axios.post('http://localhost:8020/uploadVideo',
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
      beforeMount(){
          this.urlVideo = 'http://localhost:8020/getvideo?urlVideo=' + this.videoData.localizacionVideo;  
      }

    
    },


  }
</script>
