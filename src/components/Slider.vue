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

      <!--Slide with blank fluid image to maintain slide aspect ratio -->
      <b-carousel-slide id="itemCarrusel" class="p-1" v-for="info in infos" :key="info.id">
          <template v-slot:img >
            <figure class="imagenvideo col-md-3 d-md-inline-block"  >
                <img v-bind:src="info.url1" class="img-fluid">
            </figure>                   
            <figure class="imagenvideo col-md-3 d-md-inline-block">
                <img v-bind:src="info.url2" class="img-fluid">
            </figure>     
            <figure class="imagenvideo col-md-3 d-md-inline-block">
                <img v-bind:src="info.url3" class="img-fluid">
            </figure>                         
            <figure class="imagenvideo col-md-3 d-md-inline-block">
                <img v-bind:src="info.url4" class="img-fluid">
            </figure>        
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
              id:0,
              url1:'',
              url2:'',
              url3:'',
              url4:'',
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
              var cont = 0;
              var contWrapper = 0;
              var finalResponse = new Array();
              dataList.forEach( function( element, index ){
                if(cont%4==0){
                  finalResponse.push({
                    id: contWrapper,
                    url1: dataList[cont].download_url,
                    url2: dataList[cont+1].download_url,
                    url3: dataList[cont+2].download_url,
                    url4: dataList[cont+3].download_url,
                    author: element.author
                  });
                  contWrapper++;
                }
                cont++;

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
