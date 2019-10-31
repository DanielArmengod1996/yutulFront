<template>
  <div>
    <input class="form-control mx-auto w-50 p-3 mt-5" type="text" placeholder="Search" aria-label="Search" width="20px"/>

    <!--Slide with blank fluid image to maintain slide aspect ratio -->
      <div class="mt-4" v-for="info in infos" :key="info.id">
        <b-card-group deck>
          <b-card img-alt="Card image">
            <b-img thumbnail fluid :src="info.url1" img-alt="Card image" img-bottom/>
            <b-card-text>
              {{info.author1}}
            </b-card-text>
          </b-card>

          <b-card img-alt="Card image">
            <b-img thumbnail fluid :src="info.url2" img-alt="Card image" img-bottom/>
            <b-card-text>
              {{info.author2}}
            </b-card-text>
          </b-card>
          <b-card img-alt="Card image">
            <b-img thumbnail fluid :src="info.url3" img-alt="Card image" img-bottom/>
            <b-card-text>
              {{info.author3}}
            </b-card-text>
          </b-card>
          <b-card img-alt="Card image">
            <b-img thumbnail fluid :src="info.url4" img-alt="Card image" img-bottom/>
            <b-card-text>
              {{info.author4}}
            </b-card-text>
          </b-card>
        </b-card-group>
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
              author1:'',
              url1:'',
              author2: '',
              url2:'',
              author3: '',
              url3:'',
              author4: '',
              url4:''
            }
          ],
          limitVideos: 20 
        }
      },
      methods: { 
        // axios rest callout
        axiosGetVideos(){
          const axios = require('axios');
          var url = `https://picsum.photos/v2/list?page=2&limit=${this.limitVideos}`;
          var promise = new Promise(function(resolve, reject){
            axios.get(url)
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
                    author1: dataList[cont].author,
                    url1: dataList[cont].download_url,
                    author2: dataList[cont+1].author,
                    url2: dataList[cont+1].download_url,
                    author3: dataList[cont+2].author,
                    url3: dataList[cont+2].download_url,
                    author4: dataList[cont+3].author,
                    url4: dataList[cont+3].download_url,
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
          var listVideos = promise.then(response => (this.infos = response));
          this.infos.push(listVideos);

          

        },
        handleResize() {
          this.window.width = window.innerWidth;
          this.window.height = window.innerHeight;
          if(window.innerWidth < 800){
            this.modo4 = false;
          }else{
            this.modo4 = true;
          }
        },
        /*this only triggered when the user stay at top of the window*/ 
        scroll(){
          window.onscroll = () =>{
            let bottomOfWindow = Math.max(window.pageYOffset, document.documentElement.scrollTop, document.body.scrollTop) + window.innerHeight >= document.documentElement.offsetHeight+16;
            if(bottomOfWindow){
              this.limitVideos += 4;
              console.log('final');
              this.axiosGetVideos();
            }
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
      },
      mounted(){
        this.scroll()
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
