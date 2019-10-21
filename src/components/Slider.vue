<template>
  <div id="carrusel">
    <b-carousel
      id="carousel-1"
      v-model="slide"
      controls
      background="#ababab"
      img-width="2000"
      img-height="500"
      style="text-shadow: 1px 1px 2px #333;"
      @sliding-start="onSlideStart"
      @sliding-end="onSlideEnd"
    >

      <!-- Slide with blank fluid image to maintain slide aspect ratio -->
      <b-carousel-slide id="itemCarrusel" class="p-1" >
          <template v-slot:img v-for="(value, name, index) in infos">
            <div v-if="index%4!=0">
              <figure class="imagenvideo col-md-3 d-md-inline-block" >
                  <img v-bind:src="value.url" class="img-fluid">
              </figure>
            </div>
          </template>
      </b-carousel-slide>
      
    </b-carousel>

    <div v-if="Math.random() > 0.5">
      Now you see me
    </div>
    <div v-else>
      Now you don't
    </div>
    
    
    <!--<p class="mt-4">
      Slide #: {{ slide }}<br>
      Sliding: {{ sliding }}
    </p>-->
  </div>
    
</template>

<script>
    export default {
      data() {
        return {
          slide: 0,
          sliding: null,
          window:{
            width: 0,
            heigh: 0
          },
          infos :[
            {
              id:'',
              url:'',
              author:''
            }
          ]
        }
      },
      methods: { 
        // axios rest callout
        axiosGetVideos(){
          const axios = require('axios');
          var promise = new Promise(function(resolve, reject){
            axios.get('https://picsum.photos/v2/list?page=2&limit=60')
            .then(function(response, error){
              // handle sucess
              var dataList  = response.data;
              var finalResponse = new Array();
              dataList.forEach( function( element, index ){
                
                finalResponse.push({
                  id: element.id,
                  url: element.download_url,
                  author: element.author
                });

              });
              resolve(finalResponse);

            })
            .catch(function(error){
              // handle error
              reject(error);
              
            })
            .finally(function(data){
              
            });
            //
          });
          this.infos = promise.then(response => (this.infos = response));
          //
          

        },
        // eslint-disable-next-line
        onSlideStart(slide) {
            this.sliding = true
        },//
        // eslint-disable-next-line
        onSlideEnd(slide) {
            this.sliding = false
        },
        handleResize() {
          this.window.width = window.innerWidth;
          this.window.height = window.innerHeight;
          if(window.innerWidth < 768){
            alert('cambio');
          }
        }
      },
      created(){
        window.addEventListener('resize', this.handleResize);
        this.handleResize;
      },
      destroyed(){
        window.removeEventListener('resize', this.handleResize)
      },
      beforeMount(){
        this.axiosGetVideos();
      }
    }
</script>

<style>
.figure{
  min-width: none;
}
.carousel-control-next{
    right: 0;
    width: 5vmin !important;
    margin: 0px;
    background-color: red;
}
.carousel-control-prev {
    right: 0;
    width: 5vmin !important;
    margin: 0px;
    background-color: red;
}
</style>
