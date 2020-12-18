<template>
  <div class="box" @scroll="handleScroll">

    <div class="row">
        <div v-for="info in infos" :key="info.id" class="col-md-3 col-6 my-3">
            <button class="card h-100" v-on:click="selectVideo(info)">
                <img :src="info.url" class="card-img-top">
                <div class="card-body">
                    <div class="card-title">{{ info.tituloVideo }}</div>
                    <div>
                        <span class="badge badge-pill badge-info">${{ info.tituloVideo }}</span>
                    </div>
                </div>
            </button>
        </div>
    </div>

  </div>
    
</template>



<script>
    export default {
      data() {
        return {
          window:{
            width: 0,
            heigh: 0
          },
          infos :[
            {
              id:0,
              tituloVideo:'',
              localizacionVideo:'',
              descripcionVideo:'',
              url:''
            }
          ],
          limitVideos: 50 
        }
      },
      name: 'ListVideos',
      methods: { 
        /** this method is triggered when the video is choosed */
        selectVideo: function( videoData ){
            this.$emit('verVideo', videoData);
        },
        /*this triggered when the user top the window*/ 
        handleScroll: function(evt){
            if((evt.srcElement.offsetHeight + evt.srcElement.scrollTop) >= evt.srcElement.scrollHeight-100){
                this.limitVideos += 4;
                //console.log('final');
                alert('TOP');
                this.axiosGetVideos();
                
            }
        },
        // axios rest callout
        axiosGetVideos(){
          const axios = require('axios');
          var url = `http://localhost:8020/getRandomImages`;
          var promise = new Promise(function(resolve, reject){
            axios.get(url)
            .then(function(response, error){
              if(error){
                //console.log(error);
              }
              // handle sucess
              var dataList  = response.data;
              
              var finalResponse = new Array();
              dataList.result.forEach( function( index, i ){
                
                finalResponse.push({
                id: i,
                tituloVideo: index.nombre,
                url:"http://localhost:8020/getImage?name=" + index.localizacionImage,
                localizacionVideo : index.localizacionVideo,
                descripcionVideo : index.description
                });

              });
              resolve(finalResponse);

            })
            .catch(function(error){
              // handle error
              reject(error);
              
            })
            .finally(/*function(data)*/()=>{
            });
            //
          });
          var listVideos = promise.then(response => (this.infos = response));
          this.infos.push(listVideos);
        },
        handleResize() {
          this.window.width = window.innerWidth;
          this.window.height = window.innerHeight;
          if(window.innerWidth < 1000){
            this.modo4 = false;
          }else{
            this.modo4 = true;
          }
        },
      },
      created(){
        //window.addEventListener('scroll', this.onScroll);
        window.addEventListener('resize', this.handleResize);
        this.handleResize;
      },
      destroyed(){
        //window.removeEventListener('scroll', this.onScroll);
        window.removeEventListener('resize', this.handleResize)
      },
      beforeMount(){
        this.axiosGetVideos();
      },
      mounted(){
        this.scroll()
      },
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
