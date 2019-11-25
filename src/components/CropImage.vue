<template>
    <div>
        
        <div class="modal-header">
        <h5 class="modal-title">editor de portada</h5>
        <button type="button" class="close" data-dismiss="modal" aria-label="Close">
            <span aria-hidden="true" @click="closeModal">&times;</span>
        </button>
        </div>
        <div class="modal-body">
            <clipper-fixed ref="clipper" ratio="1.50" :src="this.srcImage" preview="preview">
                <div slot="placeholder">No image</div>
            </clipper-fixed>
             <clipper-preview  name="preview"></clipper-preview>
        </div>
        <div class="modal-footer">
            <b-container class="bv-example-row">
                <b-row>
                        <button type="button" class="btn btn-primary" @click="uploadImage">Save changes</button>
                        <button type="button" class="btn btn-secondary" @click="closeModal" style="{display:inline-block}">Cancel</button>
                </b-row>
            </b-container>
        </div>
    </div>
</template>

<script>
import Vue from 'vue'
import VueRx from 'vue-rx'
// install vue-rx
Vue.use(VueRx);

import { clipperFixed,clipperPreview   } from 'vuejs-clipper'

export default {
    components:{
        clipperFixed ,
        clipperPreview
    },
    props:{
        srcImage: {
            Type: String
        }

    },
    methods:{
        uploadImage(){
            const canvas = this.$refs.clipper.clip();//call component's clip method
            var image = new Image();
            image.src = canvas.toDataURL("image/png");


            this.$emit('uploadImage', image);
        },
        closeModal(){
            this.$emit('closeModal');
        }
    }
    
}
</script>